FROM ubuntu

RUN apt-get update
RUN apt-get install -y apt-utils
RUN apt-get install -y build-essential
RUN apt-get install -y vim
RUN apt-get install -y wget
RUN apt-get install -y git

ENV DEBIAN_FRONTEND="noninteractive" TZ="Europe/London"

RUN apt-get install -y octave
RUN apt-get install -y cmake
RUN apt-get install -y alien
RUN apt-get install -y liboctave-dev

RUN apt-get install -y python3-pip
RUN pip3 install torch
RUN pip3 install scipy

RUN git clone https://github.com/soedinglab/hh-suite.git
RUN git clone https://github.com/magnusekeberg/plmDCA.git
RUN git clone https://github.com/GuanyiLi-Craig/alphafold_pytorch.git

RUN mkdir -p hh-suite/build
RUN mkdir software

RUN cd /hh-suite/build && \
    cmake -DCMAKE_INSTALL_PREFIX=. .. && \
    make -j 4 && make install && \
    export PATH="$(pwd)/bin:$(pwd)/scripts:$PATH"

RUN cd /plmDCA/plmDCA_asymmetric_v2/functions/; for i in *.c; do octave --eval "mex $i";done && \
    cd /plmDCA/plmDCA_asymmetric_v2/3rd_party_code/minFunc/; for i in *.c; do octave --eval "mex $i"; done

RUN cp /alphafold_pytorch/plmDCA.m /plmDCA/plmDCA_asymmetric_v2/

ADD ncbi-blast_2.7.1+-2_amd64.deb /software/ncbi-blast.deb
RUN dpkg -i /software/ncbi-blast.deb
