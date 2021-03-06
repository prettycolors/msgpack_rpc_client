# MsgpackRpcClient [![CircleCI](https://circleci.com/gh/brightbytes/msgpack_rpc_client.svg?style=svg)](https://circleci.com/gh/brightbytes/msgpack_rpc_client)

## A lean Ruby client for the MessagePack-RPC protocol

Use this gem to achieve reliable, fault-tolerant RPC with your microservices.

### Differences from the "official" implementation (the msgpack-rpc gem)

The official implementation:

* depends on the cool.io gem, labeled "retired" [on the homepage](https://coolio.github.io).
* does not re-establish connections.
* designed to be asynchronous
* displayed instability under high load in production
* does not support JRuby

This implementation:

* has no dependencies but the msgpack gem
* is under 200 lines in one class
* automatically re-establishes connections in the case of inevitable network errors, service restarts, deploys, and so on.
* supports SSL
* threadsafe
* reliable
* high load tested
* used in production, with up to a 1000 requests per second in a single frontend request.
* supports MRI from version 1.9.3 - for those who have a huge legacy app that you're dying to factor into microservices!
* supports JRuby (but see installation notes)

However, this implementation **does NOT support asynchronous calls** - if you require this feature, it is not for you. However, from my experience, almost no Ruby applications require asynchronous communication.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'msgpack_rpc_client'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install msgpack_rpc_client

### JRuby

There was a bug in the jruby msgpack implementation that was fixed in [this PR](https://github.com/msgpack/msgpack-ruby/pull/125). Until it's released as a stable version (>1.0.0), you need to use the HEAD version of the `msgpack` gem:

```ruby
gem 'msgpack_rpc_client'
# TODO remove once msgpack v1.0.1 or later is released
gem 'msgpack', git: 'https://github.com/msgpack/msgpack-ruby.git'
```

## Usage

``` ruby
require 'msgpack_rpc_client'

client = MsgpackRpcClient.new(host: '127.0.0.1', port: 12345)
response = client.call('HelloWorld', name: 'Ruby')
# You should re-use the client for all subsequent calls to the same server.
# It's never necessary to re-connect, and the client is threadsafe.
```

See the `examples` directory for a complete server-client example.

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/brightbytes/msgpack_rpc_client.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

