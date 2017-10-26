# emq-router

This module a router for use with MQTT (EMQ) subscriptions.

## Installation

```
npm install emq-router
```

# TLDR

If you have just started with [MQTT](https://github.com/adamvr/MQTT.js) the first thing you will notice is there is only callback registered for on Message,
even though you can register multiple subscriptions.. It is therefore up to you the developer to route these to the
correct handler, which is why I wrote this library.

I have added a simple override for the topic subscription to enable named params, really this is to avoid the
inevitable tokenising of the topic which I do every time I build complex topic structures.

*NOTE:* I will need to revisit this with some more validation, but for now it works for my simple requirements.


# usage

```javascript
var mqtt = require('mqtt')
  , mqttrouter = require('emq-router');

var settings = {
  reconnectPeriod: 5000
};

// client connection
var client = mqtt.connect('mqtt://localhost', settings);

// enable the subscription router
var router = mqttrouter.wrap(client);

// subscribe to messages for 'hello/me'
router.subscribe('hello/me', function(topic, message){
  console.log('received', topic, message);
});

// subscribe to messages for 'hello/you'
router.subscribe('hello/you', function(topic, message){
  console.log('received', topic, message);
});

// subscribe to messages for 'some/+/you' with a named param for that token
router.subscribe('some/+:person/you', function(topic, message, params){
  console.log('received', topic, message);
});

```

One thing to note is that subscriptions are refreshed on reconnect, the status of the connection is also
exposed via the `isConnected` method.

## License
Copyright (c) 2017 Daniel Chang
Licensed under the MIT license.
