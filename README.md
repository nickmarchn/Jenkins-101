# Jenkins-101
I followed a Jenkins tutorial on YouTube that provides step-by-step instructions on setting up Jenkins using Docker. The tutorial video can be found at the following link: YouTube Tutorial

Installation
Build the Jenkins BlueOcean Docker Image
To build the Jenkins BlueOcean Docker image, run the following command:

Copy code
docker build -t myjenkins-blueocean:2.332.3-1 .
If you encounter any issues building the image, you can pull it from the following registry and rename it to myjenkins-blueocean:2.332.3-1:

bash
Copy code
docker pull devopsjourney1/jenkins-blueocean:2.332.3-1 && docker tag devopsjourney1/jenkins-blueocean:2.332.3-1 myjenkins-blueocean:2.332.3-1
Create the network 'jenkins'
Create a Docker network called 'jenkins' using the following command:

lua
Copy code
docker network create jenkins
Run the Container
MacOS / Linux
For MacOS or Linux, use the following command to run the Jenkins container:

shell
Copy code
docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.332.3-1
Windows
For Windows, use the following command to run the Jenkins container:

shell
Copy code
docker run --name jenkins-blueocean --restart=on-failure --detach `
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 `
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 `
  --volume jenkins-data:/var/jenkins_home `
  --volume jenkins-docker-certs:/certs/client:ro `
  --publish 8080:8080 --publish 50000:50000 myjenkins-blueocean:2.332.3-1
Get the Password
To retrieve the initial admin password for Jenkins, execute the following command:

shell
Copy code
docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword
Connect to Jenkins
You can access Jenkins by navigating to https://localhost:8080/ in your web browser.

Installation Reference
For more detailed information on installing Jenkins using Docker, you can refer to the official Jenkins documentation: Jenkins Docker Installation

Additional Configuration: Forwarding Traffic to Docker Desktop on Host Machine
To forward traffic from Jenkins to Docker Desktop on the host machine, you can follow these steps:

Run the following command to start an alpine/socat container:
shell
Copy code
docker run -d --restart=always -p 127.0.0.1:2376:2375 --network jenkins -v /var/run/docker.sock:/var/run/docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock
Retrieve the IP address of the container by executing the following command:
shell
Copy code
docker inspect <container_id> | grep IPAddress
Using the Jenkins Python Agent
If you require a Jenkins agent with Python support, you can pull the devopsjourney1/myjenkinsagents:python image using the following command:

shell
Copy code
docker pull devopsjourney1/myjenkinsagents:python
