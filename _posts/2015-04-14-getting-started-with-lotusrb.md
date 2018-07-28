---
layout: post
title: "A quick look at Lotus"
description: ""
category: programming
tags: [lotusrb, lotus, ruby]
---

I've been using rails at work for just over a year now, and have dabbled with sinatra at home. Since we're probably going to be looking at the web application framework [Lotus](http://lotusrb.org/) at tomorrow's wmrug, I figured I'd get a head start and see what it's like.

It's my understanding that Lotus fills part of the gap between sinatra's bare bones flexibility, and rails' opinionated feature-packedness. This post covers my experience getting up and running with Lotus 0.3.0.

<h3>Hitting the Ground</h3>

Creating a new Lotus project is pretty simple.

<pre>
$ lotus new hello
$ cd hello
$ tree
.
├── apps
│   └── web
│       ├── application.rb
│       ├── config
│       │   ├── mapping.rb
│       │   └── routes.rb
│       ├── controllers
│       │   └── home
│       │       └── index.rb
│       ├── public
│       │   ├── javascripts
│       │   └── stylesheets
│       ├── templates
│       │   ├── application.html.erb
│       │   └── home
│       │       └── index.html.erb
│       └── views
│           ├── application_layout.rb
│           └── home
│               └── index.rb
├── config
│   └── environment.rb
├── config.ru
├── db
├── Gemfile
├── lib
│   ├── config
│   │   └── mapping.rb
│   ├── hello
│   │   ├── entities
│   │   └── repositories
│   └── hello.rb
├── Rakefile
└── spec
    ├── features_helper.rb
    ├── hello
    │   ├── entities
    │   └── repositories
    ├── spec_helper.rb
    ├── support
    └── web
        ├── controllers
        ├── features
        └── views

28 directories, 16 files
</pre>

The command generates a whole bunch of files and folders. If you've used rails, most of them will be pretty familiar. Various configuration gubbins, our controllers templates, gemfile and assets. There's also a `spec/` folder for our lovely tests, which use Minitest by default.

After doing a bundle install, we can simple run `bundle exec lotus server` and we have our hello world page accessible on port 2300, which prompts us with the following:

<blockquote>
	Please edit apps/web/config/routes.rb to add your first route.
</blockquote>

As explained in the routes file, the line

{% highlight ruby %}
get '/', to: 'home#index'
{% endhighlight %}

is expected to map to

{% highlight ruby %}
`Web::Controllers::Home::Index`
{% endhighlight %}

in `apps/web/controllers/home/index.rb`

<h3>Architecture</h3>

It's interesting to see how the structure here differs to rails. In rails, a controller like `UsersController` is a class and actions within it are represented by methods. In Lotus, a controller is represented by a namespaced module, and each <em>action</em> is represented by a class. An immediate advantage of this is that your controllers are naturally split across multiple files, allowing you to organise code more easily.

The corresponding view code and template for this action is stored in `apps/web/views/home/index.rb` and `apps/web/templates/home/index.html.erb`. Nice, familiar erb.

Now that we're up and running with the basic application, it's worth giving the [architectures section of the README](https://github.com/lotus/lotus#architectures) another, closer look.

The main body of a lotus application is split between the `apps/` folder and the `lib/` folder. `apps/` contains the controllers and views, as you might expect. However, the models and the bulk of the actual <em>logic</em> seem to live in the `lib/` folder.

<pre>
├── lib
│   ├── config
│   │   └── mapping.rb
│   ├── hello
│   │   ├── entities
│   │   └── repositories
│   └── hello.rb
</pre>

You may notice that even though we named our application <strong>hello</strong>, there is a folder nested within `lib/` named `hello/` and a folder in `apps/` called `web/`.

This is because Lotus encourages you to build one or more of your <strong>applications</strong>, e.g. API, web front end, etc. under the `apps/` folder. Folders in `lib/` represent <strong>use cases</strong> or <strong>features</strong>. You can tidily store many different applications in the same folder, giving them access to the same back end code. There appears to be a heavy emphasis on modular, reusable code, avoiding the pitfall of accidentally making your application monolithic and difficult to maintain, as it can sometimes be all to easy to do in rails.

This has only been a quick look, but I'm very interested in the code organisation possibilities that Lotus provides, and I'm excited to play with it more tomorrow and see how it handles databases.