# Rabbit MQ

## Content

- [RabbitMQ](#rabbitmq)
- [AMQP](#amqp)
- [Linux install](#linux-install)
- [Send message](#send-message)
- [vhost](#vhost)
- [Work Queue](#work-queue)
- [Acknowledgment](#acknowledgment)
- [Message Durability](#message-durability)
- [Fair dispatch](#fair-dispatch)
- [Exchange](#exchange)
- [Time-To-Live - TTL](#time-to-live---ttl)
- [History](#history)
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
`sudo rabbitmqctl --vhost <vhost_name> delete_queue <queue_name>` => specify the vhost (if none then the default vhost '/' will be used)  
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
The Consumer **subscribes** to the _BROKER_ and **Consumes** the _message_.

## Linux install

[Install link](https://www.rabbitmq.com/install-debian.html).

To start/stop/restart the service:  
`sudo service rabbitmq-server start/stop/restart`

To check the status:  
`sudo service rabbitmq-server status`

To open the interface in the browser, the management plugin must be enabled with this command:  
`sudo rabbitmq-plugins enable rabbitmq_management`  
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

// Import the connect function from the amqp library
import { connect } from 'amqplib';

async function send() {
  try {

    const connection = await connect('amqp://localhost/');
    const channel = await connection.createChannel();

    const queue = 'simpleQueue'
    const msg = 'Simple send'

    await channel.assertQueue(queue, { durable: true })

    channel.sendToQueue(queue, Buffer.from(msg), { persistent: true })

    console.log('[x] Sent %s', msg)

    setTimeout(() => {
      connection.close()
      process.exit(0)
    }, 500)
  } catch (error) {
    console.error(error);
  }
}

send()
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

## vhost

A **vhost** is a **namespace** that **provides a way to isolate resources** such as exchanges, queues, bindings and users. Each virtual host is completely independent of others and has its own set of resources, permissions, and policies.  

When you create a RabbitMQ instance, it comes with a default virtual host called "/" (forward slash). However, you can create additional virtual hosts using the RabbitMQ management UI, command-line tools, or programming APIs.

Virtual hosts can be useful in many scenarios, such as:

- Providing logical isolation between different applications or services that share the same RabbitMQ instance.
- Limiting access to specific resources to different users or groups.
- Enforcing different policies or settings on different sets of resources.

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

Shell one will receive the **first message** and shell 2 will receive the **second message**.  
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
await channel.assertQueue(queue, {
      durable: true // this parameter needs to be set to true on both the provider and the consumer
    });

await channel.assertExchange(exchange, 'direct', { durable: true })
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
- [Headers](#headers)
- [Fanout](#fanout)
- [Consistent Hash](#consistent-hash)
- [Default exchange](#default-exchange)
- [Named exchange and temporary queues](#named-exchange-and-temporary-queues)
- [Bindings](#bindings)

### Direct

The **direct exchange** will deliver messages to the corresponding queues using a **binding key** and a **routing key**. They need to have the **same keyword**.

Sending with the **direct exchange type**:

```javascript
import { connect } from 'amqplib'

async function sendDirect() {
  const connection = await connect('amqp://dev01:dev01@localhost/')
  const channel = await connection.createChannel()

  const exchange = "ex.direct.one"
  const args = process.argv.slice(2)
  const msg = args.slice(1).join(' ') || 'Hello direct exchange'

  const routingKey = args.length > 0 ? args[0] : 'info'

  await channel.assertExchange(exchange, 'direct', { durable: true })

  channel.publish(exchange, routingKey, Buffer.from(msg))

  console.log('[x] Sent %s', msg)

  setTimeout(() => {
    connection.close()
    process.exit(0)
  }, 500);
}

sendDirect()
```

Receiving with the direct exchange type:

```javascript
import { connect } from 'amqplib'

const args = process.argv.slice(2)

if (!args.length) {
  console.log('Command requires arguments: info/warning/error');
}

async function receiveDirect() {

  const connection = await connect('amqp://dev01:dev01@localhost/')
  const channel = await connection.createChannel()

  const exchange = 'ex.direct.one'

  await channel.assertExchange(exchange, 'direct', { durable: true })
  const queue = await channel.assertQueue('', { exclusive: true })

  await channel.bindQueue('', exchange, args[0])

  await channel.consume(queue.queue, msg => {
    console.log(`Received message: ${msg.content.toString()}`);

    channel.ack(msg)
  })

}

receiveDirect()
```

### Topic

The **topic exchange** can deliver messages based on **multiple criteria**. It doesn't use an arbitrary **routing_key** but a **list of words delimited by dots**.

Sending with the topic exchange type:

\* => can substitute for exactly one word  
\# => can substitute for zero or more words

```javascript
import { connect } from 'amqplib'

async function sendTopic() {
  const connection = await connect('amqp://dev01:dev01@localhost/')
  const channel = await connection.createChannel()

  const exchange = "ex.topic.one"
  const args = process.argv.slice(2)
  console.log({ args });

  const msg = args.slice(1).join(' ') || 'Hello topic exchange'
  console.log({ msg });

  const routingKey = args.length > 0 ? args[0] : 'sicilia.van'

  await channel.assertExchange(exchange, 'topic', { durable: true })

  channel.publish(exchange, routingKey, Buffer.from(msg))

  console.log('[x] Sent %s', msg)

  setTimeout(() => {
    connection.close()
    process.exit(0)
  }, 500);
}

sendTopic()
```

Receiving with the topic exchange type:

```javascript
import { connect } from 'amqplib'

const args = process.argv.slice(2)

if (!args.length) {
  console.log('Command requires arguments: info/warning/error');
}

async function receiveTopic() {

  const connection = await connect('amqp://dev01:dev01@localhost/')
  const channel = await connection.createChannel()

  const exchange = 'ex.topic.one'

  await channel.assertExchange(exchange, 'topic', { durable: true })
  const queue = await channel.assertQueue('', { exclusive: true })

  await channel.bindQueue('', exchange, args[0])

  await channel.consume(queue.queue, msg => {
    console.log(`Received message: ${msg.content.toString()}`);

    channel.ack(msg)
  })

}

receiveTopic()
```

### Headers

Headers exchange work like **topics** but use arguments instead of a routing-key.

x-match = any => will bind the queue if one or more argument matches  
x-match = all => will bind the queue only if all arguments match

```javascript
import { connect } from 'amqplib'

const headers = {
  vehicle: 'van',
  destination: 'sicilia',
}

async function sendHeaders() {
  const connection = await connect('amqp://dev01:dev01@localhost/')
  const channel = await connection.createChannel()

  const exchange = "ex.headers.one"
  const msg = process.argv.slice(2) || 'Hello headers exchange'

  console.log({ msg });

  await channel.assertExchange(exchange, 'headers', { durable: true })

  channel.publish(exchange, '', Buffer.from('some nic'), { headers })

  console.log('[x] Sent %s', msg)

  setTimeout(() => {
    connection.close()
    process.exit(0)
  }, 500);
}

sendHeaders()
```

Receive headers:

```javascript
import { connect } from 'amqplib'

// const args = process.argv.slice(2)

const headers = {
  'x-match': 'any/all', // value can be any or all
  vehicle: 'van',
  destination: 'sicilia',
}

async function receiveHeaders() {

  const connection = await connect('amqp://dev01:dev01@localhost/')
  const channel = await connection.createChannel()

  const exchange = 'ex.headers.one'

  await channel.assertExchange(exchange, 'headers', { durable: true })
  const queue = await channel.assertQueue('', { exclusive: true })

  await channel.bindQueue('', exchange, '', headers)

  await channel.consume(queue.queue, msg => {
    console.log(`Received message: ${msg.content.toString()}`);

    channel.ack(msg)
  })
}

receiveHeaders()
```

### Fanout

The fanout exchange type broadcasts all the messages it receives to all the queues it knows.

List exhanges:  
`sudo rabbitmqctl list_exchanges`

### Consistent hash

The consistent-hash exchange type will distribute messages.  

This exchange is **not enabled by default**.  
To enable it:  
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

## Time-To-Live - TTL

A **TTL** can be set for messages or queues so that they are deleted after a given amount of time.  

- [TTL - messages](#ttl---messages)
- [TTL - queues](#ttl---queues)

### TTL - messages

TTL can be set by using the **expiration** key when publishing a message:  

```javascript
  const expiration = 10000; // in milliseconds

  channel.publish(exchange, '', Buffer.from(message), 
    { 
      expiration // the message will be deleted after the given time
    }
  )
```

### TTL - queues

TTl for queues can be set while asserting the queue by using the **expires** key:  

```javascript
  const expires = 10000; // in milliseconds

  await channel.assertQueue(queue, 
    {
      expires // the queue will be deleted after the given time
    }
  );
```

## History

By default RabbitMQ doesn't keep track of messages but it can be done using the **x-recent-history** plugin and creating a exchange of that type.  

Enable the plugin: `rabbitmq-plugins enable rabbitmq_recent_history_exchange`

By default the recent-history exchange keeps the last 20 messages that are published. Then any queue that is newly bound to that exchange will receive those messages.  
The number of messages stored can be set up using the **x-recent-history-length** options in the arguments object.

```javascript
// history-send.js
import amqp from 'amqplib';

async function sendHistory(messageCount) {
  let count = 1
  const connection = await amqp.connect('amqp://admin:admin@localhost/');
  const channel = await connection.createChannel();

  const exchange = 'ex.history'
  const queue = 'q.history'
  const message = 'Hello, History'

  const queueOptions = {
    durable: true,
  }
  const exchangeOptions = {
    durable: true,
    arguments: {
      'x-recent-history-length': 5, // will keep the last 5 messages to be published
    }
  }

  await channel.assertExchange(exchange, 'x-recent-history', exchangeOptions)
  await channel.assertQueue(queue, queueOptions);
  await channel.bindQueue(queue, exchange)

  while (count <= messageCount) {
    channel.publish(exchange, '', Buffer.from(`${message}${count}`))
    count++
    console.log('Sending message')
  }

  count = 1

  setTimeout(() => {
    channel.close();
    connection.close();
    process.exit(0)
  }, 500);
}

sendHistory(process.argv[2]);
```

## Remote procedure call (RPC)

## Dead letter exchange (DLX)

A **Dead Letter Exchange** or **DLX** is an exchange type to which messages are routed if they **cannot be delivered** to their intended destination due to errors such as invalid routing keys or expired TTLs (time-to-live).

```javascript
// dlx-send.js
import { connect } from 'amqplib'

async function dlxSend() {
  try {
    const connection = await connect('amqp://admin:admin@localhost/')
    const channel = await connection.createChannel()

    const exchangeDlx = 'ex.dlx'
    const exchangeBup = 'ex.bup'
    const queueDlx = 'q.dlx'
    const qBackup = 'q.backup'
    const routingKey = 'Routing key'
    const dlxKey = 'Dead letter exchange routing key'

    await channel.assertExchange(exchangeDlx, 'direct', { durable: true })
    await channel.assertExchange(exchangeBup, 'direct', { durable: true })

    await channel.assertQueue(queueDlx, {
      deadLetterExchange: exchangeBup,
      deadLetterRoutingKey: dlxKey, 
      durable: false
    })

    await channel.assertQueue(qBackup, { durable: true })

    await channel.bindQueue(queueDlx, exchangeDlx, routingKey)
    await channel.bindQueue(qBackup, exchangeBup, dlxKey)

    const msg = 'Dead letter exchange is awesome!'

    channel.publish(exchangeDlx, routingKey, Buffer.from(msg))

    setTimeout(() => {
      connection.close()
      process.exit(0)
    }, 500);

  } catch (error) {
    console.log({ error });
  }
}

dlxSend()
```

Receive:

```javascript
// dlx-reveive.js
import { connect } from 'amqplib'

async function dlx() {
  try {
    console.log('log');
    const connection = await connect('amqp://dev:dev@localhost/dev')
    const channel = await connection.createChannel()

    const queueDlx = 'q.dlx'

    await channel.consume(queueDlx, (msg) => {
      console.log(`Rejecting message: ${msg.content.toString()}`);

      channel.reject(msg, false);
      // channel.ack(msg)
    })

    setTimeout(() => {
      connection.close()
      process.exit(0)
    }, 500);

  } catch (error) {
    console.log({ error });
  }
}

dlx()
```

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
RabbitMQ nodes are identified by **node names**. A node name consists of two parts: a **prefix** (usually _rabbit_) and a **host**. e.g. rabbit@local

**Scale out** => **increasing** the number of servers  
**Scale in** => **decreasing** the number of servers

=> **Need to run the instances with the same erlang cookie**
=> **It is recommanded to use an odd number of nodes**  
Create a new instance:  

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

## Security

- [Authentication](#authentication)
- [Authorization](#authorization)

### Authentication

Users can login using a username and a password.

### Authorization

Users can have **Configure**, **Write** and **Read** rights.  
The authorizations are set up using Regex:  

| Group | Name |
| - | - |
| Configure |  |
| Write | ^ex\.exchangeName$ |
| Read | ^q\.queueName\.news[1-2]{1}$ |
