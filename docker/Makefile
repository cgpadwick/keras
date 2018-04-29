SHELL = /bin/bash

GPU := 1
DOCKERCMD = nvidia-docker
TAG = cgpadwick/keras-containers:keras-gpu
DOCKERFILE = Dockerfile-gpu
ifeq ($(GPU), 0)
  DOCKERCMD = docker
  TAG = cgpadwick/keras-containers:keras-cpu
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
	sudo ./setup_host.bsh
else
	sudo ./install_docker.bsh
endif

.PHONY: push
push:	
	docker push cgpadwick/keras-containers

