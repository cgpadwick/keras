# Get Up And Running With Keras Very Quickly

This repo was written to enable users to get up and running with Keras very quickly.  You may have heard of Francios Challot's book "Deep Learning With Python" (https://www.manning.com/books/deep-learning-with-python).  The book teaches you the ins and out of Keras by using jupyter notebooks.

Installation of Keras and Tensorflow can be pretty tricky.  If you want to run on the GPU, you have to make sure that the version of CUDA that you install is compatible with Tensorflow (https://www.tensorflow.org/).  The instructions in the book are out of date and do not work.  If you are not a grizzled deep learning veteran, you could get pretty frustrated.

This guide is intended to get you up and running on either the CPU or the GPU as quickly as possible.

## Quickstart

Clone this repo, setup your host (Ubuntu 16.04) with docker.  If your host machine has a cuda enabled GPU, set GPU=1. 
```
git clone git@github.com:cgpadwick/keras.git
cd keras/docker
make setup_host GPU=0
```

Clone the notebook code from the Deep Learning With Python repo.
```
cd ~
mkdir ~/data
cd data
git clone https://github.com/fchollet/deep-learning-with-python-notebooks
```

Start up a jupyter notebook inside the docker container.  The notebook code will be mounted in the container under /home/ubuntu/data.

For a GPU enabled docker:
```
sudo nvidia-docker run -it -p 8888:8888 -v ~/data:/home/ubuntu/data cgpadwick/keras-containers:keras-ubuntu-16.04-tensorflow-gpu-1.8.0 /bin/bash -c "jupyter notebook --ip 0.0.0.0"
```

For a CPU-only docker:
```
sudo docker run -it -p 8888:8888 -v ~/data:/home/ubuntu/data cgpadwick/keras-containers:keras-ubuntu-16.04-tensorflow-1.8.0 /bin/bash -c "jupyter notebook --ip 0.0.0.0"
```
Copy and paste the token printed on the screen (something like http://0.0.0.0:8888/?token=gobbledegook) into a browser and hopefully you will see something like this:

![Alt text](screenshot.png?raw=true "Keras notebook running in a docker container")

Use Ctrl-C to stop the notebook server and exit the docker container.

