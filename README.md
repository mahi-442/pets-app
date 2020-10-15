# Gooogle docs link

[Google docs running notes](https://docs.google.com/document/d/12pLDe3ONtasPd5FL_RJQZAMqDFDVUUVZBtPOdh6D3I0/edit)

# pets-app
[Hari-Sir-Repository](https://github.com/javahometech/pets-app)

# Configure on POM.xml file
```

<distributionManagement>
		 <snapshotRepository>
		    <id>nexusRepo</id>
		    <url>http://13.233.230.166:8081/repository/maven-snapshots/</url>
		 </snapshotRepository>
		
		<repository>
		    <id>nexusRepo</id>
		    <url>http://13.233.230.166:8081/repository/maven-releases/</url>
		</repository>
  	</distributionManagement>
```
# Below is sshPublisher plugin code to deply on Tomcat

```
    stage('Deploy to Tomcat'){
      steps{
        script{
          def userHost = "ec2-user@172.31.47.226"
          def tomcatBin = "ec2-user@172.31.47.226 /opt/tomcat8/bin"
	   		  sshagent(['tomcat-dev']) {
                  // copy war file to tomcat webapps
                  sh "scp -o StrictHostKeyChecking=no target/*.war ${userHost}:/opt/tomcat8/webapps/pets-app.war"
                  // stop and start tomcat
                  sh "ssh ${tomcatBin}/shutdown.sh"
                  sh "ssh ${tomcatBin}/startup.sh"
          }
        }
      }
```
# Sample Docker image1
```
FROM alpine:3.8.5

LABEL NAME="mahi"
LABEL EMAIL="9618714521m@gmail.com"
LABEL TIER="FRONT"
ARG JAVA_VERSION=openjdk8-jre
#Install Java
RUN apk add $JAVA_VERSION

#Create work dir to store our code
WORKDIR /app/javahome

#Copy our code to image
COPY Welcome.class .
COPY startup.sh .
#Set ENVIRONMENT Variable for demo
ENV NAME="Mahesh"

#Run application when container start
ENTRYPOINT ["ping"]

CMD ["google.com"]

```
# Sample Docker image2

```
FROM alpine:3.8.5

LABEL NAME="mahi"
LABEL EMAIL="9618714521m@gmail.com"
LABEL TIER="FRONT"
ARG JAVA_VERSION=openjdk8-jre
#Install Java
RUN apk add $JAVA_VERSION

#Create work dir to store our code
WORKDIR /app/javahome

#Copy our code to image
COPY * ./
ENV APP_ENV=dev

CMD echo "Using $APP_ENV properties"

```
# Docker image to set up a small webserver on apache
```
FROM ubuntu:16.04

LABEL NAME="mahi"
LABEL EMAIL="9618714521m@gmail.com"
LABEL TIER="FRONT"

RUN apt update
RUN apt install apache2 -y

COPY index.html /var/www/html/

EXPOSE 80

CMD apachectl -D FOREGROUND

```
# Ansible-playbook to install git
```
---
- hosts: 172.31.33.3
  become: True
  tasks:
          - name: Install git
            yum:
                    name: git
                    state: present

...
# Ansible-playbook to run two task in the same Ansible-playbook(it's not a good practice in real time
```
---
- hosts: 172.31.33.3
  become: True
  tasks:
          - name: Install git
            yum:
                    name: git
                    state: present

- hosts: 172.31.33.3
  become: True
  tasks:
          - name: Install Docker
            yum:
                    name: docker
                    state: present
          - name: Start docker daemon
            service:
                    name: docker
                    state: started
                    enabled: yes



...

```

```
