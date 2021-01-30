# ==================================================================
# module list
# ------------------------------------------------------------------
# darknet       latest (git)
# python        3.8    (apt)
# torch         latest (git)
# chainer       latest (pip)
# jupyter       latest (pip)
# mxnet         latest (pip)
# onnx          latest (pip)
# paddle        latest (pip)
# pytorch       latest (pip)
# tensorflow    latest (pip)
# theano        latest (git)
# jupyterlab    latest (pip)
# keras         latest (pip)
# lasagne       latest (git)
# opencv        4.5.1  (git)
# sonnet        latest (pip)
# caffe         latest (git)
# cntk          latest (pip)
# ==================================================================

FROM nvidia/cuda:11.1.1-cudnn8-devel-ubuntu20.04
ENV LANG C.UTF-8
ENV DEBIAN_FRONTEND="noninteractive" TZ="Europe/London"
ADD ncbi-blast_2.7.1+-2_amd64.deb /tmp/ncbi-blast_2.7.1+-2_amd64.deb
ADD pyrosetta-2020.50+release.1295438cd4b-cp38-cp38-linux_x86_64.whl /tmp/pyrosetta-2020.50+release.1295438cd4b-cp38-cp38-linux_x86_64.whl
RUN rm -rf /var/lib/apt/lists/* \
           /etc/apt/sources.list.d/cuda.list \
           /etc/apt/sources.list.d/nvidia-ml.list && \

    apt-get update

# ==================================================================
# tools
# ------------------------------------------------------------------

RUN apt-get install -y --no-install-recommends \
        build-essential \
        apt-utils \
        ca-certificates \
        wget \
        git \
        vim \
        libssl-dev

# ==================================================================
# python
# ------------------------------------------------------------------

RUN apt-get install -y --no-install-recommends python3-pip && \
    pip3 --no-cache-dir install --upgrade \
        setuptools \
        && \
    pip3 --no-cache-dir install --upgrade \
        numpy \
        scipy \
        pandas \
        cloudpickle \
        scikit-image>=0.14.2 \
        scikit-learn \
        matplotlib \
        Cython \
        tqdm \
        torch

# ==================================================================
# boost
# ------------------------------------------------------------------

RUN apt-get install -y --no-install-recommends \
        libboost-all-dev

# ==================================================================
# jupyter
# ------------------------------------------------------------------

RUN pip3 --no-cache-dir install --upgrade \
        jupyter

# ==================================================================
# tensorflow
# ------------------------------------------------------------------

RUN pip3 --no-cache-dir install --upgrade \
        tensorflow-gpu

# ==================================================================
# jupyterlab
# ------------------------------------------------------------------

RUN pip3 --no-cache-dir install --upgrade \
        jupyterlab

# ==================================================================
# generate feature data
# ------------------------------------------------------------------
RUN cd /

RUN git clone https://github.com/soedinglab/hh-suite.git
RUN git clone https://github.com/magnusekeberg/plmDCA.git
RUN git clone https://github.com/GuanyiLi-Craig/alphafold_pytorch.git
RUN apt-get install -y --no-install-recommends \
          octave \
          cmake \
          liboctave-dev

RUN mkdir -p /hh-suite/build

RUN cd /hh-suite/build && \
    cmake -DCMAKE_INSTALL_PREFIX=. .. && \
    make -j 4 && make install && \
    export PATH="/hh-suite/build/bin:/hh-suite/build/scripts:$PATH"

RUN dpkg -i /tmp/ncbi-blast_2.7.1+-2_amd64.deb
    
RUN cd /plmDCA/plmDCA_asymmetric_v2/functions/; for i in *.c; do octave --eval "mex $i";done && \
    cd /plmDCA/plmDCA_asymmetric_v2/3rd_party_code/minFunc/; for i in *.c; do octave --eval "mex $i"; done && \

    cp /alphafold_pytorch/plmDCA.m /plmDCA/plmDCA_asymmetric_v2/plmDCA.m

# ==================================================================
# alphafold predict
# ------------------------------------------------------------------
#    $GIT_CLONE 


# ==================================================================
# generate pdb data
# ------------------------------------------------------------------

# ==================================================================
# pymol
# ------------------------------------------------------------------

RUN apt-get install -y --no-install-recommends \
        pymol

# ==================================================================
# pyrosetta
# ------------------------------------------------------------------

RUN pip3 --no-cache-dir install --upgrade \
        /tmp/pyrosetta-2020.50+release.1295438cd4b-cp38-cp38-linux_x86_64.whl

# ==================================================================
# config & cleanup
# ------------------------------------------------------------------

RUN ldconfig && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* ~/*

EXPOSE 8888 6006
