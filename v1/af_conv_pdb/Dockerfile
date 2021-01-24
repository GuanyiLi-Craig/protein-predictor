FROM ubuntu

RUN apt update
RUN apt-get install -y python3-pip
RUN apt-get install -y git
RUN apt-get install -y vim

ENV DEBIAN_FRONTEND="noninteractive" TZ="Europe/London"

RUN apt-get install -y pymol

ADD pyrosetta-2020.50+release.1295438cd4b-cp38-cp38-linux_x86_64.whl pyrosetta-2020.50+release.1295438cd4b-cp38-cp38-linux_x86_64.whl

RUN pip3 install pyrosetta-2020.50+release.1295438cd4b-cp38-cp38-linux_x86_64.whl
RUN pip3 install tensorflow
RUN pip3 install scipy
