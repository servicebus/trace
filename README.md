# @servicebus/trace
[![Build Status](https://travis-ci.org/servicebus/trace.svg?branch=master)](https://travis-ci.org/servicebus/trace)
[![codecov](https://codecov.io/gh/servicebus/trace/branch/master/graph/badge.svg)](https://codecov.io/gh/servicebus/trace)

Middleware to publish message receipt information to a central store, for message tracking and tracing purposes.

## middleware

Set up the @servicebus/trace middleware as follows:
```
var config = require('cconfig')();
var servicebus = require('servicebus');
var trace = require('@servicebus/trace');

var bus = servicebus.bus({
  url: config.RABBITMQ_URL
});

bus.use(trace({
  serviceName: 'my-service-name',
  store: new trace.RedisStore({
    host: config.REDIS_HOST || 'localhost',
    port: config.REDIS_PORT || 6379
  })
}));

module.exports = bus;
```
At this moment, only the RedisStore is available. 

### @servicebus/trace utility

Install @servicebus/trace globally to allow using the `@servicebus/trace` utility. 
```
npm install -g @servicebus/trace
@servicebus/trace
```
will display service middleware trace information from your local redis instance similar to below:
```
  @servicebus/trace
┌─────┬───────────────────────────────────┬──────────────────────────────┬──────────────────────────────┬──────────────────────────────┬───────────┬──────────────────────────────────────────┐
│ #   │ correlationId / cid               │ service name                 │ queue / routingKey           │ type                         │ direction │ date                                     │
├─────┼───────────────────────────────────┼──────────────────────────────┼──────────────────────────────┼──────────────────────────────┼───────────┼──────────────────────────────────────────┤
│ 1   │ patient-sunset-4ybOWdDse          │ test-service                 │ test.queue                   │ bla                          │ inbound   │ Sat Aug 31 48120 11:17:46 GMT-0400 (EDT) │
├─────┼───────────────────────────────────┼──────────────────────────────┼──────────────────────────────┼──────────────────────────────┼───────────┼──────────────────────────────────────────┤
│ 2   │ patient-sunset-4ybOWdDse          │ test-service                 │ test.queue                   │ bla                          │ outbound  │ Sat Aug 31 48120 11:17:41 GMT-0400 (EDT) │
└─────┴───────────────────────────────────┴──────────────────────────────┴──────────────────────────────┴──────────────────────────────┴───────────┴──────────────────────────────────────────┘
```
