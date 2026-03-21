# Introduction_To_Kafka_Lab

In this lab, you'll learn more about Apache Kafka. The core objectives will be to understand it's structure, how its producers and consumers work, how it can be used in the real world, and how you could use it yourself. The lab involves taking part in four separate tasks, showing the possibilities Apache Kafka can create for individuals, companies, or users in general. For the purpose on keeping this lab as simple as possible, as well as allowing everyone to participate and gather further understanding, Github Codespaces is chosen. The following commands you will try will be working for Linux environments. For Windows, some codes might give issues resulting in unexpected errors because of docker as well.

Github Codespaces allows anyone with a GitHub account to have their own environment, Visual Studio Code, where Docker is also used. The following tasks could be accomplished as well without Docker, only that Apache Kafka's software would have to be installed, therefore going off the purpose of the lecture.

The tasks will show the basics from, producing and consuming from different terminals, producing the contents from a file into another file, read live from an RSS source, to using KSQL with Apache Kafka's structure.

The following tasks will use the docker-compose.yaml file to run a containerized image within Docker of the Kafka broker, as well as Zookeeper, for the first three tasks. For the fourth task, as well as Kafka's images, KSQL's and KSQL CLI's image will be used. Have a look at the file and come back here for deeper understanding of each component, otherwise you can move on to the setup.

- services : Defines the containers that Docker Compose will manage.
- broker : The name of the service, in this case "broker", which will be referenced in the commands.
- image : The Docker image that Docker will use for that specific service
- hostname : Internal network hostname
- container_name : container name as seen by Docker
- ports : port mapping from host to container, allowing external applications to connect to Kafka
- environment: these are environment variables that configure Kafka
- depends_on: starts the container named here before this one

# Setup
To complete this lab, you will need access to a Github account. This will allow you to fork the project, and within your codespace environment complete the tasks. The following steps will help ypu get fully setup to start working on the lab:
1. Log in to GitHub from Google Chrome
2. Go to the following repository, unless you're already in it, https://github.com/angelosbw/Introduction_To_Kafka_Lab
3. Click fork at the top right
4. Click fork again, and now you should be back in the repository, only that it is within your account and under the _**Introduction_To_Kafka_Lab**_ you should see _**forked from ...**_
5. In green you should see the green button <img width="115" height="38" alt="image" src="https://github.com/user-attachments/assets/78e8b412-a75e-4aef-bdaa-176caa8a96f6" />, click it, and click on codespaces<img width="387" height="34" alt="image" src="https://github.com/user-attachments/assets/9e309b7d-d58e-45b9-87aa-5bed59027581" />

6. Select   <img width="216" height="32" alt="image" src="https://github.com/user-attachments/assets/7ee8f5e4-1083-46ec-bd91-fd1b320a6650" />
and once that's loaded you should have an environment of Visual Studio code.
7. Have a look around to make sure you've got the following files
   1. docker-compose.yaml
   2. readme.md
   3. test.txt
   4. test2.txt
8. Once that's all checked out, you may start the tasks, if you have any questions just raise your hand or ask me!

### **IMPORTANT**

Remember to close Codespaces and Delete the whole repository if you wish, as otherwise Github might charge you if you leave your codespace open. To do this click on <img width="114" height="38" alt="image" src="https://github.com/user-attachments/assets/a7145668-41af-4484-98a8-5217caa3c69b" /> then click the three dots <img width="110" height="49" alt="image" src="https://github.com/user-attachments/assets/ca08f6ea-00ef-4a0c-aec7-07db80b40087" /> and select delete <img width="266" height="53" alt="image" src="https://github.com/user-attachments/assets/0378bf82-cd51-48bb-abe4-f5a8e7ae9e4a" />



# 1. Terminal sending and receiving 
The following task will give you a grasp of how Kafka works. You'll have a look at the most basic process of Kafka's process, by having a producer sending messages to a topic, and a consumer pulling messages from that topic. This is the introduction to the next tasks, where the process will become more complex, but widely used in the real world.

As a start, in the terminal, the docker compose file needs to be started. The following command will use **-d** so that it's in detached mode (ran in the background). This might take a minute, but you should then see something like in the following image
```
docker compose up -d 
```

<img width="521" height="111" alt="image" src="https://github.com/user-attachments/assets/dba3bd17-0b78-4897-b6b3-bba71d1681ea" />

 
To check that the containers are running, the following command will show the containers, especially with their status, which should say **Up n seconds**
```
docker ps
```

## Making a topic
Once that's checked out correctly, through the commands of the broker container's image, a topic can be created using the following command. For each of thes upcoming tasks, a predefined topic name is given, but you can change it as you please. In this task the topic will be **task1** and you'll be pushing and pulling messages within this topic.
```
docker exec -it -w /opt/kafka/bin broker ./kafka-topics.sh --create --topic task1 --bootstrap-server broker:29092
```
A message will appear saying that the topic was created, but if you're interested to confirm so in more detail, or see what other topics are available, the following command will display the whole list of topics available:
```
docker exec -it -w /opt/kafka/bin broker ./kafka-topics.sh --list --bootstrap-server broker:29092
```
Don't worry just now if you see more topics such as **default_ksql_processing_log**, these are topics used from KSQLDB and Confluent to work, which will be used in later tasks.
Once the topic has been created successfully, the following commands will help you create a consumer and a producer. 

## Consumer
The following command will create a consumer which will read all the messages to the topic that were pushed by the producer. Once clicked enter, nothing will appear just yet, this is because within the topic there's no messages yet, but very soon your messages will appear.
```
docker exec -it -w /opt/kafka/bin broker ./kafka-console-consumer.sh --topic task1 --from-beginning --bootstrap-server broker:29092
```
Leave this terminal running and open a new terminal using 
<img width="25" height="24" alt="image" src="https://github.com/user-attachments/assets/f0274dcb-44ed-459e-a433-308fed016da2" />
 at the top right of the terminal, which will create a split terminal. 

## Producer
Insert the command below which will create a producer.
```
docker exec -it -w /opt/kafka/bin broker ./kafka-console-producer.sh  --topic "topic name" --bootstrap-server broker:29092
```
You can write as many messages as you'd like and as you can see by clicking **Enter**, the message can be seen from the producer's terminal onto the consumer's terminal. Once you're done, do CTRL + C to finish and close the producer and consumer.

Well Done! You've just created your first stream from one source to another. Try again to send messages from the producer by doing the previous steps.
**_Hint_**: Start the consumer again to read from the same topic, but take off --from-beginning, or add --max-messages 10 to only display the last 10 messages.

This tutorial was based on [Confluent Tutorials](https://developer.confluent.io/confluent-tutorials/kafka-on-docker/?utm_medium=sem&utm_source=google&utm_campaign=ch.sem_br.nonbrand_tp.prs_tgt.dsa_mt.dsa_rgn.emea_sbrgn.uki_lng.eng_dv.all_con.confluent-developer&utm_term=&creative=&device=c&placement=&gad_source=1&gad_campaignid=19560855027&gbraid=0AAAAADRv2c3SGbEo5llRvcdu1HsLIjo56&gclid=CjwKCAiAtq_NBhA_EiwA78nNWEr4Qv6n3Cx9VrBJVuqtQCSGg2yhUCyuibhQ5gw1xKXar--W6a_GchoCY9EQAvD_BwE).


# 2. Real time file updates
The following task will give you a grasp of how Kafka can be used in the real world for social media, as well as other industries. You'll have a look at the producer reading the contents from a file and sending the contents to a topic, and a consumer pulling messages from that topic and inserting them into a new file. 

For the following task, you should still have two terminals open, otherwise click on the button <img width="25" height="24" alt="image" src="https://github.com/user-attachments/assets/f0274dcb-44ed-459e-a433-308fed016da2" /> to have two terminals splitting the screen.

## Looking at the txt files
Open the text files within the folders of the **producer** and **consumer**, you'll see they're both currently empty. Keep both open in a split screen and within **input.txt** write all the messages you want by making a new message with the button **Enter**. Within **output.txt**, every message that's written in **input.txt** will appear in **output.txt** once the producer has pushed the contents to the topic. Write some messages within **input.txt** just like this:

<img width="1707" height="895" alt="image" src="https://github.com/user-attachments/assets/633abda3-ce91-4c74-b4a4-98e03cbf7c63" />

## Making a topic
As a start, we'll be creating a new topic called **fileUpdates** for this task. This is to not create any confusion between the pulling of the messages from the previous task
```
docker exec -it -w /opt/kafka/bin broker ./kafka-topics.sh --create --topic fileUpdates --bootstrap-server broker:29092
```
## Consumer
The following command, written on the first terminal, will help you create a consumer, which will wait for the messages to be pushed onto the topic it wants to read. 
```
docker exec broker /opt/kafka/bin/kafka-console-consumer.sh --topic fileUpdates --bootstrap-server broker:29092> consumer/output.txt
```
Leave this terminal open, as now you'll be working onto the second terminal. 

## Producer
Once you've started the consumer use the following commands at your choice, where the first will get you to have a producer pushing the messages once, or the second one, where the producer will keep on pushing all the contents from **input.txt** every 5 seconds. If the second command is chosen, just click CTRL+C within the same terminal to exit from the shell. 
```
cat producer/input.txt | docker exec -i broker /opt/kafka/bin/kafka-console-producer.sh --topic fileUpdates --bootstrap-server broker:29092
```
```
watch -n 5 'cat producer/input.txt | docker exec -i broker /opt/kafka/bin/kafka-console-producer.sh --topic fileUpdates --bootstrap-server broker:29092'
```

## Seeing final result
Once that's done, if you had the text files split like in the previous screenshot, you should now see all the contents have been copied from **input.txt** to **output.txt** just like this:

<img width="1703" height="692" alt="image" src="https://github.com/user-attachments/assets/51580bc2-6fda-44a6-998a-915726dc70b5" />

### **NOTE:** 
If you see weird items in **output.txt**, its because the **output.txt** file is in a wrong format. At the bottom right of the page you can find UTF-16 or similar. Click it, save with encoding, UTF 8 (NOT UTF 8 with BOM).

### **NOTE:** 
If you still see weird items in **output.txt**, delete all the contents from the **input.txt** file and **output.txt**. Then write again your messages within **input.txt**.
Have a try to change the text from input.txt into something different and repaste the previous code of the producer so that the contents will be sent to the topic. Though if you chose the second command, and 5 seconds have passed and you've changed the contents in **input.txt**, you can see them into **output.txt**.

**Well Done!** You've just created your first stream from one source to another like in a database, or social media like Messenger for example. Have some play around to see how things can be changed, for example add different commands to the producer or consumer as such.

# 3. Reading headlines from the BBC.
As you saw from previous examples, Kafka can be used to send or receive messages, as well as send or receive contents of files. In this task though, you'll be shown something that you can do yourself throughout your daily life. Many websites, companies nowadays give out free, or with a plan, RSS resources. These resources can be accessed through Kafka Connect, which is something used within enterprises to make this process autonomous, or through CURL commands.
For the following task, you should still have two terminals open, otherwise click on the button <img width="25" height="24" alt="image" src="https://github.com/user-attachments/assets/f0274dcb-44ed-459e-a433-308fed016da2" /> to have two terminals splitting the screen.

## Making a topic
As a start, a new topic called **rssfeed** will be created to not cause confusion when pulling the messages from the previous tasks's topics.
```
docker exec -it -w /opt/kafka/bin broker ./kafka-topics.sh --create --topic rssfeed --bootstrap-server broker:29092
```

## Consumer
You'll be starting a consumer which will pull from the **rssfeed** topic within one of the terminals
```
docker exec broker /opt/kafka/bin/kafka-console-consumer.sh --topic rssfeed --bootstrap-server broker:29092 --from-beginning --max-messages 10
```

## Looking online at news
Once that's ready, go to the [BBC website](https://www.bbc.co.uk/) and look at the latest news. Remember some of them as we'll be fetching them!

## BBC command explained
The following command sections will be explained on what they do, but they'll be added into one command after the explanations:
1. See raw RSS data from the BBC
```
curl -s "https://feeds.bbci.co.uk/news/rss.xml" 
```
2. Filter for titles
```
grep "<title>"
```
3. Clean the data
```
sed 's/<title>//g; s/<\/title>//g; s/<!\[CDATA\[//g; s/^[ \t]*//;s/]]>//g; s/[ \t]*$//'
```
4. All of them together
```
curl -s "https://feeds.bbci.co.uk/news/rss.xml" | grep "<title>" | sed 's/<title>//g; s/<\/title>//g; s/<!\[CDATA\[//g; s/^[ \t]*//;s/]]>//g; s/[ \t]*$//'
```
These commands added together, can show the following output on your terminal, where each line is a headline from the xml news feeds file from the BBC:

<img width="1410" height="427" alt="image" src="https://github.com/user-attachments/assets/f96984e8-a3b0-4cee-a742-fab046e4a6a3" />

Do you recognise any of the news you saw previously? Go look again and you will see some will be showing on the [BBC website](https://www.bbc.co.uk/) 

## Producer
To use those commands with Kafka, the following command will use the previous code plus the creation of a producer which will push the messages to the topic **rssfeed**.
```
curl -s "https://feeds.bbci.co.uk/news/rss.xml" | grep "<title>" | sed 's/<title>//g; s/<\/title>//g; s/<!\[CDATA\[//g; s/^[ \t]*//;s/]]>//g; s/[ \t]*$//' | docker exec -i broker /opt/kafka/bin/kafka-console-producer.sh --topic rssfeed --bootstrap-server broker:29092
```
And as a result, you should see something like this within your consumer terminal

<img width="1428" height="417" alt="image" src="https://github.com/user-attachments/assets/a594e225-c6d2-4064-b081-7a2e6f8f43a9" />

Well Done! You've just created your first stream from an RSS source. Have some play around to see how things can be changed, for example finding different RSS sources online like the BBC, or maybe filtering out which news you specifically want or you don't want!

# 4. KSQLDB exercise
For this exercise, we'll find out the latest location of simulated riders. As reference you can go to [Confluent's website](https://docs.confluent.io/platform/current/ksqldb/quickstart.html) which is where the following task was based from. You'll be using Apache Kafka, like previously, only that now KSQLDB will be used as well. If you noticed when running the docker compose file, the ksqldb-server and the ksqldb-cli images were started, this is so that now you can work, like in the industry, with Apache Kafka and a database within Kafka's topics. As a start, write the following codes within the terminal to make a topic called **locations**.

For the following task, you should still have two terminals open, otherwise click on the button <img width="25" height="24" alt="image" src="https://github.com/user-attachments/assets/f0274dcb-44ed-459e-a433-308fed016da2" /> to have two terminals splitting the screen.

## Making a topic
```
docker exec -it -w /opt/kafka/bin broker ./kafka-topics.sh --create --topic locations --bootstrap-server broker:29092
```

## KSQLDB CLI container
Once you're sure that the topic you wished for is created, you can start working forward. 
Now we'll be starting the ksqlDB CLI container. Within the ksql> you'll be writing your SQL queries that run on the KSQLDB server. Write the following command, and you should have something that looks like the following:
```
docker exec -it ksqldb-cli ksql http://ksqldb-server:8088
```
<img width="613" height="423" alt="image" src="https://github.com/user-attachments/assets/30f8fd2e-3c28-45da-8936-624699049a86" />

## Creating a stream
So far so good, within here as said you'll be writing your SQL statements, so as a first we'll be creating a stream. 
A stream associates a schema with a kafka topic. You would use the **CREATE STREAM** to register a stream to a topic. If the topic doesn't exist yet, ksqlDB creates it for you. As you'll see, kafka_topic is the name of the topic, whereas value_format is the encoding in which the messages are stored in the Kafka topic, in this case JSON. Write the following and replace **locations** with the topic you wish for
```
CREATE STREAM riderLocations (profileId VARCHAR, latitude DOUBLE, longitude DOUBLE)
  WITH (kafka_topic='locations', value_format='json', partitions=1);
```
you should see something like 

<img width="673" height="137" alt="image" src="https://github.com/user-attachments/assets/323aaadc-9971-4853-a549-773690ed5557" />

## Creating a table
The goal of this task is to keep track of the latest location of the riders by using a [materialized view](https://docs.confluent.io/platform/current/ksqldb/concepts/materialized-views.html). Run the following command to create the **currentLocation** table.
```
-- Create the currentLocation table
CREATE TABLE currentLocation AS
  SELECT profileId,
         LATEST_BY_OFFSET(latitude) AS la,
         LATEST_BY_OFFSET(longitude) AS lo
  FROM riderlocations
  GROUP BY profileId
  EMIT CHANGES;
```
you should see something like this 

<img width="381" height="309" alt="image" src="https://github.com/user-attachments/assets/1a1771fa-101a-4000-8bf6-596f8a108f99" />

CTAS stands for **CREATE TABLE AS SELECT**, just an abbreviation basically. Now we'll be running a push query, and seeing it displayed within the **currentLocation table**. 

## Pulling the table 
Run the following command to first pull the table's values and such:
```
-- Mountain View lat, long: 37.4133, -122.1162
SELECT * FROM riderLocations
  WHERE GEO_DISTANCE(latitude, longitude, 37.4133, -122.1162) <= 5 EMIT CHANGES;
```
You should see something like this 

<img width="1226" height="142" alt="image" src="https://github.com/user-attachments/assets/72646ad5-9869-411e-bf71-7e96b5c3f333" />


What you're currently looking at is like we had previously. Leave this terminal running and move forward 

## Populating the stream
Now we'll be populating the stream with some values. The following commands will again start a ksqlDB CLI instance, and within it, you'll be inserting statements.
```
docker exec -it ksqldb-cli ksql http://ksqldb-server:8088
```
```
INSERT INTO riderLocations (profileId, latitude, longitude) VALUES ('c2309eec', 37.7877, -122.4205);
INSERT INTO riderLocations (profileId, latitude, longitude) VALUES ('18f4ea86', 37.3903, -122.0643);
INSERT INTO riderLocations (profileId, latitude, longitude) VALUES ('4ab5cbad', 37.3952, -122.0813);
INSERT INTO riderLocations (profileId, latitude, longitude) VALUES ('8b6eae59', 37.3944, -122.0813);
INSERT INTO riderLocations (profileId, latitude, longitude) VALUES ('4a7c7b41', 37.4049, -122.0822);
INSERT INTO riderLocations (profileId, latitude, longitude) VALUES ('4ddad000', 37.7857, -122.4011);
```
Once you click enter, you can see that only three rows appear in the first terminal as such 


<img width="1412" height="371" alt="image" src="https://github.com/user-attachments/assets/0dd8091f-d4d4-425a-88ab-963429a2d6cd" />


Even though you've just inserted five statements, ksqlDB automatically filters the results because of the previous query you inserted. 

## Kafka consumer instead of the ksql 
Try out with a normal Kafka consumer in the second terminal, by writing **exit** to go out from ksql's shell and then write the following command as such.
```
docker exec broker /opt/kafka/bin/kafka-console-consumer.sh --topic locations --bootstrap-server broker:29092 --from-beginning --max-messages 5
```
and you should see something like this:

<img width="1401" height="417" alt="image" src="https://github.com/user-attachments/assets/f236d249-c587-40ac-b3cb-af6cf5e749b6" />


This is because the Kafka Consumer doesn't filter the results, as all the inserted statements are still within the topic, but in this case, ksqlDB filters them unlike Kafka.
Well done, you've just created your first Database filtering within Kafka Topics. Try out different things but once you're done, clean the space by doing CTRL + C and then writing **exit** you should have something like this


<img width="140" height="53" alt="image" src="https://github.com/user-attachments/assets/e4a92b26-b838-4980-a7d7-14773d485d2f" />

# Closing the workspace, IMPORTANT



Remember to close Codespaces and Delete the whole repository if you wish, as otherwise Github might charge you if you leave your codespace open. To do this click on <img width="114" height="38" alt="image" src="https://github.com/user-attachments/assets/a7145668-41af-4484-98a8-5217caa3c69b" /> then click the three dots <img width="110" height="49" alt="image" src="https://github.com/user-attachments/assets/ca08f6ea-00ef-4a0c-aec7-07db80b40087" /> and select delete <img width="266" height="53" alt="image" src="https://github.com/user-attachments/assets/0378bf82-cd51-48bb-abe4-f5a8e7ae9e4a" />






#.  Useful commands
git config core.autocrlf true 
git restore 
.git status
