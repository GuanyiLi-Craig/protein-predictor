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
# ==================================================================

FROM nvidia/cuda:10.2-cudnn7-devel-ubuntu18.04
ENV LANG C.UTF-8

ADD protein_prodiction.ipynb /protein_prodiction.ipynb
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
        tqdm \
        && \

# ==================================================================
# torch
# ------------------------------------------------------------------

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        sudo \
        && \

    $GIT_CLONE https://github.com/nagadomi/distro.git ~/torch --recursive && \
    cd ~/torch && \
    bash install-deps && \
    sed -i 's/${THIS_DIR}\/install/\/usr\/local/g' ./install.sh && \
    ./install.sh && \

# ==================================================================
# jupyter
# ------------------------------------------------------------------

    $PIP_INSTALL \
        jupyter \
        && \

# ==================================================================
# pytorch
# ------------------------------------------------------------------

    $PIP_INSTALL \
        future \
        numpy \
        protobuf \
        enum34 \
        pyyaml \
        typing \
        && \
    $PIP_INSTALL \
        --pre torch torchvision -f \
        https://download.pytorch.org/whl/nightly/cu102/torch_nightly.html \
        && \

# ==================================================================
# tensorflow
# ------------------------------------------------------------------

    $PIP_INSTALL \
        tensorflow-gpu \
        && \

# ==================================================================
# jupyterlab
# ------------------------------------------------------------------

    $PIP_INSTALL \
        jupyterlab \
        && \

# ==================================================================
# keras
# ------------------------------------------------------------------

    $PIP_INSTALL \
        h5py \
        keras \
        && \

# ==================================================================
# Get Prospr
# ------------------------------------------------------------------
RUN cd /

RUN git clone https://github.com/dellacortelab/prospr.git

# ==================================================================
# alphafold pipeline
# ------------------------------------------------------------------

# ==================================================================
# generate pdb data
# ------------------------------------------------------------------
RUN git clone https://github.com/leklab/alphafoldv1.git

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
# config & cleanup
# ------------------------------------------------------------------

RUN ldconfig && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* ~/*

EXPOSE 8888 6006