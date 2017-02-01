## Clone Repository for Java Worker Process

## Launch a intermediate container to install worker app 

Create a Container with  **schoolofdevops/voteapp-mvn:v0.1.0** image 

```
docker run -idt --name interim schoolofdevops/voteapp-mvn:v0.1.0  sh

```

Connect to the container using docker exec and create a directory /code
```
mkdir /code
```

## Copy over the Source Code


```
docker container cp .  /code/
```

Connect to container again and compile and package the code 

```
mvn dependency:resolve
mvn verify
mvn package

```


Commit  container to an image
 
  * Exit from the container shell and stop container
  * Note container ID 


   
  docker container ps 
 
  docker container commit <container id> -t <docker hub user id >/vote-worker:v0.1.0  

Test by launching container 

  docker run -idt  --name test-worker  <docker hub user id >/vote-worker-:v0.1.0 sh -c "java -jar target/worker-jar-with-dependencies.jar"


