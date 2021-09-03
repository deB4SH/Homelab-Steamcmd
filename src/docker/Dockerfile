FROM ghcr.io/deb4sh/homelab-debian:0.10.10
LABEL maintainer="github@b4sh.de"
# update container debian to latest patch files
RUN dpkg --add-architecture i386
RUN apt-get update && apt-get upgrade -y
RUN apt-get install wget lib32stdc++6 lib32gcc1 ca-certificates nano libpulse0:i386 libdbus-1-3:i386 libsystemd0:i386 libsdl2-2.0-0:i386 curl -y --allow-remove-essential
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

