# Simple Kafka Producer & Consumer Demo(in Java)
# Preparation 
First, we create a topic named 'test'.  
Then we start the zookeeper server and the Kafka server.
# Producer Demo
Using the Apache Kafka Java Client API, we have to modify the pom.xml, adding  
![M](/Homework3/pics/dependency.jpg)  
Then create ProducerDemo class, setting some important parameters:   
properties.put("bootstrap.servers", "192.168.1.110:9092");    
and send a message to the topic test:  
String msg = "Producer demo test!";  
producer.send(new ProducerRecord<String, String>("test", msg));  
And we get message with the consumer:  
![Image 1](/Homework3/pics/ptest.jpg)
# Consumer Demo
We create ConsumerDemo class, setting some important parameters:  
kafkaConsumer.subscribe(Arrays.asList("test"));  
And we get all the message from the topic 'test':  
![Image 1](/Homework3/pics/ctest.jpg)
# See code in the folder KafkaProj!