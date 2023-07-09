FROM jenkins/jenkins:lts

USER root

# Variables de entorno para la instalación de Maven
ARG MAVEN_VERSION

# Descarga e instala Maven
RUN curl -L -O https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/3.6.3/apache-maven-3.6.3-bin.tar.gz 
RUN ls -al
RUN tar xf apache-maven-${MAVEN_VERSION}-bin.tar.gz
RUN mv apache-maven-${MAVEN_VERSION} /usr/local/maven
RUN rm apache-maven-${MAVEN_VERSION}-bin.tar.gz


# Configura las variables de entorno de Maven
ENV MAVEN_HOME=/usr/local/maven
ENV PATH=${MAVEN_HOME}/bin:${PATH}

USER jenkins
