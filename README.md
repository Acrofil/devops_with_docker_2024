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

