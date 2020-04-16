# pets-app
````
[repository] (https://github.com/javahometech/pets-app)
````

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
  	
