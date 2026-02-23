jagadish@jenkins:/var/lib/jenkins/mtdevcredits/linkgenerator$ ls -lrt
total 136240
-rwxrwxrwx 1 prasad prasad        526 Apr  8  2024 Dockerfile
-rwxrwxrwx 1 prasad prasad        567 Apr 17  2024 application-clientsprod.properties
-rwxrwxrwx 1 prasad jenkins 139494739 Apr 17  2024 LinkProviderService-0.0.1-SNAPSHOT.jar
jagadish@jenkins:/var/lib/jenkins/mtdevcredits/linkgenerator$ cat Dockerfile
FROM openjdk:8-jdk-alpine
RUN mkdir /home/LinkGenerator && \
    mkdir /home/LinkGenerator/config
COPY LinkProviderService-0.0.1-SNAPSHOT.jar /home/LinkGenerator
COPY application-clientsprod.properties /home/LinkGenerator/config
WORKDIR /home/LinkGenerator/config
EXPOSE 5004
ENTRYPOINT ["java", "-Xms1024m", "-Xmx2048m", "-Dspring.profiles.active=clientsprod", "-jar", "/home/LinkGenerator/LinkProviderService-0.0.1-SNAPSHOT.jar","-Dspring.config.location=file:/home/LinkGenerator/config/application-clientsprod.properties"]
---------------------------------------------------------------
jagadish@jenkins:/var/lib/jenkins/proddockerfiles/australia_dockerfiles/llm_feeder$ cat Dockerfile
FROM openjdk:8-jdk-alpine
RUN mkdir /home/llmfeeder && \
    mkdir /home/llmfeeder/config
COPY llmfeeder-0.0.1-SNAPSHOT.jar /home/llmfeeder
COPY application-clientsprod.properties /home/llmfeeder/config
WORKDIR /home/llmfeeder/config
EXPOSE 9011
ENV JAVA_OPTS="-XX:PermSize=1024m -XX:MaxPermSize=3072m"
ENTRYPOINT ["java", "-Xms1024m", "-Xmx5120m", "-XX:+HeapDumpOnOutOfMemoryError", "-XX:HeapDumpPath=/home/llmfeeder/heapdump.hprof", "-Dspring.profiles.active=clientsprod", "-jar", "/home/llmfeeder/llmfeeder-0.0.1-SNAPSHOT.jar","-Dspring.config.location=file:/home/llmfeeder/config/application-clientsprod.properties"]
----------------------------------------
jagadish@jenkins:/var/lib/jenkins/mtdevcredits/jumpreeqa/ems$ cat Dockerfile
FROM openjdk:8-jdk-alpine
RUN mkdir /home/EMS && \
         mkdir /home/EMS/config
COPY EMS-0.0.1-SNAPSHOT.jar  /home/EMS
COPY application-clientsprod.properties /home/EMS/config
WORKDIR /home/EMS/config
EXPOSE 3000
ENTRYPOINT /usr/sbin/crond && java -Xms1024m -Xmx2560m -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/home/EMS/heapdump.hprof -Dspring.profiles.active=clientsprod -jar /home/EMS/EMS-0.0.1-SNAPSHOT.jar -Dspring.config.lcation=file:/home/EMS/config/application-clientsprod.properties
-----------------------------------------------------
FROM eclipse-temurin:8-jre-alpine
WORKDIR /home/EMS
RUN mkdir -p config
COPY EMS-0.0.1-SNAPSHOT.jar .
COPY application-clientsprod.properties config/
EXPOSE 3000
ENTRYPOINT ["java",
 "-Xms512m",
 "-Xmx2048m",
 "-XX:+HeapDumpOnOutOfMemoryError",
 "-XX:HeapDumpPath=/home/EMS/heapdump.hprof",
 "-Dspring.profiles.active=clientsprod",
 "-Dspring.config.location=file:/home/EMS/config/application-clientsprod.properties",
 "-jar",
 "/home/EMS/EMS-0.0.1-SNAPSHOT.jar"]
-----------------------------------------------------------------------------------------------------------------------------------------------------------
jagadish@jenkins:/var/lib/jenkins/mtdev/jumpreeqa/Analytics_v4$ cat Dockerfile
FROM openjdk:8-jdk-alpine
RUN mkdir /home/SpaceMngmtv2 && \
    mkdir /home/SpaceMngmtv2/config
COPY analyticsservice-0.0.1-SNAPSHOT.jar /home/SpaceMngmtv2
COPY application-clientsprod.properties /home/SpaceMngmtv2/config
#COPY ClusreMonitorClient-0.0.1-SNAPSHOT.jar /home/SpaceMngmtv2
#COPY cap.sh /home/SpaceMngmtv2/cap.sh
#RUN echo ' *  *  *  *  * sh /home/SpaceMngmtv2/cap.sh' >> /etc/crontabs/root
#RUN echo ' */5 * * * * java  -Xms250m -Xmx512m -jar -Dregion=dev -Dspring.profiles.active=prod /home/SpaceMngmtv2/ClusreMonitorClient-0.0.1-SNAPSHOT.jar > /dev/null 2>&1 &' >> /etc/crontabs/root
EXPOSE 4888
WORKDIR /home/SpaceMngmtv2/config
ENTRYPOINT /usr/sbin/crond && java -Xms1024m -Xmx3096m -Dspring.profiles.active=clientsprod -jar /home/SpaceMngmtv2/analyticsservice-0.0.1-SNAPSHOT.jar -Dspring.config.location=file:/home/SpaceMngmtv2/config/application-clientsprod.properties
-----------------------------------------------------------------
jagadish@jenkins:/var/lib/jenkins/proddockerfiles/singapore_dockerfiles/keymanagement$ cat Dockerfile
FROM openjdk:8-jdk-alpine
RUN mkdir /home/keymanagement && \
    mkdir /home/keymanagement/config
COPY *.jar /home/keymanagement
COPY application-clientsprod.properties /home/keymanagement/config
WORKDIR /home/keymanagement/config
EXPOSE 6970
ENTRYPOINT ["java", "-Xms1024m", "-Xmx3072m",  "-Dspring.profiles.active=clientsprod", "-jar", "/home/keymanagement/KeyManagement-0.0.1-SNAPSHOT.jar","-Dspring.config.location=file:/home/keymanagement/config/application-clientsprod.properties"]
-------------------------------------------------------------------------------
Draining a node -- Cordon a Node
---------------------------------------
jagadish@jenkins:/var/lib/jenkins/mtdevold/dbsreplica/spacemanagementdashboard$ cat Dockerfile
FROM tomcat:latest
RUN chmod -R 777 /usr/local/tomcat/webapps/
COPY spacemanagementV2.war /usr/local/tomcat/webapps/
RUN mkdir -p /usr/local/tomcat/webapps/assets
ADD assets /usr/local/tomcat/webapps/assets
ADD webviewV2 /usr/local/tomcat/webapps/webviewV2
ADD webviewV3 /usr/local/tomcat/webapps/webviewV3
---------------------------------------
jagadish@jenkins:/var/lib/jenkins/mtdev/propertyguru/spacemanagementdashboard$ cat Dockerfile
FROM tomcat:9.0-alpine
COPY spacemanagementV2.war /usr/local/tomcat/webapps/spacemanagementV2.war
ADD assets /usr/local/tomcat/webapps/assets
ADD webview /usr/local/tomcat/webapps/webview
ADD webviewV2 /usr/local/tomcat/webapps/webviewV2
ADD webviewV3 /usr/local/tomcat/webapps/webviewV3
ADD statusWebview /usr/local/tomcat/webapps/statusWebview
ADD eventsaiwebview /usr/local/tomcat/webapps/eventsaiwebview
CMD ["catalina.sh", "run"]
----------------------------------------
jagadish@jenkins:/var/lib/jenkins/mtdevcredits/jumpreeqa/saasadmin$ cat Dockerfile
FROM tomcat:9.0-alpine
RUN rm -rf /usr/local/tomcat/webapps/*
COPY saasAdminDashboard.war /usr/local/tomcat/webapps/
#COPY community_v2 /usr/local/tomcat/webapps/
RUN unzip /usr/local/tomcat/webapps/saasAdminDashboard.war
RUN mkdir -p /usr/local/tomcat/webapps/community_v2 && \
    mkdir /usr/local/tomcat/webapps/UserDocs && \
    mkdir -p /usr/local/tomcat/webapps/communityv2 && \
    mkdir -p /usr/local/tomcat/webapps/communityv3
#COPY web.xml /usr/local/tomcat/conf/
#ADD UserDocs /usr/local/tomcat/webapps/UserDocs
#ADD tfadocs /usr/local/tomcat/webapps/tfadocs
#ADD nintendodocs /usr/local/tomcat/webapps/nintendodocs
#ADD mdlzdocs /usr/local/tomcat/webapps/mdlzdocs
#ADD ifmdocs /usr/local/tomcat/webapps/ifmdocs
ADD Events /usr/local/tomcat/webapps/community_v2/Events
ADD Offers /usr/local/tomcat/webapps/community_v2/Offers
ADD Polls /usr/local/tomcat/webapps/community_v2/Polls
ADD PollsResult /usr/local/tomcat/webapps/community_v2/PollsResult
ADD webFAQ /usr/local/tomcat/webapps/community_v2/webFAQ
ADD Events /usr/local/tomcat/webapps/communityv3/Events
ADD Offers /usr/local/tomcat/webapps/communityv3/Offers
ADD Polls /usr/local/tomcat/webapps/communityv3/Polls
ADD PollsResult1 /usr/local/tomcat/webapps/communityv3/PollsResult
ADD webFAQ1 /usr/local/tomcat/webapps/communityv3/webFAQ
ADD Events /usr/local/tomcat/webapps/communityv2/Events
ADD Offers /usr/local/tomcat/webapps/communityv2/Offers
ADD Polls /usr/local/tomcat/webapps/communityv2/Polls
ADD PollsResult1 /usr/local/tomcat/webapps/communityv2/PollsResult
ADD webFAQ1 /usr/local/tomcat/webapps/communityv2/webFAQ
ADD zoomverify /usr/local/tomcat/webapps/zoomverify
ADD fasttracknew /usr/local/tomcat/webapps/communityv2/fasttracknew
RUN /bin/sh -c cd /usr/local/tomcat/webapps/saasAdminDashboard & cp -avr assets /usr/local/tomcat/webapps/
CMD ["catalina.sh", "run"]
--------------------------------------------------------------------------------------
jagadish@jenkins:/var/lib/jenkins/mtdevcredits/propertyguru/ReactWebApp/code$ cat Dockerfile
# Use a specific Node.js Alpine image as the base image for the build stage
FROM node:20.5.1-alpine as builder

# Update and install necessary packages
RUN apk update && apk add --no-cache git

# Set the working directory inside the container
WORKDIR /frontend

# Copy package.json and package-lock.json
COPY package.json package-lock.json ./

# Clean npm cache and install dependencies
RUN npm cache clean --force
RUN npm install -g webpack webpack-cli
RUN npm install --legacy-peer-deps

# Copy the rest of the application code
COPY . .

# Build the application
RUN npm run web-build

# Use Tomcat image for the final stage
FROM tomcat:latest
RUN chmod -R 777 /usr/local/tomcat/webapps/

# Clean up existing webapps and set up necessary directories
RUN mkdir -p /usr/local/tomcat/webapps/assets && \
    mkdir -p /usr/local/tomcat/webapps/digitalApp/fonts

# Copy the built application and assets from the builder stage
COPY --from=builder /frontend/dist /usr/local/tomcat/webapps/digitalApp
COPY --from=builder /frontend/src/assests/ /usr/local/tomcat/webapps/assets/
COPY --from=builder /frontend/public/fonts /usr/local/tomcat/webapps/digitalApp/fonts/
-------------------------------------------------------------------------------------------------

FROM jfrogimg.sgp.dbs.com:8443/baseos_redhat/ubi8/openjdk-8:latest as csmt-ems

# Update and clean up
RUN microdnf update && \
    microdnf -y reinstall tzdata && \
    microdnf clean all

# Copy scripts and JAR files
COPY ./scripts /app/scripts
COPY ./jars /app/java
#COPY ./*.p12 /app/ssl/

RUN mkdir -p /app/lib && \
   chmod -R g+rwx /app  /app/lib

# Set permissions
#RUN chmod -R 775 /app
# Timezone configuration (if needed)
#RUN rm /etc/localtime && \
#ln -s /usr/share/zoneinfo/Asia/Singapore /etc/localtime

# Start the final stage
FROM scratch
COPY --from=csmt-ems . .

# Include SSL debugging in the Java CMD
CMD java -Dspring.profiles.active=${profile} -Xms3072m -Xmx5120m -jar /app/java/*.jar

=================================================================================================================================================
# Stage 1: Download dependencies as wheels
FROM python:3.11-slim AS wheel-downloader
WORKDIR /wheeldownload
COPY ./app/requirements.txt .
RUN apt-get update && apt-get install -y build-essential gcc && \
    pip install --upgrade pip && \
    mkdir /wheels && \
    pip download --no-cache-dir -r requirements.txt -d /wheels
# Stage 2: Final build using pre-downloaded wheels
FROM python:3.11-slim
WORKDIR /app
# Copy your application code
COPY ./app /app
# Copy wheels and requirements.txt from the first stage
COPY --from=wheel-downloader /wheels /app/wheels
COPY ./app/requirements.txt /app/requirements.txt
# Install dependencies from local wheel folder
RUN pip3 install --upgrade pip && \
    pip3 install --no-index --find-links=/app/wheels -r /app/requirements.txt
EXPOSE 5010
CMD ["gunicorn", "-b", "0.0.0.0:5010", "autostacking:app", "--timeout", "4000", "--log-level", "debug"]



prasad@jenkins:/var/lib/jenkins/mtdevold/dbsreplica_gcp/AlgorithmService$ docker build -t algorithm-service .
prasad@jenkins:/var/lib/jenkins/mtdevold/dbsreplica_gcp/AlgorithmService$ docker create --name temp-wheel-container algorithm-service
prasad@jenkins:/var/lib/jenkins/mtdevold/dbsreplica_gcp/AlgorithmService$ docker cp temp-wheel-container:/app/wheels ./wheels
prasad@jenkins:/var/lib/jenkins/mtdevold/dbsreplica_gcp/AlgorithmService$ zip -r wheels.zip wheels/





dbs

# Use Python 3.12 base image from your registry
FROM jfrogimg.sgp.dbs.com:8443/baseos_redhat/ubi8/python-312:latest

# Copy the zipped or bundled code directory into the container
COPY ./unzipBinary /app/unzipBinary

# List contents for debug (optional)
RUN echo "Unzipped folder structure:" && ls -lR /app/unzipBinary

# Set application root where autostacking.py and requirements.txt exist
WORKDIR /app/unzipBinary/AlgorithmService/app

# Install Python dependencies with timeout and no cache

RUN pip install --upgrade pip --index-url=https://jfrogbin.sgp.dbs.com:9443/artifactory/api/pypi/pypi-all/simple --trusted-host=jfrogbin.sgp.dbs.com:9443

RUN pip install --no-cache-dir --default-timeout=100 -r requirements.txt --index-url=https://jfrogbin.sgp.dbs.com:9443/artifactory/api/pypi/pypi-all/simple --trusted-host=jfrogbin.sgp.dbs.com:9443


# Start the application
CMD ["gunicorn", "-b", "0.0.0.0:5010", "autostacking:app", "--timeout", "4000", "--log-level", "debug"]


--------------------------------------------------------------------------------------------------

# --- STAGE 1: Build the application ---
FROM maven:3.9-eclipse-temurin-17 AS build_stage

# Set the working directory
WORKDIR /app

# Copy the pom.xml and source code
COPY pom.xml .
COPY src ./src

# Build the jar file (skipping tests for speed)
RUN mvn clean package -DskipTests

# --- STAGE 2: Create the final lightweight image ---
FROM eclipse-temurin:17-jre-alpine

WORKDIR /app

# Copy ONLY the built jar from the first stage
COPY --from=build_stage /app/target/my-app-1.0.jar app.jar

# Run the application
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
