# Use the official Jenkins LTS image as the base image
FROM --platform=linux/amd64  jenkins/jenkins:lts

# Switch to the root user temporarily
USER root


# Install Docker
RUN apt-get update && \
    apt-get install -y docker.io && \
    usermod -aG docker jenkins

# Switch back to the Jenkins user
USER jenkins

