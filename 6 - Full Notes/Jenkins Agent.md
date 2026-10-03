2025-06-08 20:15

Tags: [[ci cd]] [[jenkins]] [[runner]]

# Jenkins Agent
## Docker Socket Security
- The socat command above is an isolated local-learning example, not a production recommendation.
- It grants containers on the Jenkins network access to the host Docker daemon; binding the published port to localhost does not protect that internal network path.
- Port 2376 does not enable TLS. This command forwards plaintext TCP to the socket.
- On a rootful Docker host, daemon access can allow privileged containers and host filesystem changes.
- Prefer isolated build hosts and authenticated, encrypted remote access where required. Never expose this proxy to untrusted jobs or networks.
- Review the selected agent's authority before using it from [[CI CD with Jenkins]] or [[Jenkins Pipeline]].


## Docker Cloud Agent (Local)
- Download Plugin
- Run a sosocat container for the Jenkins container to be able to connect to the docker host
```bash
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
[Docker daemon security](https://docs.docker.com/engine/security/)
