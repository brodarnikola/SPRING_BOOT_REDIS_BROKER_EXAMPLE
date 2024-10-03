## How to launch, test this Spring boot application ? 

First install "Docker desktop" and then you can normal just launch main spring boot application. 

If everything is good you should see this messages inside your Spring boot logs. 
" Sending message...
Received <Hello from Redis!> " 

## You will build an application that uses StringRedisTemplate to publish a string message and has a POJO subscribe for the message by using MessageListenerAdapter.

It may sound strange to be using Spring Data Redis as the means to publish messages, but, as you will discover, Redis provides not only a NoSQL data store but a messaging system as well.

## Register the Listener and Send a Message
Spring Data Redis provides all the components you need to send and receive messages with Redis. Specifically, you need to configure:

A connection factory

A message listener container

A Redis template

You will use the Redis template to send messages, and you will register the Receiver with the message listener container so that it will receive messages.
 The connection factory drives both the template and the message listener container, letting them connect to the Redis server.

This example uses Spring Boot’s default RedisConnectionFactory, an instance of JedisConnectionFactory that is based on the Jedis Redis library.
The connection factory is injected into both the message listener container and the Redis template, as the following example (from src/main/java/com/example/messagingredis/MessagingRedisApplication.java) shows:

The bean defined in the listenerAdapter method is registered as a message listener in the message listener container defined in container and will listen for messages on the chat topic. Because the Receiver class is a POJO, it needs to be wrapped in a message listener adapter that implements the MessageListener interface (which is required by addMessageListener()). The message listener adapter is also configured to call the receiveMessage() method on Receiver when a message arrives.

The connection factory and message listener container beans are all you need to listen for messages. To send a message, you also need a Redis template.
Here, it is a bean configured as a StringRedisTemplate, an implementation of RedisTemplate that is focused on the common use of Redis, where both keys and values are String instances.

The main() method kicks off everything by creating a Spring application context. 
The application context then starts the message listener container, and the message listener container bean starts listening for messages.
The main() method then retrieves the StringRedisTemplate bean from the application context and uses it to send a Hello from Redis! message on the chat topic.
Finally, it closes the Spring application context, and the application ends.

## Setting up the Redis server
Before you can build a messaging application, you need to set up the server that will handle receiving and sending messages. This guide assumes that you use Spring Boot Docker Compose support. A prerequisite of this approach is that your development machine has a Docker environment, such as Docker Desktop, available. Add a dependency spring-boot-docker-compose that does the following:

Search for a compose.yml and other common compose filenames in your working directory

Call docker compose up with the discovered compose.yml

Create service connection beans for each supported container

Call docker compose stop when the application is shutdown
