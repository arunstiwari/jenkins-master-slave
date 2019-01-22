1. Create the volume container jenkins_master
   sudo docker volume create jenkins_master

2. Execute the docker container using the following command
    sudo docker run --name myjenkins -p 8080:8080 -p 50000:50000 -v jenkins_master:/var/jenkins_home jenkins/jenkins:lts

3. Copy the password and login to the http://localhost:8080

4. Select the "Install recommended plugins"

5. Now open the http://localhost:8080/script
   Paste the following groovy script and run it. Copy the contents till the line "Result: [Plugin:cloudbees-folder,"
   and paste this in the plugins.txt file

   Groovy script is 
    Jenkins.instance.pluginManager.plugins.each{
    plugin ->
        println ("${plugin.getShortName()}")
    }

6. sudo docker build -t jenkinsqai .
   This takes sometime as it is going to download the plugins 
7. Run the following command
   sudo docker run -p 8080:8080 -p 50000:50000 -v jenkins_master:/var/jenkins_home --rm --name myjenkins jenkinsqai:latest

   Verify if all the plugins are installed by running the step 5

8. Download the jdk 8 from the url https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

9. Download the maven 3 from the url
http://mirrors.estointernet.in/apache/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.tar.gz

10. Stop the docker container and restart it again using the following command
Find the mac ip address using the following command
ifconfig | grep "inet " | grep -v 127.0.0.1

sudo docker run -p 8080:8080 --rm --name myjenkins  -v jenkins_master:/var/jenkins_home -v `pwd`/downloads:/var/jenkins_home/downloads -e SONARQUBE_HOST='http://192.168.1.5:9000'  jenkinsqai:latest

sudo docker exec -it myjenkins /bin/bash

11. Sonarqube 
$ sudo docker run --rm -d --name sonarqube -p 9000:9000 -v sonarqube:/opt/sonarqube sonarqube:6.7.6-community

12. sudo docker logs sonarqube 

SonarScanner : - https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner
13. Create Token
mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.login=9228bb4f76a756d8ea542cd462bb40ef2777e54b


https://pghalliday.com/jenkins/groovy/sonar/chef/configuration/management/2014/09/21/some-useful-jenkins-groovy-scripts.html
https://github.com/Accenture/adop-jenkins/blob/master/resources/init.groovy.d/adop_sonar.groovy


docker run -p 8080:8080 --rm --name myjenkins  -v jenkins_master:/var/jenkins_home -v `pwd`/downloads:/var/jenkins_home/downloads -e SONARQUBE_HOST=http://192.168.1.5:9000 -e SONAR_ENABLED=true -e SONAR_SERVER_URL=http://192.168.1.5:9000 -e SONAR_AUTH_TOKEN=9228bb4f76a756d8ea542cd462bb40ef2777e54b  jenkinsqai:latest
