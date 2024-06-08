# DevOps with Docker 2024

## Part 1

This part introduces containerization with Docker and relevant concepts such as image and volume. By the end of this part you are able to:

- Run containerized applications
- Containerize applications
- Utilize volumes to store data persistently outside of the containers.
- Use port mapping to enable access via TCP to containerized applications
- Share your own containers publicly

### Exercises

#### Exercise 1.1
Since we already did "Hello, World!" in the material let's do something else.
Start 3 containers from an image that does not automatically exit (such as nginx) in detached mode.
Stop two of the containers and leave one container running.
Submit the output for docker ps -a which shows 2 stopped containers and one running.

![Exercise 1.1 output](https://github.com/Acrofil/devops_with_docker_2024/blob/main/part1/exercise1.1.png)

#### Exercise 1.2
 We have containers and an image that are no longer in use and are taking up space. Running docker ps -a and docker image ls will confirm this.
Clean the Docker daemon by removing all images and containers.
Submit the output for docker ps -a and docker image ls

![Exercise 1.2 output](https://github.com/Acrofil/devops_with_docker_2024/blob/main/part1/exercise1.2.png)

#### Exercise 1.3
Now that we've warmed up it's time to get inside a container while it's running!
Image devopsdockeruh/simple-web-service:ubuntu will start a container that outputs logs into a file. 
Go inside the running container and use tail -f ./text.log to follow the logs. Every 10 seconds the clock will send you a "secret message".
Submit the secret message and command(s) given as your answer.

![Exercise 1.3 output](https://github.com/Acrofil/devops_with_docker_2024/blob/main/part1/exercise1.3.png)

#### Exercise 1.4
Start a Ubuntu image with the process sh -c 'while true; do echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website; done'
If you're on Windows, you'll want to switch the ' and " around: sh -c "while true; do echo 'Input website:'; read website; echo 'Searching..'; sleep 1; curl http://$website; done".
You will notice that a few things required for proper execution are missing. 
Be sure to remind yourself which flags to use so that the container actually waits for input.
Note also that curl is NOT installed in the container yet. You will have to install it from inside of the container.

Test inputting helsinki.fi into the application. It should respond with something like

```HTML
<html>
  <head>
    <title>301 Moved Permanently</title>
  </head>

  <body>
    <h1>Moved Permanently</h1>
    <p>The document has moved <a href="http://www.helsinki.fi/">here</a>.</p>
  </body>
</html>
```

This time return the command you used to start process and the command(s) you used to fix the ensuing problems.

Hint for installing the missing dependencies you could start a new process with docker exec.
    This exercise has multiple solutions, if the curl for helsinki.fi works then it's done. Can you figure out other (smart) solutions?
    
![Exercise 1.4 output](https://github.com/Acrofil/devops_with_docker_2024/blob/main/part1/exercise1.4.png)

```bash
acrofil@acrofil:~$ sudo docker ps -a | grep ubuntu
[sudo] password for acrofil: 
9f57a99b8430   ubuntu    "sh -c 'while true; …"   23 seconds ago   Up 22 seconds             dreamy_euclid

acrofil@acrofil:~$ sudo docker exec -it 9f5 bash
root@9f57a99b8430:/# apt-get update
....
root@9f57a99b8430:/# apt install curl
```

#### Exercise 1.5
In the Exercise 1.3 we used devopsdockeruh/simple-web-service:ubuntu.
Here is the same application but instead of Ubuntu is using Alpine Linux: devopsdockeruh/simple-web-service:alpine.
Pull both images and compare the image sizes. 
Go inside the Alpine container and make sure the secret message functionality is the same. 
Alpine version doesn't have bash but it has sh, a more bare-bones shell.

```bash
acrofil@acrofil:~$ sudo docker pull devopsdockeruh/simple-web-service:ubuntu                                         
ubuntu: Pulling from devopsdockeruh/simple-web-service
5d3b2c2d21bb: Pull complete 
3fc2062ea667: Pull complete 
75adf526d75b: Pull complete 
965d4bbb586a: Pull complete 
4f4fb700ef54: Pull complete 
Digest: sha256:d44e1dce398732e18c7c2bad9416a072f719af33498302b02929d4c112e88d2a
Status: Downloaded newer image for devopsdockeruh/simple-web-service:ubuntu
docker.io/devopsdockeruh/simple-web-service:ubuntu
acrofil@acrofil:~$ sudo docker pull devopsdockeruh/simple-web-service:alpine
alpine: Pulling from devopsdockeruh/simple-web-service
ba3557a56b15: Pull complete 
1dace236434b: Pull complete 
4f4fb700ef54: Pull complete 
Digest: sha256:dd4d367476f86b7d7579d3379fe446ae5dfce25480903fb0966fc2e5257e0543
Status: Downloaded newer image for devopsdockeruh/simple-web-service:alpine
docker.io/devopsdockeruh/simple-web-service:alpine
acrofil@acrofil:~$ sudo docker image ls -a
REPOSITORY                          TAG       IMAGE ID       CREATED       SIZE
devopsdockeruh/simple-web-service   ubuntu    4e3362e907d5   3 years ago   83MB
devopsdockeruh/simple-web-service   alpine    fd312adc88e0   3 years ago   15.7MB
acrofil@acrofil:~$ sudo docker run -d -it devopsdockeruh/simple-web-service:alpine
b720c01b358af53e8d5a5773202da4e2bf8cfbed23b26cb7f78b0e85bd6a03d5
acrofil@acrofil:~$ sudo docker ps -a
CONTAINER ID   IMAGE                                      COMMAND                 CREATED              STATUS              PORTS     NAMES
b720c01b358a   devopsdockeruh/simple-web-service:alpine   "/usr/src/app/server"   About a minute ago   Up About a minute             boring_clarke
acrofil@acrofil:~$ sudo docker exec -it b72 sh
/usr/src/app # tail -f ./text.log
2024-06-08 07:24:51 +0000 UTC
2024-06-08 07:24:53 +0000 UTC
2024-06-08 07:24:55 +0000 UTC
2024-06-08 07:24:57 +0000 UTC
Secret message is: 'You can find the source code here: https://github.com/docker-hy'
2024-06-08 07:24:59 +0000 UTC
```

#### Exercise 1.6
Run docker run -it devopsdockeruh/pull_exercise.
The command will wait for your input.
Navigate through the Docker hub to find the docs and Dockerfile that was used to create the image.
Read the Dockerfile and/or docs to learn what input will get the application to answer a "secret message".
Submit the secret message and command(s) given to get it as your answer.

```bash
acrofil@acrofil:~$ sudo docker run -it devopsdockeruh/pull_exercise
[sudo] password for acrofil: 
Unable to find image 'devopsdockeruh/pull_exercise:latest' locally
latest: Pulling from devopsdockeruh/pull_exercise
8e402f1a9c57: Pull complete 
5e2195587d10: Pull complete 
6f595b2fc66d: Pull complete 
165f32bf4e94: Pull complete 
67c4f504c224: Pull complete 
Digest: sha256:7c0635934049afb9ca0481fb6a58b16100f990a0d62c8665b9cfb5c9ada8a99f
Status: Downloaded newer image for devopsdockeruh/pull_exercise:latest
Give me the password: basics
You found the correct password. Secret message is:
"This is the secret message"

```
