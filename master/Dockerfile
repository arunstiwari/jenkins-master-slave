FROM jenkins/jenkins:latest

MAINTAINER ARUN TIWARI arunstiwari@gmail.com

ENV JENKINS_USER admin
ENV JENKINS_PASS admin
#get rid of admin password setup
ENV JAVA_OPTS="-Djenkins.install.runSetupWizard=false"

#automatically installing all plugins
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

#adding scripts
COPY groovy/* /usr/share/jenkins/ref/init.groovy.d/
# This is done as when Jenkins starts, it runs all scripts in a directory called init.groovy

VOLUME /var/jenkins_home