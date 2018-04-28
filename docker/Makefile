SHELL = /bin/bash

docker:
	nvidia-docker build -t cgpadwick/keras-containers:keras-gpu -f Dockerfile .

.PHONY: setup_host
setup_host: 
	sudo ./setup_host.bsh

test: docker
	nvidia-docker run -it -p 8888:8888 cgpadwick/keras-containers:keras-gpu /bin/bash -c "cd code/keras/; python examples/mnist_cnn.py"

smoketest: docker
	nvidia-docker run -it -p 8888:8888 cgpadwick/keras-containers:keras-gpu /bin/bash -c "nvidia-smi"
	nvidia-docker run -it -p 8888:8888 cgpadwick/keras-containers:keras-gpu /bin/bash -c "python tf_validate.py"
	nvidia-docker run -it -p 8888:8888 cgpadwick/keras-containers:keras-gpu /bin/bash -c "./run_benchmark.bsh"

clean:
	- docker stop $(docker ps -a -q)
	- docker rm -f $(docker ps -a -q)
	- docker images -q | xargs docker rmi -f

.PHONY: push
push:	
	docker push cgpadwick/keras-containers

