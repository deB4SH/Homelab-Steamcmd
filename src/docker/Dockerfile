FROM lab.0x09.de:61000/homelab/base:latest
LABEL maintainer="hello@b4sh.de"
# update container debian to latest patch files
RUN dpkg --add-architecture i386
RUN apt-get update && apt-get upgrade -y
RUN apt-get install wget lib32stdc++6 lib32gcc1 ca-certificates nano libsdl2-2.0-0:i386 curl -y
# add steamcmd user and executable
RUN adduser steam
USER steam
WORKDIR /home/steam
RUN mkdir steamcmd
WORKDIR /home/steam/steamcmd
RUN wget http://media.steampowered.com/installer/steamcmd_linux.tar.gz
RUN tar -xvzf steamcmd_linux.tar.gz
# add executable directory to path
ENV PATH=/home/steam/steamcmd:$PATH
RUN ./steamcmd.sh +quit
#Comment in for development purposes
#ENTRYPOINT ["tail", "-f", "/dev/null"]

