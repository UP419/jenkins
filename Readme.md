1. Add Dockerfile ( you can copy content from Jenkins installation guide)
2. build Jenkins blueocean Docker image - docker build -t myjenkins-blueocean:2.528.2-1 .
3. Create a bridge network in Docker - docker network create jenkins
4. run the container. for windows -
   docker run --name jenkins-blueocean --restart=on-failure --detach --network jenkins --env DOCKER_HOST=tcp://docker:2376 --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 --volume jenkins-data:/var/jenkins_home --volume jenkins-docker-certs:/certs/client:ro --publish 8080:8080 --publish 50000:50000 myjenkins-blueocean:2.528.2-1
5. You can access Jenkins on port 8080 (by default, you can check port with docker ps)
6. Get administrator password - docker exec jenkins-blueocean cat /path/to/initialAdminPassword
7. Create admin user
