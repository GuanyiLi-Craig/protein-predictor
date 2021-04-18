# ==================================================================
# module list
# ------------------------------------------------------------------
# python        3.6    (apt)
# torch         latest (git)
# jupyter       latest (pip)
# pytorch       latest (pip)
# tensorflow    latest (pip)
# jupyterlab    latest (pip)
# keras         latest (pip)
# pymol         latest (pip)
# pyrosetta     2020.50 package
# ==================================================================

FROM nvidia/cuda:10.2-cudnn7-devel-ubuntu18.04
ENV LANG C.UTF-8
ENV DEBIAN_FRONTEND="noninteractive" TZ="Europe/London"
ADD pyrosetta-2020.50+release.1295438cd4b-cp36-cp36m-linux_x86_64.whl /tmp/pyrosetta-2020.50+release.1295438cd4b-cp36-cp36m-linux_x86_64.whl

RUN APT_INSTALL="apt-get install -y --no-install-recommends" && \
    PIP_INSTALL="python -m pip --no-cache-dir install --upgrade" && \
    GIT_CLONE="git clone --depth 10" && \

    rm -rf /var/lib/apt/lists/* \
           /etc/apt/sources.list.d/cuda.list \
           /etc/apt/sources.list.d/nvidia-ml.list && \

    apt-get update && \

# ==================================================================
# tools
# ------------------------------------------------------------------

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        build-essential \
        apt-utils \
        ca-certificates \
        wget \
        git \
        vim \
        libssl-dev \
        curl \
        unzip \
        unrar \
        && \

    $GIT_CLONE https://github.com/Kitware/CMake ~/cmake && \
    cd ~/cmake && \
    ./bootstrap && \
    make -j"$(nproc)" install && \

# ==================================================================
# python
# ------------------------------------------------------------------

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        software-properties-common \
        && \
    add-apt-repository ppa:deadsnakes/ppa && \
    apt-get update && \
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        python3.6 \
        python3.6-dev \
        python3-distutils-extra \
        && \
    wget -O ~/get-pip.py \
        https://bootstrap.pypa.io/get-pip.py && \
    python3.6 ~/get-pip.py && \
    ln -s /usr/bin/python3.6 /usr/local/bin/python3 && \
    ln -s /usr/bin/python3.6 /usr/local/bin/python && \
    $PIP_INSTALL \
        setuptools \
        && \
    $PIP_INSTALL \
        numpy \
        scipy \
        pandas \
        cloudpickle \
        scikit-image>=0.14.2 \
        scikit-learn \
        matplotlib \
        Cython \
        tqdm

# ==================================================================
# jupyter
# ------------------------------------------------------------------

RUN python -m pip --no-cache-dir install \
        jupyter

# ==================================================================
# pytorch
# ------------------------------------------------------------------

RUN python -m pip --no-cache-dir install \
        future \
        numpy \
        protobuf \
        enum34 \
        pyyaml \
        typing \
        && \
    python -m pip --no-cache-dir install \
        --pre torch torchvision -f \
        https://download.pytorch.org/whl/nightly/cu102/torch_nightly.html

# ==================================================================
# tensorflow
# ------------------------------------------------------------------

RUN python -m pip --no-cache-dir install \
        tensorflow-gpu

# ==================================================================
# jupyterlab
# ------------------------------------------------------------------

RUN python -m pip --no-cache-dir install \
        jupyterlab

# ==================================================================
# keras
# ------------------------------------------------------------------

RUN python -m pip --no-cache-dir install \
        keras

# ==================================================================
# pymol
# ------------------------------------------------------------------

RUN apt-get install -y --no-install-recommends \
        pymol

# ==================================================================
# pyrosetta
# ------------------------------------------------------------------
RUN python -m pip install \
        /tmp/pyrosetta-2020.50+release.1295438cd4b-cp36-cp36m-linux_x86_64.whl

# ==================================================================
# hhsuit
# ------------------------------------------------------------------

RUN git clone https://github.com/soedinglab/hh-suite.git
RUN apt-get install -y --no-install-recommends \
          cmake \
          liboctave-dev

RUN mkdir -p /hh-suite/build

RUN cd /hh-suite/build && \
    cmake -DCMAKE_INSTALL_PREFIX=. .. && \
    make -j 4 && make install
    
ENV PATH "$PATH=/hh-suite/build/bin:/hh-suite/build/scripts:$PATH"

# ==================================================================
# Get Prospr
# ------------------------------------------------------------------
RUN cd /
RUN git clone https://github.com/dellacortelab/prospr.git

# ==================================================================
# generate pdb data
# ------------------------------------------------------------------
RUN git clone https://github.com/leklab/alphafoldv1.git

# ==================================================================
# config & cleanup
# ------------------------------------------------------------------

RUN ldconfig && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* ~/*

EXPOSE 8888 6006
