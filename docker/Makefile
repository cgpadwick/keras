SHELL = /bin/bash

GPU := 1
DOCKERCMD = nvidia-docker
DISTRO := ubuntu-16.04
TFVER := $(shell cat requirements-gpu.txt | grep tensorflow | tr -d '[:space:]' | sed -e s/==/-/g)
TAG = cgpadwick/keras-containers:keras-$(DISTRO)-$(TFVER)
DOCKERFILE = Dockerfile-gpu
ifeq ($(GPU), 0)
  DOCKERCMD = docker
  TFVER := $(shell cat requirements-cpu.txt | grep tensorflow | tr -d '[:space:]' | sed -e s/==/-/g)
  TAG = cgpadwick/keras-containers:keras-$(DISTRO)-$(TFVER)
  DOCKERFILE = Dockerfile-cpu
endif

docker:
	sudo $(DOCKERCMD) build -t $(TAG) -f $(DOCKERFILE) .

test: docker
	sudo $(DOCKERCMD) run -it -p 8888:8888 $(TAG) /bin/bash -c "cd code/keras/; python examples/mnist_cnn.py"

smoketest: docker
	sudo $(DOCKERCMD) run -it -p 8888:8888 $(TAG) /bin/bash -c "python tf_validate.py"
ifeq ($(GPU), 1)
	sudo $(DOCKERCMD) run -it -p 8888:8888 $(TAG) /bin/bash -c "nvidia-smi"
	sudo $(DOCKERCMD) run -it -p 8888:8888 $(TAG) /bin/bash -c "./run_benchmark.bsh"
endif

dockerclean:
	- docker stop $(docker ps -a -q)
	- docker rm -f $(docker ps -a -q)
	- docker images -q | xargs docker rmi -f

.PHONY: setup_host
setup_host:
ifeq ($(GPU), 1)
	sudo ./install_cuda.bsh
	sudo ./install_docker.bsh
	sudo ./install_nvidia_docker.bsh
else
	sudo ./install_docker.bsh
endif

.PHONY: push
push:	
	sudo docker push cgpadwick/keras-containers

