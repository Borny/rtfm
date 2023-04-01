# Rabbit MQ

- [RabbitMQ](#rabbitmq)
- [AMQP](#amqp)
- [Linux install](#linux-install)
- [Send message](#send-message)
- [Work Queue](#work-queue)
- [Acknowledgment](#acknowledgment)
- [Message Durability](#message-durability)
- [Fair dispatch](#fair-dispatch)
- [Exchange](#exchange)
- [Routing](#routing)
- [Remote procedure call (RPC)](#remote-procedure-call-rpc)
- [Dead letter exchange](#dead-letter-exchange-dlx)
- [Alarms](#alarms)

## RabbitMQ

Open-source **message-broker software**. It implements the **Advanced Message Queuing Protocol (AMQP)**.
It supports plugins for other protocols:

- Streaming Text Oriented Messaging Protocol (**STOMP**)
- Message Queuing Telemetry Transport (**MQTT**)

Written in **Erlang Programming Language**.

**Throughput** refers to the **rate** at which **messages can be processed** by a RabbitMQ cluster.  
It is a measure of the system's ability to handle a high volume of messages without becoming overloaded or experiencing performance issues.

### Command line

`sudo rabbitmqctl status`  
`sudo rabbitmqctl cluster_status --formatter json | jq` => Note: the jq library needs to be installed with `sudo apt install jq`  

### List

`sudo rabbitmqctl list_queues`  
`sudo rabbitmqctl list_exchanges`  
`sudo rabbitmqctl list_bindings`

### Purge queue

`sudo rabbitmqctl --node <node_name> purge_queue <queue_name>`

### Delete queue

`sudo rabbitmqctl --node <node_name> delete_queue <queue_name>`  
`sudo rabbitmqctl --node <node_name> delete_queue <queue_name> --if-empty` => if the queue is empty  
`sudo rabbitmqctl --node <node_name> delete_queue <queue_name> --if-unused` => if the queue is unused

### Rest calls

`curl -u guest:guest http://localhost:15672/api/queues | jq`
`curl -u guest:guest http://localhost:15672/api/exchanges | jq`

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

The message limit is 2GB. Message is sent per frames: 131KB by default.

### Queuing models

Producer => BROKER (RabbitMQ) => Consumer

The Producer **publishes** the message to the broker.  
A **Queue** is a **Buffer** that stores messages.  
The Consumer **Consumes** the _message_ and **subscribes** to the _BROKER_

## Linux install

[Install link](https://www.rabbitmq.com/install-debian.html).

To start/stop/restart the service:  
`service rabbitmq-server start/stop/restart`

To check the status:  
`service rabbitmq-server status`

To open the interface in the browser, first run:  
`rabbitmq-plugins enable rabbitmq_management`  
Then open the browser on:  
`127.0.0.1:15672`  or `localhost:15672`

Use the following credentials to login for the first time locally:

- user : **guest**
- password : **guest**

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
// ./worker.js

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

### Forgotten acknowledgment

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
channel.consume(queue, (msg) => {...})
...
```

## Exchange

A **producer** shouldn't send a message directly to a queue, it shouldn't even know about any queue. Instead it delivers a message to an **exchange** that will in turn send it to a queue.

Producer => Exchange => Queue => Consumer(s)

So an **exchange** receives a message from a **Producer** and depending on the rules defined in the **exchange type**, it will deal with the messages accordingly.

Exchange types:  

- [Direct](#direct)
- [Topic](#topic)
- [Header](#header)
- [Fanout](#fanout)
- [Consistent Hash](#consistent-hash)
- [Default exchange](#default-exchange)
- [Named exchange and temporary queues](#named-exchange-and-temporary-queues)
- [Bindings](#bindings)

### Direct

The **direct exchange** will deliver messages to the corresponding queues using a **binding key** and a **routing key**. They need to have the **same keyword**.

### Topic

The **topic exchange** can deliver messages based on multiple criteria. It doesn't use an arbitrary **routing_key** but a **list of words delimited by dots**.

### Header

Headers exchange work like **topics** but use arguments instead of a routing-key.

x-match = any => will bind the queue if one or more argument matches  
x-match = all => will bind the queue only if all arguments match

### Fanout

The fanout exchange type broadcasts all the messages it receives to all the queues it knows.

List exhanges:  
`sudo rabbitmqctl list_exchanges`

### Consistent hash

The consistent-hash exchange type will distribute messages.  

This exchange is **not installed by default**.  
Install the plugin:  
`sudo rabbitmq-plugins enable rabbitmq_consistent_hash_exchange`

### Default exchange

The default exchange is used if no exchange is provided.  
For instance when sending directly to a queue:  
`channel.sendToQueue('hello', Buffer.from('Hello World!'))`  

### Named exchange and temporary queues

Publishing to a specific exchange:  
`channel.publish('logs', '', Buffer.from('Hello World!'))`  
The first parameter is the name of the exchange.  
The second parameter is the name of the queue. If no name is privided (an empty string) then a **temporary queue** will be created by the amqp.node client.  
The third parameter is the message to be sent.

A temporary queue **will be deleted** once every consumers disconnect from the exchange:  

```javascript
channel.assertQueue('', {
  exclusive: true
});
```

### Bindings

The relationship **between an exchange and a queue** is called **binding**:  
`channel.bindQueue(queue_name, 'logs', '')`

List bindings:  
`rabbitmqctl list_bindings`

Sending:

```javascript
const amqp = require('amqplib/callback_api')

amqp.connect('amqp://localhost', (error0, connection) => {
  if(error0)throw error0

  connection.createChannel((error1,channel ) => {
    if(error1)throw error1

    // Naming the exchange
    const exchange = 'logs'
    const msg = process.argv.slice(2).join(' ') || 'Hello World!'

    // Creating the exchange with a fanout type
    channel.assertExchange(exchange, 'fanout', {durable: false})

    // Publishing the message to the temporary queue
    channel.publish(exchange, '', Buffer.from(msg))

    console.log('[x] Sent %s', msg);
  })

  setTimeout(() => {
    connection.close()
    process.exit(0)
  }, 500);
})
```

Receiving:

```javascript
const amqp = require('amqplib/callback_api')

amqp.connect('amqp://localhost', (error0, connection) => {
  if (error0) throw error0

  connection.createChannel((error1, channel) => {
    if (error1) throw error1

    // The exchange must have the same name as the one declared in the send file
    const exchange = 'logs'

    channel.assertExchange(exchange, 'fanout', { durable: false })

    // Asserting the temporary queue
    channel.assertQueue('', { exclusive: true }, (error2, q) => {
      if (error2) throw error2
      console.log(' [*] Waiting for messages in %s. To exit press CTRL+C', q.queue)

      // Binding the exchange to the temporary queue
      channel.bindQueue(q.queue, exchange, '')

      channel.consume(q.queue, (msg) => {
        if (msg.content) console.log("[x] %s", msg.content.toString());
      }, { noAck: true })
    })
  })
})

```

## Routing

Sending with the **direct exchange type**:

```javascript
const amqp = require('amqplib/callback_api')

amqp.connect('amqp://localhost', (error0, connection) => {
  if(error0)throw error0

  connection.createChannel((error1,channel ) => {
    if(error1)throw error1

    const exchange = 'direct_logs'
    const args = process.argv.slice(2)
    const msg = args.slice(1).join(' ') || 'Hello World!'
    const severity = args.length > 0 ? args[0] : 'info'

    channel.assertExchange(exchange, 'direct', {durable: false})

    channel.publish(exchange, severity, Buffer.from(msg))

    console.log('[x] Sent %s', msg);
  })

  setTimeout(() => {
    connection.close()
    process.exit(0)
  }, 500);
})
```

Receiving with the direct exchange type:

```javascript
const amqp = require('amqplib/callback_api')

const args = process.argv.slice(2)

if (args.length === 0) {
  console.log('Usage: receive-logs-direct.js [info] [warning] [error]');
  process.exit(1)
}

amqp.connect('amqp://localhost', (error0, connection) => {
  if (error0) throw error0

  connection.createChannel((error1, channel) => {
    if (error1) throw error1

    const exchange = 'direct_logs'

    channel.assertExchange(exchange, 'direct', { durable: false })

    channel.assertQueue('', { exclusive: true }, (error2, q) => {
      if (error2) throw error2
      console.log(' [*] Waiting for messages in %s. To exit press CTRL+C', q.queue)

      args.forEach(severity => {
        channel.bindQueue(q.queue, exchange, severity)
      })

      channel.consume(q.queue, (msg) => {

        console.log("[x] %s: '%s", msg.fields.routingKey, msg.content.toString());
      }, { noAck: true })
    })
  })
})
```

## Topics

Sending with the topic exchange type:

\* => can substitute for exactly one word
\# => can substitute for zero or more words

```javascript
const amqp = require('amqplib/callback_api')

amqp.connect('amqp://localhost', (error0, connection) => {
  if(error0)throw error0

  connection.createChannel((error1,channel ) => {
    if(error1)throw error1

    const exchange = 'topic_logs'
    const args = process.argv.slice(2)
    const key = args.length > 0 ? args[0] : 'anonymous.info'
    const msg = args.slice(1).join(' ') || 'Hello World!'

    channel.assertExchange(exchange, 'topic', {durable: false})

    channel.publish(exchange, key, Buffer.from(msg))

    console.log('[x] Sent %s', msg);
  })

  setTimeout(() => {
    connection.close()
    process.exit(0)
  }, 500);
})
```

Receiving with the topic exchange type:

```javascript
const amqp = require('amqplib/callback_api')

const args = process.argv.slice(2)

if (args.length === 0) {
  console.log('Usage: receive-logs-direct.js <facility>.<severity>');
  process.exit(1)
}

amqp.connect('amqp://localhost', (error0, connection) => {
  if (error0) throw error0

  connection.createChannel((error1, channel) => {
    if (error1) throw error1

    const exchange = 'topic_logs'

    channel.assertExchange(exchange, 'topic', { durable: false })

    channel.assertQueue('', { exclusive: true }, (error2, q) => {
      if (error2) throw error2

      console.log(' [*] Waiting for messages in %s. To exit press CTRL+C', q.queue)

      args.forEach(key => {
        channel.bindQueue(q.queue, exchange, key)
      })

      channel.consume(q.queue, (msg) => {

        console.log("[x] %s: '%s", msg.fields.routingKey, msg.content.toString());
      }, { noAck: true })
    })
  })
})
```

## Remote procedure call (RPC)

## Dead letter exchange (DLX)

## Policies

**Policies** are used for specifying optional arguments for groups of queues and exchanges,

## Lazy queues

??? <https://www.udemy.com/course/rabbitmq-in-practice/learn/lecture/29608430#questions>
<https://www.rabbitmq.com/lazy-queues.html>

## Priority

<https://www.udemy.com/course/rabbitmq-in-practice/learn/lecture/30400630#questions>
<https://www.rabbitmq.com/priority.html>

## Cluster

A **RabbitMQ node** refers to a single instance of RabbitMQ running on a server or virtual machine. When multiple RabbitMQ nodes are connected together, they form a **RabbitMQ cluster**.  
A **cluster** is a **group of servers**.  

**Scale out** => **increasing** the number of servers  
**Scale in** => **decreasing** the number of servers

## Alarms

Alarms can be set up for different metrics:

- [Memory](#memory)
- [Disk space](#disk-space)

### Memory

Update the **rabbitmq.conf** file:

```bash
vm_memory_high_watermark.relative = <value between 0 and 1>
```

### Disk space

Update the **rabbitmq.conf** file:  

```bash
disk_free_limit.relative = <value between 0 and 10>
```

Or directly via the command line:  
`sudo rabbitmqctl set_disk_free_limit <value: 1MB / 5GB / 1000 / ...>`
