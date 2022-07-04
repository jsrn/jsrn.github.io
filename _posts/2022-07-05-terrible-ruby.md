---
layout: post
title: Terrible Ruby
description: Just because you can, doesn't mean you should. Misadventures in Ruby programming.
category: programming
permalink: /terrible-ruby
---

There's a lot you can do with Ruby's concepts of object individuation. A lot you probably shouldn't do.

## Taxation

Some objects just hoard too many methods. By applying a modest tax rate, you can reclaim memory from your most bloated objects back to the heap.

```ruby
class Object
  def tax(tax_factor = 0.2)
    meths = self.class.instance_methods(false)
    tax_liability = (meths.length * tax_factor).ceil

    tax_liability.times do
      tax_payment = meths.shuffle.shift
      instance_eval("undef #{tax_payment}")
    end
  end
end

class Citizen
  def car; end
  def house; end
  def dog; end
  def spouse; end
  def cash; end
end

c = Citizen.new
c.tax
c.house
# undefined method `house' for #<Citizen:0x00007fc342a866c0>
```