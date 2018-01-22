## Clone Repository for Java Worker Process

```
git clone https://github.com/schoolofdevops/voting-app-worker.git
```

## Launch a intermediate container to install worker app 

Create a Container with  **maven** image 

```
docker run -idt --name interim  maven  bash

```

## Copy over the Source Code


```
cd voting-app-worker 
docker container cp .  interim:/code

```

Connect to container to compile and package the code 


```
docker exec -it interim bash 

cd /code

mvn package

```

## Verify jarfile has been built 

```
ls target/

java -jar target/worker-jar-with-dependencies.jar
```


Commit  container to an image
 
  * Exit from the container shell 
  * Note container ID 



```
     
  docker container commit interim  <docker_hub_id>/worker:v1

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
xyz/worker               v1              90cbeb6539df        18 minutes ago      194MB

```

To push the image, 


```
docker push <docker_hub_id>/worker:v1
```

