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
5. In green you should see the green button _**Code**_, click it, and click on codespaces.
6. Select _**Create codespace on main**_, and once that's loaded you should have an environment of Visual Studio code.
7. Have a look around to make sure you've got the following files
   1. docker-compose.yaml
   2. readme.md
   3. test.txt
   4. test2.txt
8. Once that's all checked out, you may start the tasks, if you have any questions just raise your hand or ask me!




# Terminal sending and receiving 
The following task will give you a grasp of how Kafka works. You'll have a look at the most basic process of how Kafka works, by having a producer sending messages to a topic, and a consumer pulling messages from that topic. This is the introduction to the next tasks, where the process will become more complex, but widely used in the real world.

As a start, in the terminal, the docker compose file needs to be started. The following command will use **-d** so that it's in detached mode (ran in the background).You should see something like in the following image
```
docker compose up -d 
```

<img width="521" height="111" alt="image" src="https://github.com/user-attachments/assets/dba3bd17-0b78-4897-b6b3-bba71d1681ea" />

 
To check that the containers are running, the following command will show the containers, especially with their status, which should say **Up n seconds**
```
docker ps
```
Once that's checked out correctly, a topic can be created using the following command. Through the commands of the broker container's image, a topic can be created. Make sure to change **"topic name"** to the topic you chose previously.
```
docker exec -it -w /opt/kafka/bin broker ./kafka-topics.sh --create --topic "topic name" --bootstrap-server broker:29092
```
A message will appear saying that the topic was created, but if you're interested to confirm so in more detail, or see what other topics are available, the following command will display the whole list of topics available:
```
docker exec -it -w /opt/kafka/bin broker ./kafka-topics.sh --list --bootstrap-server broker:29092
```
Don't worry just now if you see more topics such as **default_ksql_processing_log**, these are topics used from KSQL and Confluent to work, which will be used in later tasks.
Once the topic has been created successfully, the following commands will help you create a consumer and a producer. the following command will create a consumer which will read all the messages to the topic that were pushed by the producer. Once clicked enter, nothing will appear just yet, this is because within the topic there's no messages yet. Remember to switch the following text **"topic name"** to the topic name you chose previously.
```
docker exec -it -w /opt/kafka/bin broker ./kafka-console-consumer.sh --topic "topic name" --from-beginning --bootstrap-server broker:29092
```
Leave this terminal running and open a new terminal using 
<img width="25" height="24" alt="image" src="https://github.com/user-attachments/assets/f0274dcb-44ed-459e-a433-308fed016da2" />
 at the top right of the terminal, which will create a split terminal. Insert the code below by switching **"topic name"** to the name you gave to the topic previously. This command will create a producer.
```
docker exec -it -w /opt/kafka/bin broker ./kafka-console-producer.sh  --topic "topic name" --bootstrap-server broker:29092
```
You can write as many messages as you'd like and as you can see by clicking **enter**, the message can be seen from the producer's terminal. Once you're done, do CTRL + C to finish and close the producer.

Well Done! You've just created your first stream from one source to another. Try again to send messages from the producer by doing the previous steps.
**_Hint_**: Start the consumer again to read from the same topic, but take off --from-beginning, or add --max-messages 10 to only display the last 10 messages.

This tutorial was gotten from [Confluent Tutorials](https://developer.confluent.io/confluent-tutorials/kafka-on-docker/?utm_medium=sem&utm_source=google&utm_campaign=ch.sem_br.nonbrand_tp.prs_tgt.dsa_mt.dsa_rgn.emea_sbrgn.uki_lng.eng_dv.all_con.confluent-developer&utm_term=&creative=&device=c&placement=&gad_source=1&gad_campaignid=19560855027&gbraid=0AAAAADRv2c3SGbEo5llRvcdu1HsLIjo56&gclid=CjwKCAiAtq_NBhA_EiwA78nNWEr4Qv6n3Cx9VrBJVuqtQCSGg2yhUCyuibhQ5gw1xKXar--W6a_GchoCY9EQAvD_BwE).


# Real time file updates
The following task will give you a grasp of how Kafka can be used in the real world for social media, as well as other industry scenarios. You'll have a look at the producer reading the contents from a file and sending the contents to a topic, and a consumer pulling messages from that topic and inserting them into a new file. 

As a start like the previous task, if you've closed docker compose or have just started from this task instead, the following commands will help you start the environment, and make sure to replace the **"topic name"** to the name you want your topic to have.
```
docker compose up -d
```
```
docker ps
```
```
docker exec -it -w /opt/kafka/bin broker ./kafka-topics.sh --create --topic "topic name" --bootstrap-server broker:29092
```
```
docker exec -it -w /opt/kafka/bin broker ./kafka-topics.sh --list --bootstrap-server broker:29092
```

Once confirmed that the topic is created and the containers are running, above your terminal on the top right, click the button <img width="25" height="24" alt="image" src="https://github.com/user-attachments/assets/f0274dcb-44ed-459e-a433-308fed016da2" /> to have two terminals splitting the screen. This will help you visualize better and see in real time the changes.
The following command, written on the first terminal, will help your create a consumer, which will wait for the messages to be pushed onto the topic it wants to read. Remember to write the same topic name you've created recently, or the topic you used in the previous task instead of **"topic name"**
```
docker exec broker /opt/kafka/bin/kafka-console-consumer.sh --topic "topic name" --bootstrap-server broker:29092> test2.txt
```
Leave this terminal open, as now you'll be working onto the second terminal (the one on the right). Following this action, i suggest opening the text file called test2.txt, you'll see its currently empty. In here, every message that's written in test.txt will appear in here once the producer has pushed the contents to the topic. Write some messages within test.txt 
Once you had a look at the files, use the following commands at your choice, where the first will get you to have a producer pushing the messages once, or the second one, where the producer will keep on pushing all the contents in test.txt every 5 seconds. If the second command is chosen, just click CTRL+C within the same terminal to exit from the shell. Remember to change **"topic name"** to the name of your topic.
```
cat test.txt | docker exec -i broker /opt/kafka/bin/kafka-console-producer.sh --topic "topic name" --bootstrap-server broker:29092
```
```
watch -n 5 'cat test.txt | docker exec -i broker /opt/kafka/bin/kafka-console-producer.sh --topic "topic name" --bootstrap-server broker:29092'
```
Once that's done, look at test2.txt, you'll see the file has the same contents as test.txt
**NOTE:** if you see weird items in test2.txt, its because the test.txt file is in a wrong format. At the bottom right of the page you can find UTF-16 or similar. Click it, save with encoding, UTF 8 (NOT UTF 8 with BOM).
**NOTE:** if you still see weird items in test2.txt, delete all the contents from the test.txt file and test2.txt. Then write again your messages within test.txt.
Have a try to change the text from test.txt into something different and repaste the previous code so that the contents will be sent to the topic. Though if you chose the second command, and 5 seconds have passed and you've changed the contents in test.txt, you can see them into test2.txt.

Well Done! You've just created your first stream from one source to another like in a database, or social media like Messenger for example. Have some play around to see how things can be changed, for example add different commands to the producer or consumer as such.

# Reading headlines from the BBC.
As you saw from previous examples, Kafka can be used to send or receive messages, as well as send or receive contents of files. In this task though, you'll be shown something that you can do yourself throughout your daily life. Many websites, companies nowadays give out free, or with a plan, RSS resources. These resources can be accessed through Kafka Connect, which is something used within enterprises to make this process autonomous, or through CURL commands.
As a start, like previously, follow these steps to have the environment started and setup.
```
docker compose up -d
```
```
docker logs broker
```
```
docker exec -it -w /opt/kafka/bin broker ./kafka-topics.sh --create --topic "topic name" --bootstrap-server broker:29092
```
And then you'll be starting a consumer
```
docker exec broker /opt/kafka/bin/kafka-console-consumer.sh --topic "topic name" --bootstrap-server broker:29092 --from-beginning --max-messages 10
```

Once that's ready the following command sections will be explained on what they do, but they'll be added into one command:
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
4. Sending the content to Kafka's topic
```
docker exec -i broker /opt/kafka/bin/kafka-console-producer.sh --topic rssfeed --bootstrap-server broker:29092
```
These commands added together, will push onto the topic "rssfeed" for the messages to then be read by consumers. Therefore try this command in a split terminal putting the topic title to what you wish!
```
curl -s "https://feeds.bbci.co.uk/news/rss.xml" | grep "<title>" | sed 's/<title>//g; s/<\/title>//g; s/<!\[CDATA\[//g; s/^[ \t]*//;s/]]>//g; s/[ \t]*$//' | docker exec -i broker /opt/kafka/bin/kafka-console-producer.sh --topic rssfeed --bootstrap-server broker:29092
```

Well Done! You've just created your first stream from an RSS source, or like a social media as Messenger for example. Have some play around to see how things can be changed, for example finding different RSS sources online like the BBC!

task 4
https://docs.confluent.io/platform/current/ksqldb/quickstart.html


# Useful commands
git config core.autocrlf true 
git restore 
.git status
