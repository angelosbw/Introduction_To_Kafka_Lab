# Introduction_To_Kafka_Lab

In this lab, three separate tasks will take place, showing the possibilities Apache Kafka can create for individuals, companies, or users in general. For the purpose to keep this lab as simple as possible, as well as allowing everyone to participate and gather further understanding, Github Codespaces is chosen.

Github Codespaces allows anyone with a github account have their own environment , Visual Studio Code, where Docker in our tasks will also be used, as otherwise Apache Kafka's software would have to be installed, therefore going off the purpose of the lecture.

The tasks will show the basics from, producing and consuming from different terminals, producing the contents from a file into another file, to then read live from an RSS source.

The following tasks will gather use of the docker-compose.yaml file to run a containerized image within Docker of the Kafka server, as well as Zookeeper. Have a look at the file and come back here for deeper understanding of each component, otherwise you can move on to the first task.

- services : Defines the containers that Docker Compose will manage.
- broker : The name of the service, in this case "broker", which will be referenced in the commands.
- image : apache/kafka:latest : 




git config core.autocrlf true 
git restore 
.git status

Task 1
1. docker compose up -d //-d flag runs the docker container in detached mode which is similar to running Unix commands in the background by appending &
2. docker logs broker // to see the logs of kafka to confirm that the container is running
3. docker ps // other confirmation to do so
4. docker exec -it -w /opt/kafka/bin broker sh  // open Kafka's container to make commands from within it.
5. ./kafka-topics.sh --create --topic <a topic name> --bootstrap-server broker:29092 //creates a topic for the kafka broker to produce and consume messages.
5.1. ./kafka-topics.sh --list --bootstrap-server broker:29092 //check topics that are created
6. ./kafka-console-producer.sh  --topic <topic name chosen previously> --bootstrap-server broker:29092 // start a producer console with this command
7. write whatever messages to then be consumed and press enter for each message, press CTROL -C to finish
8. open new terminal 
9. write step 4 again
10. ./kafka-console-consumer.sh --topic <topic name chosen previously> --from-beginning --bootstrap-server broker:29092  //consumes the messages from the chosen topic
11. write messages in the previous terminal, and they can be seen from the consumer's terminal once pressed enter after each message.
where the example was gotten from:
https://developer.confluent.io/confluent-tutorials/kafka-on-docker/?utm_medium=sem&utm_source=google&utm_campaign=ch.sem_br.nonbrand_tp.prs_tgt.dsa_mt.dsa_rgn.emea_sbrgn.uki_lng.eng_dv.all_con.confluent-developer&utm_term=&creative=&device=c&placement=&gad_source=1&gad_campaignid=19560855027&gbraid=0AAAAADRv2c3SGbEo5llRvcdu1HsLIjo56&gclid=CjwKCAiAtq_NBhA_EiwA78nNWEr4Qv6n3Cx9VrBJVuqtQCSGg2yhUCyuibhQ5gw1xKXar--W6a_GchoCY9EQAvD_BwE

Task 2
