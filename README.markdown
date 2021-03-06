Redis Persistence
=================

`Redis::Persistence` is a lightweight object persistence framework,
fully compatible with _ActiveModel_, based on [_Redis_](http://redis.io),
easily used standalone or within Rails applications.

Installation
------------

    $ gem install redis-persistence

Features:
---------

* 100% Rails compatibility
* 100% ActiveModel compatibility: callbacks, validations, serialization, ...
* No crazy `has_many`-type of semantics
* Auto-incrementing IDs
* Defining default values for properties
* Casting properties as built-in or custom classes
* Convenient "dot access" to properties (<tt>article.views.today</tt>)
* Support for "collections" of embedded objects (eg. article » comments)
* Automatic conversion of UTC-formatted strings to Time objects
* Small: 1 file, ~ 200 lines of code
* Fast: ~2000 saves/sec, ~6000 finds/sec

Basic example
-------------

```ruby
    require 'redis/persistence'

    Redis::Persistence.config.redis = Redis.new
    # => #<Redis client v2.2.2 connected to redis://127.0.0.1:6379/0 (Redis v2.4.1)>

    class Article
      include Redis::Persistence

      property :title
      property :body
      property :author, :default  => '(Unknown)'
      property :created
    end

    Article.create title: 'The Thing', body: 'So, in the beginning...', created: Time.now.utc

    article = Article.find(1)
    # => <Article: {"id"=>1, "title"=>"The Thing", ...}>

    article.title
    # => The Thing!

    article.created.year
    # => 2011

    article.title = 'The Cabin'
    article.save
    # => <Article: {"id"=>1, "title"=>"The Cabin", ...}>
```

See the [`examples/article.rb`](http://github.com/socialinsider/redis-persistence/blob/master/examples/article.rb) for full example.

-----

(c) 2011, Social Insider, s.r.o., released under the MIT License.
