RabbitMQ is a message queue, that allows for publishers to publish messages to message queues. Consumers can consume from the queues.

### Advanced Message Queue Protocol (AMQP)
One of the message queue protocols RabbitMQ supports.

### The RabbitMQ Server
A server that listens to port 5672 by default.

### Publisher
A client that can publish messages to queues, for consumers to read.
It creates a TCP connection to the server.

### Consumer
The consumer creates a TCP connection with the server. It reads messages from the queue it is subscribed to. *Terminology: The server pushes messages to the consumer.*
##### Acknowledgement of a message
A consumer needs to tell the server that it has read the message pushed to it. This will cause the server to dequeue the message, removing it from the queue.

### Channel
![[Pasted image 20231105212507.png]]
A channel is like a connection inside your connection.
This is what is known as [[Multiplexing|multiplexing]].
##### Creating and using queues
There are two methods that channels use to handle queues.
- `assertQueue(q)` - Create a queue if it doesn't exist.
- `sendToQueue(q, msg)` - Publish a message to a queue.

### Queue
A buffer on the server that works as a message queue.
Technically, the publishers and consumers don't know about the queues. They must use what are known as exchanges.

### Exchange
A message is not sent directly to a queue. Instead, the producer sends messages through an exchange. You can think of it as a mail delivery person.

When you publish a message to an exchange, you give it some arguments
```javascript
channel.basic_publish(exchange='amq.direct',
					  routing_key='example.key'
					  properties=pika.BasicProperties(
						  headers={'key': 'value'}
					  ),
					  body='Hello World!')
```

When you publish a message to an exchange, you specify the exchange type, routing keys, header attributes, and the message body. The types of exchanges determine how they handle and route messages.
##### Exchange Types

- `amq.direct`: Messages are routed to queues based on the routing key. Each message is sent to the queue whose binding key matches the routing key of the message.
- `amq.topic`: Messages are routed to queues based on wildcard matches between the routing key and the routing pattern specified in the binding.
- `amq.fanout`: Messages are broadcast to all queues that are bound to this exchange.
- `amq.headers`: Messages are routed based on header attributes and their values.

### Binding

In RabbitMQ, a binding is the link between an exchange and a queue. Bindings define the rules for how messages should be routed from an exchange to queues. They consist of:

- **Exchange:** The source of the messages.
- **Queue:** The destination where messages will be delivered.
- **Routing Key or Pattern:** The criteria used by the exchange to determine how to route messages to the queues.

When you create a binding, you're essentially establishing the connection between an exchange and a queue, defining how the messages should be routed. This configuration enables the exchange to properly direct messages to the associated queues based on defined rules.