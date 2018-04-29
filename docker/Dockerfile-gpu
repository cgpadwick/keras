FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04

RUN apt-get update && apt-get install -y \
	python-pip \
	python-dev \
	python-virtualenv \
	build-essential \
	cmake \
	git \
	unzip \
	pkg-config \
	libopenblas-dev \
	liblapack-dev \
	vim \
	python-numpy \
	python-scipy \
	python-matplotlib \
	python-yaml \
	libhdf5-serial-dev \
	python-h5py \
	graphviz \
	python-opencv \
	sudo

#Create a user called ubuntu
RUN useradd -m ubuntu && echo "ubuntu:ubuntu" | chpasswd && adduser ubuntu sudo
USER ubuntu
WORKDIR /home/ubuntu

# Copy files that we need.
COPY requirements-gpu.txt /home/ubuntu

USER root
RUN pip install --upgrade pip
WORKDIR /home/ubuntu
RUN pip install -r requirements-gpu.txt

USER ubuntu
WORKDIR /home/ubuntu
RUN mkdir code
WORKDIR /home/ubuntu/code

#Clone the repo for running benchmarks.
RUN git clone https://github.com/tensorflow/benchmarks.git

#Clone the keras repo.
USER ubuntu
RUN git clone https://github.com/fchollet/keras
USER root 
RUN cd keras; python setup.py install

#Copy a validation script and a script for running benchmarks.
USER ubuntu
WORKDIR /home/ubuntu
ADD tf_validate.py /home/ubuntu
ADD run_benchmark.bsh /home/ubuntu
RUN git clone https://github.com/tensorflow/benchmarks.git

