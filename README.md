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

```
# Ansible-playbook to run two tasks in the same playbook(it's not a good practice in realtime)
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
# Task0 Write a ansible playbook to install apache server on Managed Node and deploy a sample html file on apache server.
```
Make sure you are on /home/ec2-user/ansproj(where your custome inventory file is loacted)

In index.html file write the below code(using vi editor)

<h1 style="color: red"> This is deployed using ansible <h1>

In apache.yml file write the below code(using vi editor)

- hosts: 172.31.33.3
  become: True
  tasks:
          - name: Install apache
            yum:
                    name: httpd
                    state: present
          - name: start and enable apache
            service:
                    name: httpd
                    state: started
                    enabled: yes
          - name: Deploy html file on apache
            copy:
                    src: index.html
                    dest: /var/www/html/
          - name: Restart appache
            service:
                    name: httpd
                    state: restarted




```
# Same above ansible playbook using handlers to restart the apache only when there are changes in index.html file
```
- hosts: 172.31.33.3
  become: True
  tasks:
          - name: Install apache
            yum:
                    name: httpd
                    state: present

          - name: start and enable apache
            service:
                    name: httpd
                    state: started
                    enabled: yes

          - name: Deploy html file on apache
            copy:
                    src: index.html
                    dest: /var/www/html/
            notify:
                    - Restart appache

  handlers:
          - name: Restart appache
            service:
                    name: httpd
                    state: restarted

```
# ansible-playbook to downoad artifact from nexus and deploy it on tomcatserver
```
- hosts: webservers
  tasks:
          - name: Download from nexus and deploy to tomcat
            maven_artifact:
                    group_id: in.javahome
                    artifact_id: pets-app
                    repository_url: 'nexus repository url'
                    username: admin
                    password: admin
                    dest: /opt/tomcat8/webapps/pets-app.war

```

