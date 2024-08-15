FROM ubuntu:22.04
RUN apt-get update
RUN apt-get install openjdk-17-jdk -y
RUN apt-get install git -y
RUN apt-get install curl -y
RUN apt-get install wget -y
RUN apt-get install zip -y
RUn apt-get install sudo -y

# Preparar ambiente para back-end
# Maven

ENV MAVEN_VERSION=3.9.8
ENV BASE_URL=https://dlcdn.apache.org/maven/maven-3/${MAVEN_VERSION}/binaries

RUN mkdir -p /usr/share/maven /usr/share/maven/ref
RUN curl -fsSL -o /tmp/apache-maven.tar.gz ${BASE_URL}/apache-maven-${MAVEN_VERSION}-bin.tar.gz
RUN tar -xzf /tmp/apache-maven.tar.gz -C /usr/share/maven --strip-components=1
RUN rm -f /tmp/apache-maven.tar.gz
RUN ln -s /usr/share/maven/bin/mvn /usr/bin/mvn

ENV MAVEN_HOME=/usr/share/maven
ENV MAVEN_CONFIG=/workspace1/.m2

ADD settings.xml /usr/share/maven/conf/

RUN curl -Ls https://sh.jbang.dev | bash -s - trust add https://repo1.maven.org/maven2/io/quarkus/quarkus-cli/
RUN curl -Ls https://sh.jbang.dev | bash -s - app install --fresh --force quarkus@quarkusio

# Preparar o ambiente para front-end
# NVM, NodeJS, NPM e Angular CLI

ENV NODE_VERSION=18.20.3
ENV NVM_DIR=/root/.nvm

RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
RUN . "$NVM_DIR/nvm.sh" && nvm install ${NODE_VERSION}
RUN . "$NVM_DIR/nvm.sh" && nvm use v${NODE_VERSION}
RUN . "$NVM_DIR/nvm.sh" && nvm alias default v${NODE_VERSION}

ENV PATH="/root/.nvm/versions/node/v${NODE_VERSION}/bin/:${PATH}"

EXPOSE 4200/tcp
EXPOSE 8080/tcp

RUN npm install -g @angular/cli
RUN npm install -g json-server
