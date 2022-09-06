FROM jenkins/jenkins

USER root

RUN apt-get update && apt-get install -y \
    sudo \
    openssh-server \
    python3 \
    python3-pip \
    sshpass \
    ansible

EXPOSE 8080 50000 22

# reset jenkins's password and setup passwordless sudo
RUN echo "jenkins:jenkins" | chpasswd
RUN echo "jenkins ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/10_jenkins

# create dir for keeping image files
RUN mkdir /data

USER jenkins