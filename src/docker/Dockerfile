FROM lab.0x09.de:61000/homelab/base:latest
LABEL maintainer="hello@b4sh.de"
# update container debian to latest patch files
RUN apt-get update && apt-get upgrade -y
RUN apt-get install wget -y
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

#Comment in for development purposes
#ENTRYPOINT ["tail", "-f", "/dev/null"]

