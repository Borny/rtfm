# Rabbit MQ

- [Queues](#queues)
- [RabbitMQ](#rabbitmq)
- [AMQP](#amqp)
- [Linux install](#linux-install)
- [Send message](#send-message)
- [Work Queue](#work-queue)
- [Acknowledgment](#acknowledgment)
- [Message Durability](#message-durability)

## Queues

## RabbitMQ

Open-source **message-broker software**. It implements the **Advanced Message Queuing Protocol (AMQP)**.
It supports plugins for other protocols:  

- Streaming Text Oriented Messaging Protocol (**STOMP**)
- Message Queuing Telemetry Transport (**MQTT**)

Written in **Erlang Programming Language**.

## AMQP

- [About AMQP](#about-amqp)
- [AMQP Messages](#amqp-messages)
- [Queuing models](#queuing-models)

### About AMQP

RabbitMQ => AMQP Protocol => AMQP Message

AMQP is an open standard application layer protocol.

- Designed for asynchronous communication
- Plateform independent
- Technology independent (many SDKs)

### AMQP Messages

A message has:

- header : Metadata, key-value pair. Defined by AMQP specification
- properties : Metadata, key-value pair. Application-specific information holder
- body : payload. In byte[].

The message limit is 2GB. Message is sent per frames: 131KB bydefault.

### Queuing models

Producer => BROKER (RabbitMQ) => Consumer

The Producer **publishes** the message to the broker.
The Consumer **Consumes** the _message_ and **subscribes** to the _BROKER_

## Linux install

[Install link](https://www.rabbitmq.com/install-debian.html).

To start/stop the service:  
`service rabbitmq-server start/stop`

To check the status:  
`service rabbitmq-server status`

To open the interface in the browser, first run:  
`rabbitmq-plugins enable rabbitmq_management`
Then open the browser on:  
`127.0.0.1:15672`  

Use the following credentials to login for the first time locally:

- user : guest
- password : guest

## Send message

Producer => Queue => Consumer

### Install AMQP

Create a folder, `cd` into it and run `npm install amqplib` to install the amqp client for NodeJS.

Creating a send file:

```javascript
// ./send.js

// Import the amqp library
const amqp = require('amqplib/callback_api');

// Connect to the RabbitMQ server
amqp.connect('amqp://localhost', (error0, connection) => {
  if(error0){
    throw error0
  }
  
  // Create a channel
  connection.createChannel((error1, channel) => {
    if(error1){
      throw error1
    }
    const queue = 'hello' // Name of the queue
    const msg = 'Hello World' // Message to send

    // Declaring a queue to send the message to
    channel.assertQueue(queue, {
      durable: false
    })

    // Publishing the message to the queue
    channel.sendToQueue(queue, Buffer.from(msg))

    console.log('[x] Sent %s', msg);
  })

  setTimeout(() => {
    connection.close()
    process.exit(0)
  }, 500)
})
```

Creating a receive file

```javascript
// ./receive.js

// Import the amqp library
const amqp = require('amqplib/callback_api');

// Connect to the RabbitMQ server
amqp.connect('amqp://localhost', (error0, connection) => {
  if (error0) {
    throw error0
  }

  // Create a channel
  connection.createChannel((error1, channel) => {
    if (error1) {
      throw error1
    }

    const queue = 'hello' // Name of the queue

    // Declaring a queue to get the message from
    channel.assertQueue(queue, {
      durable: false,
    })

    console.log(' [*] Waiting for messages in %s. To exit press CTRL+C', queue)
    
    // Consuming the message in the queue
    channel.consume(queue, (msg) => {
      console.log(' [x] Received %s', msg.content.toString())
    }, { noAck: true })
  })
})
```

Run both files: `node send.js` and `node receive.js`.

## Work Queue

A **Work Queues** or **Task Queues** are used to distribute time-consuming tasks among multiple workers.

```javascript
// ./new-task.js

// Import the amqp library
const amqp = require('amqplib/callback_api');

// Connect to the RabbitMQ server
amqp.connect('amqp://localhost', (error0, connection) => {
  if(error0){
    throw error0
  }
  
  // Create a channel
  connection.createChannel((error1, channel) => {
    if(error1){
      throw error1
    }
    const queue = 'task_queue'; // Name of the queue
    const msg = process.argv.slice(2).join(' ') || "Hello World!"; // Message to send

    // Declaring a queue to send the message to
    channel.assertQueue(queue, {
      durable: true
    })

    // Publishing the message to the queue
    channel.sendToQueue(queue, Buffer.from(msg), {
      persistent: true
    })
    console.log('[x] Sent %s', msg);
  })

  setTimeout(() => {
    connection.close()
    process.exit(0)
  }, 500)
})
```

```javascript
// ./receive.js

// Import the amqp library
const amqp = require('amqplib/callback_api');

// Connect to the RabbitMQ server
amqp.connect('amqp://localhost', (error0, connection) => {
  if (error0) {
    throw error0
  }

  // Create a channel
  connection.createChannel((error1, channel) => {
    if (error1) {
      throw error1
    }

    const queue = 'task_queue' // Name of the queue

    // Declaring a queue to get the message from
    channel.assertQueue(queue, {
      durable: true,
    })

    console.log(' [*] Waiting for messages in %s. To exit press CTRL+C', queue)
    
    // Consuming the message in the queue
    channel.consume(queue,  (msg) => {
      const secs = msg.content.toString().split('.').length - 1;

      console.log(" [x] Received %s", msg.content.toString());
      setTimeout(function () {
        console.log(" [x] Done");
      }, secs * 1000);
    }, {
      // automatic acknowledgment mode,
      // see ../confirms.html for details
      noAck: false // will prevent the message from being deleted if the worker/task stops or crashes
    });
  })
})
```

Open three terminals:

- shell one : `node worker.js`
- shell two : `node worker.js`
- shell three :  `node new-task.js first message..` then `node new-task.js second message...`

Shell one will receive **first message** and shell 2 will receive **second message**.  
The messages are distributed evenly accross all instances of consumers: this is called **round-robin**.

## Acknowledgment

Add **noAck: false** to make sure the message is not lost if the worker crashes.

```javascript
channel.consume(queue, (msg) => {
  const secs = msg.content.toString().split('.').length - 1;

  console.log(" [x] Received %s", msg.content.toString());
  setTimeout(function () {
    console.log(" [x] Done");
  }, secs * 1000);
}, {
  // automatic acknowledgment mode,
  // see ../confirms.html for details
  noAck: false // will prevent the message from being deleted if the worker/task stops or crashes
});
```

This way the consumer tells RabbitMQ that a message was received, processed and is ready for deletion.

### Forgotten acknoledgment

Run the following command to display unacknowledged messages :  
`sudo rabbitmqctl list_queues name messages_ready messages_unacknowledged`

## Message Durability

Use **durable: true** and **persistent:true** to make sure the messages are saved if the RabbitMQ server crashes.

```javascript
channel.assertQueue(queue, {
      durable: true // this parameter needs to be set to true on both the provider and the consumer
    });
```

```javascript
channel.sendToQueue(queue, Buffer.from(msg), 
  {
    persistent: true
  });
```

## Fair dispatch

To make sure that a worker only receives one task at a time use **channel.prefetch(1)**:

```javascript
channel.assertQueue(queue, {
  durable: true,
})
channel.prefetch(1);
```
