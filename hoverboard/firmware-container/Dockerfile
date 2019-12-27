FROM ubuntu:bionic
 
WORKDIR /tmp

RUN apt update
RUN apt install -y software-properties-common
RUN add-apt-repository ppa:team-gcc-arm-embedded/ppa
RUN apt install -y gcc-arm-embedded build-essential openocd

CMD make
