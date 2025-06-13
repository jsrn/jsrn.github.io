---
layout: tcd
id: "0005"
title: Ruby memory analysis
authored: "2025-06-12"
updated:  "XXXX-XX-XX"
---

It is natural to want to profile the memory consumption of Ruby code. This communication describes techniques for doing so on a MacOS system.

* Do not remove this line (it will not be displayed)
{:toc}

## Construction of a suitable harness

Using the `rails runner` command provides a suitable harness for all of the below techniques.

```ruby
arr = []

1_000_000.times do
  arr << SecureRandom.hex
end
```

## Using `time -l`

`/usr/bin/time -l rails runner script.rb` gives you some useful stats. The inclusion of the `-l` flag includes the context of the [rusage](https://developer.apple.com/library/archive/documentation/System/Conceptual/ManPages_iPhoneOS/man2/getrusage.2.html) structure.

I've bolded the ones you probably care about.

- **maximum resident set size** in KB.
- average shared memory size
- average unshared data size
- average unshared stack size
- page reclaims
- page faults
- swaps
- block input operations
- block output operations
- messages sent
- messages received
- signals received
- voluntary context switches
- involuntary context switches
- instructions retired
- cycles elapsed
- **peak memory footprint**

## Using the MemoryProfiler gem

The [MemoryProfiler gem](https://github.com/SamSaffron/memory_profiler) gives more detail about what part of the application is actually consuming memory.

It also allows you to single out an individual part of the code for profiling, sidestepping the problem you would face with `time`, where Ruby framework code is included in the measurements.

```ruby
MemoryProfiler.report do
  arr = []

  1_000_000.times do
    arr << SecureRandom.hex
  end
end.pretty_print(detailed_report: true)
```

`rails runner script.rb` outputs the following, which I have edited to display only the top three items from each category.

```plain
Total allocated: 171681156 bytes (2000478 objects)
Total retained:  5000 bytes (60 objects)

allocated memory by gem
-----------------------------------
  91646180  other
  80000000  securerandom-0.4.1
     18080  listen-3.7.1

allocated memory by file
-----------------------------------
  80000000  lib/ruby/gems/3.3.0/gems/securerandom-0.4.1/lib/securerandom.rb
  80000000  <internal:pack>
  11636312  script.rb

allocated memory by location
-----------------------------------
  80000000  lib/ruby/gems/3.3.0/gems/securerandom-0.4.1/lib/securerandom.rb:71
  80000000  <internal:pack>:29
  11636312  script.rb:4

allocated memory by class
-----------------------------------
 160016068  String
  11646992  Array
      4640  Thread::Backtrace::Location

allocated objects by gem
-----------------------------------
   1000055  other
   1000000  securerandom-0.4.1
       258  listen-3.7.1

allocated objects by file
-----------------------------------
   1000000  lib/ruby/gems/3.3.0/gems/securerandom-0.4.1/lib/securerandom.rb
   1000000  <internal:pack>
       248  lib/ruby/gems/3.3.0/gems/listen-3.7.1/lib/listen/record/entry.rb

allocated objects by location
-----------------------------------
   1000000  lib/ruby/gems/3.3.0/gems/securerandom-0.4.1/lib/securerandom.rb:71
   1000000  <internal:pack>:29
        99  lib/ruby/gems/3.3.0/gems/binding_of_caller-1.0.0/lib/binding_of_caller/mri.rb:21

allocated objects by class
-----------------------------------
   2000150  String
       187  Array
        58  Thread::Backtrace::Location

retained memory by gem
-----------------------------------
      4360  listen-3.7.1
       320  lib
       240  other

retained memory by file
-----------------------------------
      3080  lib/ruby/gems/3.3.0/gems/listen-3.7.1/lib/listen/record/entry.rb
      1280  lib/ruby/gems/3.3.0/gems/listen-3.7.1/lib/listen/record.rb
       320  lib/ruby/3.3.0/set.rb

retained memory by location
-----------------------------------
      1600  lib/ruby/gems/3.3.0/gems/listen-3.7.1/lib/listen/record/entry.rb:21
      1040  lib/ruby/gems/3.3.0/gems/listen-3.7.1/lib/listen/record/entry.rb:55
       960  lib/ruby/gems/3.3.0/gems/listen-3.7.1/lib/listen/record.rb:80

retained memory by class
-----------------------------------
      2040  String
      1600  Listen::Record::Entry
      1280  Hash

retained objects by gem
-----------------------------------
        55  listen-3.7.1
         2  lib
         2  other

retained objects by file
-----------------------------------
        47  lib/ruby/gems/3.3.0/gems/listen-3.7.1/lib/listen/record/entry.rb
         8  lib/ruby/gems/3.3.0/gems/listen-3.7.1/lib/listen/record.rb
         2  lib/ruby/3.3.0/set.rb

retained objects by location
-----------------------------------
        20  lib/ruby/gems/3.3.0/gems/listen-3.7.1/lib/listen/record/entry.rb:21
        20  lib/ruby/gems/3.3.0/gems/listen-3.7.1/lib/listen/record/entry.rb:55
         6  lib/ruby/gems/3.3.0/gems/listen-3.7.1/lib/listen/record.rb:80

retained objects by class
-----------------------------------
        31  String
        20  Listen::Record::Entry
         8  Hash

Allocated String Report
-----------------------------------
        26  "lib/ruby/gems/3.3.0/gems/better_errors-2.10.1/lib/better_errors/exception_extension.rb"
        26  lib/ruby/gems/3.3.0/gems/better_errors-2.10.1/lib/better_errors/exception_extension.rb:6

         6  "."
         6  lib/ruby/gems/3.3.0/gems/listen-3.7.1/lib/listen/record/entry.rb:55

         6  ".."
         6  lib/ruby/gems/3.3.0/gems/listen-3.7.1/lib/listen/record/entry.rb:55

Retained String Report
-----------------------------------
         1  "/gitlab/lib/click_house"
         1  lib/ruby/3.3.0/set.rb:512

         1  "/gitlab/lib/gitaly"
         1  lib/ruby/3.3.0/set.rb:512

         1  "/gitlab/lib/gitlab_settings.rb"
         1  lib/ruby/gems/3.3.0/gems/listen-3.7.1/lib/listen/record/entry.rb:26
```

### "Allocated" vs. "retained" memory

```plain
allocated memory by class
-----------------------------------
 160016068  String

retained memory by class
-----------------------------------
      2040  String
```

The String class has allocated a lot of memory, but almost all of it was reclaimed. Let's update our example script to extend the lifetime of our array beyond the profiled block.

```ruby
arr = []

MemoryProfiler.report do
  1_000_000.times do
    arr << SecureRandom.hex
  end
end.pretty_print(detailed_report: true)
```

```plain
allocated memory by class
-----------------------------------
 160015868  String

retained memory by class
-----------------------------------
  80002560  String
```

Now that there are references to those strings outside of the profiled block, they were not able to be collected by the time profiling ended. They are now marked as **retained memory**. This is useful for spotting memory leaks.
