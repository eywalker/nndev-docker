# Build nndev with support cuDNN v5
FROM nvidia/cuda:8.0-cudnn5-devel-ubuntu16.04
MAINTAINER Edgar Y. Walker <edgar.walker@gmail.com>

ENV LANG C.UTF-8

# Install essential Ubuntu packages, and upgrade pip
RUN apt-get update &&\
    apt-get install -y software-properties-common \
                       build-essential \
                       cmake \
                       git \
                       wget \
                       vim \
                       curl \
                       zip \
                       zlib1g-dev \
                       unzip \ 
                       pkg-config \
                       libblas-dev \
                       liblapack-dev \
                       python-dev \
                       python3-dev \
                       python3-pip \
                       python3-tk \
                       python3-wheel \
                       swig &&\
    ln -s /usr/bin/python3 /usr/local/bin/python &&\
    ln -s /usr/bin/pip3 /usr/local/bin/pip &&\
    pip install --upgrade pip &&\
    apt-get clean &&\
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install essential Python packages
RUN pip3 --no-cache-dir install \
         cython \
         numpy \
         matplotlib \
         scipy \
         pandas \
         jupyter \
         scikit-learn \
         seaborn \
         datajoint \
         tensorflow-gpu \
         http://download.pytorch.org/whl/cu80/torch-0.1.11.post5-cp35-cp35m-linux_x86_64.whl \
         torchvision \
         theano \
         lasagne \
         keras

# Install libgpuarray and pygpu
WORKDIR /src
RUN git clone https://github.com/Theano/libgpuarray.git &&\
    cd libgpuarray &&\
    mkdir Build && cd Build &&\
    cmake .. -DCMAKE_BUILD_TYPE=Release &&\
    make && make install && cd .. &&\
    python3 setup.py build &&\
    python3 setup.py install &&\
    ldconfig



# Add .theanorc
COPY theanorc /root/.theanorc

# Add profiling library support
ENV LD_LIBRARY_PATH /usr/local/cuda/extras/CUPTI/lib64:${LD_LIBRARY_PATH}
