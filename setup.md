## Clone Repository for Java Worker Process

```
git clone https://github.com/schoolofdevops/voting-app-worker.git
```

## Launch a intermediate container to install worker app 

Create a Container with  **schoolofdevops/voteapp-mvn:v0.1.0** image 

```
docker run -idt --name interim schoolofdevops/voteapp-mvn:v0.1.0  sh

```

## Copy over the Source Code


```
cd voting-app-worker 
docker container cp .  interim:/code

```

Connect to container to compile and package the code 


```
docker exec -it interim sh 

mvn package

```

## Verify jarfile has been built 

```
ls target/

```


Commit  container to an image
 
  * Exit from the container shell 
  * Note container ID 

```
     
  docker container commit interim  <docker_hub_id>/worker:v1

```

Test before pushing  by launching container with the packaged app

```
  docker run -idt  --name test-worker  <docker hub user id >/worker:v1 java -jar target/worker-jar-with-dependencies.jar
```

### Push Image to registry 

Before you push the image, you need to be logged in to the registry, with the docker hub id created earlier. Login using the following command, 

```
docker login 
```

To push the image, first list it, 

```
docker image ls
```

[Sample Output]

```
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
initcron/vote-worker         v0.2.0              90cbeb6539df        18 minutes ago      194MB
initcron/vote-worker         v0.1.0              c0199f782489        34 minutes ago      189MB

```

To push the image, 


```
docker push <docker_hub_id>/vote-worker:v0.1.0
```

