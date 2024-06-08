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
#### Exercise 1.7
We can improve our previous solutions now that we know how to create and build a Dockerfile.
Create a new file script.sh on your local machine with the following contents:

```bash
while true
do
  echo "Input website:"
  read website; echo "Searching.."
  sleep 1; curl http://$website
done
```

Create a Dockerfile for a new image that starts from ubuntu:22.04 and add instructions to install curl into that image. 
Then add instructions to copy the script file into that image and finally set it to run on container start using CMD.
After you have filled the Dockerfile, build the image with the name "curler".

If you are getting permission denied, use chmod to give permission to run the script.
The following should now work:

```bash
$ docker run -it curler

  Input website:
  helsinki.fi
  Searching..
  <!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
  <html><head>
  <title>301 Moved Permanently</title>
  </head><body>
  <h1>Moved Permanently</h1>
  <p>The document has moved <a href="https://www.helsinki.fi/">here</a>.</p>
  </body></html>
```

Remember that RUN can be used to execute commands while building the image!
Submit the Dockerfile.

#### Dockerfile for exercise 1.7
```dockerfile
# Start from ubuntu 22.04
FROM ubuntu:22.04

# Update/upgrade the image and install curl
RUN apt-get update && apt-get upgrade -y && apt-get install -y curl

# Use /usr/src/app as our workdir. The following instructions will be executed in this location.
WORKDIR /usr/src/app

# Copy the script.sh
COPY script.sh .

# Alternatively, if we skipped chmod earlier, we can add execution permissions during the build.
RUN chmod +x script.sh

# When running docker run the command will be ./hello.sh
CMD ./script.sh
```

#### Exercise 1.8
By default our devopsdockeruh/simple-web-service:alpine doesn't have a CMD. Instead, it uses ENTRYPOINT to declare which application is run.
We'll talk more about ENTRYPOINT in the next section, but you already know that the last argument in docker run can be used to give a command or an argument.
As you might've noticed it doesn't start the web service even though the name is "simple-web-service". A suitable argument is needed to start the server!
Try docker run devopsdockeruh/simple-web-service:alpine hello. The application reads the argument "hello" but will inform that hello isn't accepted.
In this exercise create a Dockerfile and use FROM and CMD to create a brand new image that automatically runs server.
The Docker documentation CMD says a bit indirectly that if a image has ENTRYPOINT defined, CMD is used to define it the default arguments.
Tag the new image as "web-server"
Return the Dockerfile and the command you used to run the container.
Running the built "web-server" image should look like this:

```bash
$ docker run web-server
[GIN-debug] [WARNING] Creating an Engine instance with the Logger and Recovery middleware already attached.

[GIN-debug] [WARNING] Running in "debug" mode. Switch to "release" mode in production.
- using env:   export GIN_MODE=release
- using code:  gin.SetMode(gin.ReleaseMode)

[GIN-debug] GET    /*path                    --> server.Start.func1 (3 handlers)
[GIN-debug] Listening and serving HTTP on :8080
```

#### Exercise 1.8 Dockerfile and output
```dockerfile
FROM devopsdockeruh/simple-web-service:alpine
CMD server
```

```bash
docker build . -t web-server
docker run web-server

[GIN-debug] [WARNING] Creating an Engine instance with the Logger and Recovery middleware already attached.

[GIN-debug] [WARNING] Running in "debug" mode. Switch to "release" mode in production.
 - using env:   export GIN_MODE=release
 - using code:  gin.SetMode(gin.ReleaseMode)

[GIN-debug] GET    /*path                    --> server.Start.func1 (3 handlers)
[GIN-debug] Listening and serving HTTP on :8080
```

#### Exercise 1.9
In this exercise we won't create a new Dockerfile.
Image devopsdockeruh/simple-web-service creates a timestamp every two seconds to /usr/src/app/text.log when it's not given a command. 
Start the container with a bind mount so that the logs are created into your filesystem.
Submit the command you used to complete the exercise.

```bash
touch log.txt
docker run -v "$(pwd)/log.txt:/usr/src/app/text.log" devopsdockeruh/simple-web-service
```

#### Output
```bash
Starting log output
Wrote text to /usr/src/app/text.log
Wrote text to /usr/src/app/text.log
```

#### Exercise 1.10
In this exercise, we won't create a new Dockerfile.
The image devopsdockeruh/simple-web-service will start a web service in port 8080 when given the argument "server". 
In Exercise 1.8 you already did an image that can be used to run the web service without any argument.
Use now the -p flag to access the contents with your browser. 
The output to your browser should be something like: { message: "You connected to the following path: ...
Submit your used commands for this exercise.

```bash
docker run -it -p 8080:8080 --name port-mapping devopsdockeruh/simple-web-service server
```

```json
{"message":"You connected to the following path: /"
"path":"/"}

```
