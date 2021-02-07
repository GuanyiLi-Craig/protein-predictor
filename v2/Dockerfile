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

#FROM nvidia/cuda:11.1.1-cudnn8-devel-ubuntu20.04
FROM nvidia/cuda:10.2-cudnn7-devel-ubuntu18.04
ENV LANG C.UTF-8
ENV DEBIAN_FRONTEND="noninteractive" TZ="Europe/London"
ADD ncbi-blast_2.7.1+-2_amd64.deb /tmp/ncbi-blast_2.7.1+-2_amd64.deb
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
# darknet
# ------------------------------------------------------------------

    $GIT_CLONE https://github.com/pjreddie/darknet.git ~/darknet && \
    cd ~/darknet && \
    sed -i 's/GPU=0/GPU=1/g' ~/darknet/Makefile && \
    sed -i 's/CUDNN=0/CUDNN=1/g' ~/darknet/Makefile && \
    make -j"$(nproc)" && \
    cp ~/darknet/include/* /usr/local/include && \
    cp ~/darknet/*.a /usr/local/lib && \
    cp ~/darknet/*.so /usr/local/lib && \
    cp ~/darknet/darknet /usr/local/bin && \

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
# boost
# ------------------------------------------------------------------

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        libboost-all-dev \
        && \

# ==================================================================
# chainer
# ------------------------------------------------------------------

    $PIP_INSTALL \
        cupy \
        chainer \
        && \

# ==================================================================
# jupyter
# ------------------------------------------------------------------

    $PIP_INSTALL \
        jupyter \
        && \

# ==================================================================
# mxnet
# ------------------------------------------------------------------

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        libatlas-base-dev \
        graphviz \
        && \

    $PIP_INSTALL \
        mxnet-cu102 \
        graphviz \
        && \

# ==================================================================
# onnx
# ------------------------------------------------------------------

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        protobuf-compiler \
        libprotoc-dev \
        && \

    $PIP_INSTALL \
        --no-binary onnx onnx \
        && \

    $PIP_INSTALL \
        onnxruntime \
        && \

# ==================================================================
# paddle
# ------------------------------------------------------------------

    $PIP_INSTALL \
        paddlepaddle-gpu \
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
# theano
# ------------------------------------------------------------------

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        libblas-dev \
        && \

    wget -qO- https://github.com/Theano/libgpuarray/archive/v0.7.6.tar.gz | tar xz -C ~ && \
    cd ~/libgpuarray* && mkdir -p build && cd build && \
    cmake -D CMAKE_BUILD_TYPE=RELEASE \
          -D CMAKE_INSTALL_PREFIX=/usr/local \
          .. && \
    make -j"$(nproc)" install && \
    cd ~/libgpuarray* && \
    python setup.py build && \
    python setup.py install && \

    printf '[global]\nfloatX = float32\ndevice = cuda0\n\n[dnn]\ninclude_path = /usr/local/cuda/targets/x86_64-linux/include\n' > ~/.theanorc && \

    $PIP_INSTALL \
        https://github.com/Theano/Theano/archive/master.zip \
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
# lasagne
# ------------------------------------------------------------------

    $GIT_CLONE https://github.com/Lasagne/Lasagne ~/lasagne && \
    cd ~/lasagne && \
    $PIP_INSTALL \
        . && \

# ==================================================================
# opencv
# ------------------------------------------------------------------

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        libatlas-base-dev \
        libgflags-dev \
        libgoogle-glog-dev \
        libhdf5-serial-dev \
        libleveldb-dev \
        liblmdb-dev \
        libprotobuf-dev \
        libsnappy-dev \
        protobuf-compiler \
        && \

    $GIT_CLONE --branch 4.5.1 https://github.com/opencv/opencv ~/opencv && \
    mkdir -p ~/opencv/build && cd ~/opencv/build && \
    cmake -D CMAKE_BUILD_TYPE=RELEASE \
          -D CMAKE_INSTALL_PREFIX=/usr/local \
          -D WITH_IPP=OFF \
          -D WITH_CUDA=OFF \
          -D WITH_OPENCL=OFF \
          -D BUILD_TESTS=OFF \
          -D BUILD_PERF_TESTS=OFF \
          -D BUILD_DOCS=OFF \
          -D BUILD_EXAMPLES=OFF \
          .. && \
    make -j"$(nproc)" install && \
    ln -s /usr/local/include/opencv4/opencv2 /usr/local/include/opencv2 && \

# ==================================================================
# sonnet
# ------------------------------------------------------------------

    $PIP_INSTALL \
        tensorflow_probability \
        "dm-sonnet>=2.0.0b0" --pre \
        && \

# ==================================================================
# caffe
# ------------------------------------------------------------------

    apt-get update && \
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        caffe-cuda \
        && \
# ==================================================================
# cntk
# ------------------------------------------------------------------

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        openmpi-bin \
        libpng-dev \
        libjpeg-dev \
        libtiff-dev \
        && \

    # Fix ImportError for CNTK
    ln -s /usr/lib/x86_64-linux-gnu/libmpi_cxx.so.20 /usr/lib/x86_64-linux-gnu/libmpi_cxx.so.1 && \
    ln -s /usr/lib/x86_64-linux-gnu/libmpi.so.20.10.1 /usr/lib/x86_64-linux-gnu/libmpi.so.12 && \

    wget --no-verbose -O - https://github.com/01org/mkl-dnn/releases/download/v0.14/mklml_lnx_2018.0.3.20180406.tgz | tar -xzf - && \
    cp mklml*/* /usr/local -r && \

    wget --no-verbose -O - https://github.com/01org/mkl-dnn/archive/v0.14.tar.gz | tar -xzf - && \
    cd *-0.14 && mkdir build && cd build && \
    ln -s /usr/local external && \
    cmake -D CMAKE_BUILD_TYPE=RELEASE \
          -D CMAKE_INSTALL_PREFIX=/usr/local \
          .. && \
    make -j"$(nproc)" install && \

    $PIP_INSTALL \
        cntk-gpu

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

RUN cd /plmDCA/plmDCA_asymmetric_v2/functions/; for i in *.c; do octave --eval "mex $i";done && \
    cd /plmDCA/plmDCA_asymmetric_v2/3rd_party_code/minFunc/; for i in *.c; do octave --eval "mex $i"; done && \

    cp /alphafold_pytorch/plmDCA.m /plmDCA/plmDCA_asymmetric_v2/plmDCA.m

# ==================================================================
# alphafold pipeline
# ------------------------------------------------------------------
# RUN git clone https://github.com/GuanyiLi-Craig/alphafold_pipeline.git

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

RUN dpkg -i /tmp/ncbi-blast_2.7.1+-2_amd64.deb

# ==================================================================
# config & cleanup
# ------------------------------------------------------------------

RUN ldconfig && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* ~/*

EXPOSE 8888 6006
