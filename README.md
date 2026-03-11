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
2. Go to the following repository, https://github.com/angelosbw/Introduction_To_Kafka_Lab
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

As a start, in the terminal, the docker compose file needs to be started. The following command will use **-d** so that it's in detached mode (ran in the background).
```
docker compose up -d 
```
To check that the containers are running, the following command will show the containers, especially with their status, which should say **Up n seconds**
```
docker ps
```
Once that's checked out correctly, a topic can be created using the following command. Through the commands of the broker container's image, a topic can be created. Make sure to change **<a topic name>**.
```
docker exec -it -w /opt/kafka/bin broker ./kafka-topics.sh --create --topic <a topic name> --bootstrap-server broker:29092
```
A message will appear saying that the topic was created, but if you're interested to confirm so in more detail, or see what other topics are available, the following command will display the whole list of topics available:
```
docker exec -it -w /opt/kafka/bin broker ./kafka-topics.sh --list --bootstrap-server broker:29092
```
Don't worry just now if you see more topics such as **default_ksql_processing_log**, these are topics used from KSQL to work.
Once the topic has been created successfully, the following command will create a producer which will push all the messages to the topic once clicked CTRL+C. Remember to switch **<topic name chosen previously>** to the name you gave to the topic previously.
```
docker exec -it -w /opt/kafka/bin broker ./kafka-console-producer.sh  --topic <topic name chosen previously> --bootstrap-server broker:29092
```
You can write as many messages, by clicking **enter** to make a new message, and then CTRL + C to finish.
Once all the messages are pushed, open a new terminal and create a consumer, to read from the topic, the messages the producer just pushed.
```
docker exec -it -w /opt/kafka/bin broker ./kafka-console-consumer.sh --topic aTopic --from-beginning --bootstrap-server broker:29092
```
Well Done! You've just created your first stream from one source to another. Try again to send messages from the producer by doing the previous steps.
**_Hint_**: Start the consumer again to read from the same topic, but take off --from-beginning, this will let your consumer display messages in real time as soon as they're pushed from the producer.

where the example was gotten from:
https://developer.confluent.io/confluent-tutorials/kafka-on-docker/?utm_medium=sem&utm_source=google&utm_campaign=ch.sem_br.nonbrand_tp.prs_tgt.dsa_mt.dsa_rgn.emea_sbrgn.uki_lng.eng_dv.all_con.confluent-developer&utm_term=&creative=&device=c&placement=&gad_source=1&gad_campaignid=19560855027&gbraid=0AAAAADRv2c3SGbEo5llRvcdu1HsLIjo56&gclid=CjwKCAiAtq_NBhA_EiwA78nNWEr4Qv6n3Cx9VrBJVuqtQCSGg2yhUCyuibhQ5gw1xKXar--W6a_GchoCY9EQAvD_BwE

Task 2
1. docker compose up -d
2. docker logs broker
3. docker exec -it -w /opt/kafka/bin broker ./kafka-topics.sh --create --topic <topic name chosen> --bootstrap-server broker:29092
3.1. docker exec -it -w /opt/kafka/bin broker ./kafka-topics.sh --list --bootstrap-server broker:29092 //check topics that are created
5. docker exec broker /opt/kafka/bin/kafka-console-consumer.sh --topic <topic name chosen previously> --bootstrap-server broker:29092> test2.txt
6. leave this terminal open and open a second one
7. open the txt file called test2.txt, you'll see its currently empty
8.1. USE THiS for producing all the file every 5 seconds watch -n 5 'cat test.txt | docker exec -i broker /opt/kafka/bin/kafka-console-producer.sh --topic <topic name chosen previously> --bootstrap-server broker:29092'
8.2. OR USE for doing it just once cat test.txt | docker exec -i broker /opt/kafka/bin/kafka-console-producer.sh --topic <topic name chosen previously> --bootstrap-server broker:29092
9. look at test2.txt, you'll see the file has the same contents as test.txt
9.1. NOTE: if you see weird items in test2.txt, its because the test.txt file is in a wrong format. at the bottom right of the page you can find UTF-16 or similar. click it, save with encoding, UTF 8 (NOT UTF 8 with BOM).
10. change the text from test.txt into something different and if 5 seconds have passed, you can see them into test2.txt, or if you chose the command in step 8.2. just repaste it and the contents will be sent to the topic.

Task 3
1. docker compose up -d
2. docker logs broker
3. docker exec -it -w /opt/kafka/bin broker ./kafka-topics.sh --create --topic <topic name chosen> --bootstrap-server broker:29092
4. curl -s "https://feeds.bbci.co.uk/news/rss.xml" | grep "<title>" | sed 's/<title>//g; s/<\/title>//g; s/^[ \t]*//; s/[ \t]*$//' | docker exec -i broker /opt/kafka/bin/kafka-console-producer.sh --topic rssfeed --bootstrap-server broker:29092
5. docker exec broker /opt/kafka/bin/kafka-console-consumer.sh --topic rssfeed --bootstrap-server broker:29092 --from-beginning --max-messages 10

task 4
https://docs.confluent.io/platform/current/ksqldb/quickstart.html


# Useful commands
git config core.autocrlf true 
git restore 
.git status
