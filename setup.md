## Clone Repository for Java Worker Process

## Launch a intermediate container to install worker app 

Create a Container with  **schoolofdevops/voteapp-mvn:v0.1.0** image 

```
docker run -idt --name interim schoolofdevops/voteapp-mvn:v0.1.0  sh

```

## Copy over the Source Code


```
docker container cp .  interim:/code

```

Connect to container again and compile and package the code 


```
docker exec -it interim sh 

mvn dependency:resolve
mvn verify
mvn package

```

## Verify jarfile has been built 

```
ls target/

```


Commit  container to an image
 
  * Exit from the container shell and stop container
  * Note container ID 

```
   
  docker container stop interim 
  
  docker container commit interim  <docker_hub_id>/vote-worker:v0.1.0

```

Test by launching container 

```
  docker run -idt  --name test-worker  <docker hub user id >/vote-worker:v0.1.0 sh -c "java -jar target/worker-jar-with-dependencies.jar"
```


