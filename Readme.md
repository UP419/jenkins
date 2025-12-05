1. Add Dockerfile (you can copy content from Jenkins installation guide)
2. build Jenkins blueocean Docker image - docker build -t myjenkins-blueocean:2.528.2-1 .
3. Create a bridge network in Docker - docker network create jenkins
4. run the container. for windows -
   docker run --name jenkins-blueocean --restart=on-failure --detach --network jenkins --env DOCKER_HOST=tcp://docker:2376 --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 --volume jenkins-data:/var/jenkins_home --volume jenkins-docker-certs:/certs/client:ro --publish 8080:8080 --publish 50000:50000 myjenkins-blueocean:2.528.2-1
5. You can access Jenkins on port 8080 (by default, you can check port with docker ps)
6. Get administrator password - docker exec jenkins-blueocean cat /path/to/initialAdminPassword
7. Create admin user
8. You can open terminal inside container - docker exec -it jenkins-blueocean bash. (We can look at Jenkins data, path is specified in docker run command - jenkins-data:/var/jenkins_home, workspaces are stored there. Also install python if you need)
9. How to create Docker agent -
   1. run alpine/socat container - docker run -d --restart=always -p 127.0.0.1:2376:2375 --network jenkins -v /var/run/docker.sock:/var/run/docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock
      to expose your host machine's Docker engine to Jenkins. Jenkins will use this container to mediate between docker host and Jenkins container.
   2. get IP address of container with - docker inspect <container_name>
   3. fill Docker Host URI - tcp://IP_Address:Port
   4. get docker image name from docker hub - jenkins/agent
   5. fill Remote File System Root - /home/jenkins
10. trigger build with Poll SCM - fill in `H */5 * * *` (checks git for changes in every 5 minutes at a random minute)
11.
