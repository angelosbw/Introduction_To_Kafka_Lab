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
7. write whatever messages to then be consumed and press enter for each message, press CTRL + C to finish
8. open new terminal 
9. write step 4 again
10. ./kafka-console-consumer.sh --topic <topic name chosen previously> --from-beginning --bootstrap-server broker:29092  //consumes the messages from the chosen topic
11. write messages in the previous terminal, and they can be seen from the consumer's terminal once pressed enter after each message.
where the example was gotten from:
https://developer.confluent.io/confluent-tutorials/kafka-on-docker/?utm_medium=sem&utm_source=google&utm_campaign=ch.sem_br.nonbrand_tp.prs_tgt.dsa_mt.dsa_rgn.emea_sbrgn.uki_lng.eng_dv.all_con.confluent-developer&utm_term=&creative=&device=c&placement=&gad_source=1&gad_campaignid=19560855027&gbraid=0AAAAADRv2c3SGbEo5llRvcdu1HsLIjo56&gclid=CjwKCAiAtq_NBhA_EiwA78nNWEr4Qv6n3Cx9VrBJVuqtQCSGg2yhUCyuibhQ5gw1xKXar--W6a_GchoCY9EQAvD_BwE

Task 2
1. docker compose up -d
2. docker logs broker
2.5 docker exec -it -w /opt/kafka/bin broker sh
3. ./kafka-topics.sh --create --topic <a topic name> --bootstrap-server broker:29092
3.1. ./kafka-topics.sh --list --bootstrap-server broker:29092 //check topics that are created
4. exit
5. docker exec broker /opt/kafka/bin/kafka-console-consumer.sh --topic <topic name chosen previously> --bootstrap-server broker:29092> test2.txt
6. leave this terminal open and open a second one
7. open the txt file called test2.txt, you'll see its currently empty
8.1. USE THiS for producing all the file every 5 seconds watch -n 5 'cat test.txt | docker exec -i broker /opt/kafka/bin/kafka-console-producer.sh --topic <topic name chosen previously> --bootstrap-server broker:29092'
8.2. OR USE for doing it just once cat test.txt | docker exec -i broker /opt/kafka/bin/kafka-console-producer.sh --topic <topic name chosen previously> --bootstrap-server broker:29092
9. look at test2.txt, you'll see the file has the same contents as test.txt
9.1. NOTE: if you see weird items in test2.txt, its because the test.txt file is in a wrong format. at the bottom right of the page you can find UTF-16 or similar. click it, save with encoding, UTF 8 (NOT UTF 8 with BOM).
10. change the text from test.txt into something different and if 5 seconds have passed, you can see them into test2.txt
