########################################################################################
# Teste de criação de container com DockerFile, para rodar o SapGUI.
# Tentar futuramente rodar um cliente VPN para poder conectar
# Referências:
#  * http://fabiorehm.com/blog/2014/09/11/running-gui-apps-with-docker/
########################################################################################
# Line to Build:
#   $ sudo docker build -t="thalesvb/platingui:730r9" .
# Line to run:
#   $ sudo docker run -v /tmp/.X11-unix:/tmp/.X11-unix --rm -e DISPLAY -it thalesvb/platingui:730r9
########################################################################################
# If you are unable to connect, maybe X server is running with protection,
#   $ xhost +local:(username)
########################################################################################
# Port 3200 to connect on default SAP installations

FROM debian:7
MAINTAINER Thales Batista "thalesvb@live.com"
# Constantes
# Remove noninteractive messages from apt
ENV DEBIAN_FRONTEND=noninteractive \
# UserID and GroupID for developer user
    uid=1000 \
    gid=1000

# Dependências
# Java (http://www.webupd8.org/2014/03/how-to-install-oracle-java-8-in-debian.html)
RUN echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" | tee /etc/apt/sources.list.d/webupd8team-java.list && \
    echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" | tee -a /etc/apt/sources.list.d/webupd8team-java.list&& \
    echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections && \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886

# Installing java
RUN apt-get update && \
    apt-get install -q -y \
    oracle-java7-installer \
    --no-install-recommends && \
    apt-get clean

# Installing specific libraries to make sapgui start
RUN apt-get update && \
    apt-get install -q -y \
    sudo \
    libxext6 \
    libxrender1 \
    libxtst6 \
    --no-install-recommends && \
    apt-get clean

# Create user directory permissions specified on ENV vars
RUN mkdir -p /home/developer && \
    echo "developer:x:${uid}:${gid}:Developer,,,:/home/developer:/bin/bash" >> /etc/passwd && \
    echo "developer:x:${uid}:" >> /etc/group && \
    echo "developer ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/developer && \
    chmod 0440 /etc/sudoers.d/developer && \
    chown ${uid}:${gid} -R /home/developer

USER developer
ENV HOME /home/developer

# Copy PlatinGUI and its insallconfig to container
COPY PlatinGUI730Linux_9-20008877.JAR sapgui_inst.conf /home/developer/

# Run installer
RUN java -jar /home/developer/PlatinGUI730Linux_9-20008877.JAR --nogui -i sapgui_inst.conf
# Set to start PlatinGUI when container starts
ENTRYPOINT ["/home/developer/SAPClients/SAPGUI/bin/guilogon"]
# Show help of PlatinGUI when starting
#CMD ["-?"]