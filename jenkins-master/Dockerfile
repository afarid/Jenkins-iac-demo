FROM jenkins/jenkins:lts
USER root
RUN curl -L https://get.docker.io | bash -
RUN usermod -a -G docker jenkins
USER jenkins