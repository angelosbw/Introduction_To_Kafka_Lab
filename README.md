# Introduction_To_Kafka_Lab

In this lab, three separate tasks will take place, showing the possibilities Apache Kafka can create for individuals, companies, or users in general. For the purpose to keep this lab as simple as possible, as well as allowing everyone to participate and gather further understanding, Github Codespaces is chosen.

Github Codespaces allows anyone with a github account have their own environment , Visual Studio Code, where Docker in our tasks will also be used, as otherwise Apache Kafka's software would have to be installed, therefore going off the purpose of the lecture.

The tasks will show the basics from, producing and consuming from different terminals, producing the contents from a file into another file, to then read live from an RSS source.

The following tasks will gather use of the docker-compose.yaml file to run a containerized image within Docker of the Kafka server, as well as Zookeeper. Have a look at the file and come back here for deeper understanding of each component, otherwise you can move on to the first task.

- services : Defines the containers that Docker Compose will manage.
- broker : The name of the service, in this case "broker", which will be referenced in the commands.
- image : apache/kafka:latest : 