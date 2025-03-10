FROM jenkins/jenkins:jdk21
# Switch to root user
USER root
# Install dependencies
RUN apt-get update && \
    apt-get install -y \
    ca-certificates \
    curl \
    gnupg \
    lsb-release \
    sudo \
    && curl -fsSL https://get.docker.com -o get-docker.sh && \
    sh get-docker.sh && \
    rm get-docker.sh
# Add Jenkins user to the Docker group
RUN usermod -aG docker jenkins
# Switch back to Jenkins user
USER jenkins
# Expose Jenkins default port
EXPOSE 8080
EXPOSE 50000
# Define the entrypoint
ENTRYPOINT ["/usr/bin/tini", "--", "/usr/local/bin/jenkins.sh"]