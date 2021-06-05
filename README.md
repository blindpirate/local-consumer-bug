## Reproduction for Vert.x local consumer issue

This issue is reproducible with both ZooKeeper and Hazelcast.

### Steps to reproduce

- ZooKeeper:
  - Run `./gradlew installDist` to package the application.
  - Start ZooKeeper at `2181`, e.g. `docker run -d -p 2181:2181 zookeeper:3.6.2`
  - Run `PORT=8080 CONSUMER=1 app/build/install/app/bin/app` start the event consumer.
  - In another terminal, run `PORT=8081 app/build/install/app/bin/app` to start the event producer.
  - No message displayed in event consumer. This is expected because now only local consumer is registered.
  - Now kill the consumer and producer.
  - Run `PORT=8080 CONSUMER=1 ENABLE_DISTRIBUTED_CONSUMER=1 app/build/install/app/bin/app` start the event consumer.
  - In another terminal, run `PORT=8081 ENABLE_DISTRIBUTED_CONSUMER=1 app/build/install/app/bin/app` to start the event producer.
  - Now you can see the local consumer gets a message from distributed event bus.
- Hazelcast:
  - Run `./gradlew installDist` to package the application. 
  - Run `PORT=8080 CONSUMER=1 MANAGER=hazelcast app/build/install/app/bin/app` start the event consumer.
  - In another terminal, run `PORT=8081 MANAGER=hazelcast app/build/install/app/bin/app` to start the event producer.
  - No message displayed in event consumer. This is expected because now only local consumer is registered.
  - Now kill the consumer and producer.
  - Run `PORT=8080 CONSUMER=1 MANAGER=hazelcast ENABLE_DISTRIBUTED_CONSUMER=1 app/build/install/app/bin/app` start the event consumer.
  - In another terminal, run `PORT=8081 MANAGER=hazelcast ENABLE_DISTRIBUTED_CONSUMER=1 app/build/install/app/bin/app` to start the event producer.
  - Now you can see the local consumer gets a message from distributed event bus.  
  
