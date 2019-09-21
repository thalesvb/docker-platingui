########################################################################################
# Line to Build:
#   $ docker build -t="thalesvb/platingui:750r7" .
# Line to run:
#   $ docker run -v /tmp/.X11-unix:/tmp/.X11-unix --rm -e DISPLAY -it --net=host thalesvb/platingui:750r7
########################################################################################
# If you are unable to connect, maybe X server is running with protection,
#   $ xhost +local:(username)
########################################################################################
# Port 3200 to connect on default SAP installations

FROM debian:10
MAINTAINER Thales Batista "thalesvb@live.com"

ARG platingui_installer=PlatinGUI750_7-80002496.JAR

# Remove noninteractive messages from apt
ENV DEBIAN_FRONTEND=noninteractive \
# UserID and GroupID for developer user
    uid=1000 \
    gid=1000

# Installing OpenJDK and OpenJFX
RUN apt-get update && \
    apt-get install -q -y \
    openjdk-11-jre-headless \
    openjdk-11-jre \
    openjfx \
    --no-install-recommends && \
    apt-get clean

# Create user directory permissions specified on ENV vars
RUN mkdir -p /home/developer && \
    echo "developer:x:${uid}:${gid}:Developer,,,:/home/developer:/bin/bash" >> /etc/passwd && \
    echo "developer:x:${uid}:" >> /etc/group && \
    chown ${uid}:${gid} -R /home/developer

USER developer
ENV HOME /home/developer

# Copy PlatinGUI and its install config to container
COPY ${platingui_installer} sapgui_inst.conf /home/developer/

# Run installer
# -a argument is to indicate OpenJFX libraty path (why they didn't make it available in response file?)
RUN java -jar /home/developer/${platingui_installer} --nogui -a /usr/share/openjfx/lib -i sapgui_inst.conf && \
    rm /home/developer/${platingui_installer}

# Set to start PlatinGUI when container starts
ENTRYPOINT ["/home/developer/SAPClients/SAPGUI/bin/guilogon"]
# Show help of PlatinGUI when starting
#CMD ["-?"]
