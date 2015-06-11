# Redis stores for Ruby frameworks

__Redis Store__ provides a full set of stores (*Cache*, *I18n*, *Session*, *HTTP Cache*) for modern Ruby frameworks like: __Ruby on Rails__, __Sinatra__, __Rack__, __Rack::Cache__ and __I18n__. It supports object marshalling, timeouts, single or multiple nodes, and namespaces.

Please check the *README* file of each gem for usage and installation guidelines.

## Redis Installation

### Option 1: Homebrew

MacOS X users should use [Homebrew](https://github.com/mxcl/homebrew) to install Redis:

```shell
brew install redis
```

### Option 2: From Source

Download and install Redis from [the download page](http://redis.io//download) and follow the instructions.

## Running tests

```ruby
git clone git://github.com/redis-store/redis-store.git
cd redis-store
gem install bundler
bundle exec rake
```

If you are on **Snow Leopard** you have to run `env ARCHFLAGS="-arch x86_64" ruby ci/run.rb`

### Changing storage strategies

Several months ago Matt Huggins (https://github.com/mhuggins) introduced a change to the redis-store gem to decouple how it marshals data into the redis database. Normally, data is marshalled using Ruby's Marshal class. This strategy is fine when access to the data is only given to Ruby-based application but not for any other kind of application unless it has implemented its own Ruby Marshal parser. With this change, a more universal marshal strategy can be applied like JSON. There are caveats to using different strategies such as JSON not serializing Ruby objects properly so make sure that you are using the strategy that fits your needs.

Example usage:

Each of the following will create a new redis store object that uses the new Marshal adapter

    Redis::Store.new
    Redis::Store.new(:strategy => :marshal)  # symbol shorthand
    Redis::Store.new(:strategy => Redis::Store::Strategy::Marshal)  # actual class
    Redis::Store.new(:strategy => "Redis::Store::Strategy::Marshal")  # string name representing class

Each of the following will create a new redis store object that uses the new Json adapter

    Redis::Store.new(:strategy => :json)   # symbol shorthand
    Redis::Store.new(:strategy => Redis::Store::Strategy::Json)  # actual class
    Redis::Store.new(:strategy => "Redis::Store::Strategy::Json")  # string name representing class

Creating a strategy:

A new strategy simply has to be able to perform two methods: _dump and _load.

    module RedisYamlAdapter
      def self._dump(object)
        ::YAML.dump(object)
      end

      def self._load(string)
        ::YAML.load(string)
      end
    end

    Redis::Store.new(:adapter => RedisYamlAdapter)

## Contributors

  * Matt Horan ([@mhoran](https://github.com/mhoran))

## Status

[![Gem Version](https://badge.fury.io/rb/redis-store.png)](http://badge.fury.io/rb/redis-store) [![Build Status](https://secure.travis-ci.org/redis-store/redis-store.png?branch=master)](http://travis-ci.org/jodosha/redis-store?branch=master) [![Code Climate](https://codeclimate.com/github/jodosha/redis-store.png)](https://codeclimate.com/github/redis-store/redis-store)

## Copyright

2009 - 2013 Luca Guidi - [http://lucaguidi.com](http://lucaguidi.com), released under the MIT license