2025-06-08 20:15

Tags: [[ci cd]] [[jenkins]] [[runner]]

# Jenkins Agent
## Docker Cloud Agent (Local)
- Download Plugin
- Run a socat container for the Jenkins container to be able to connect to the docker host
```
docker run -d --restart=always -p 127.0.0.1:2376:2375 --network jenkins -v /var/run/docker.sock:/var/run/docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock
docker inspect <container_id> | grep IPAddress
```
- Create a cloud agent with the IP and port of the cat container
- Create agent template

## Agent Template
- Label > allow job to select the agent for execution
- Specify the image
- Instance Capacity (set to a smaller number first)
- Remote File System Root `/home/jenkins`


# References
[[2 - Source Materials/Videos/Youtube Videos/Jenkins Agent|Jenkins Agent]]