FROM jenkinsci/slave:latest
USER root
ENV TERRAFROM_VERSION 0.13.1
RUN curl -o terraform.zip https://releases.hashicorp.com/terraform/${TERRAFROM_VERSION}/terraform_${TERRAFROM_VERSION}_linux_amd64.zip && \
    unzip terraform.zip && \
    mv terraform /usr/bin && \
    rm -rf terraform.zip
USER jenkins