# Get Up And Running With Keras Very Quickly

This repo was written to enable users to get up and running with Keras very quickly.  You may have heard of Francios Challot's book "Deep Learning With Python" (https://www.manning.com/books/deep-learning-with-python).  The book teaches you the ins and out of Keras by using jupyter notebooks.

Installation of Keras and Tensorflow can be pretty tricky.  If you want to run on the GPU, you have to make sure that the version of CUDA that you install is compatible with Tensorflow (https://www.tensorflow.org/).  The instructions in the book are out of date and do not work.

## Quickstart

Clone this repo, setup your host (Ubuntu 16.04) with cuda and docker. 
```
git clone fdsaf
cd keras/docker
make setup_host
```

Clone the notebook code from the Deep Learning With Python repo.
```
cd ~
mkdir ~/data
cd data
git clone https://github.com/fchollet/deep-learning-with-python-notebooks
```

Start up a jupyter notebook inside the docker container.
```
nvidia-docker run -it -p 8888:8888 -v ~/data:/home/ubuntu/data cgpadwick/keras-containers:keras-gpu /bin/bash -c "jupyter notebook --ip 0.0.0.0"
```
Copy and paste the token printed on the screen (something like http://0.0.0.0:8888/?token=gobbledegook) into a browser and hopefully you will see something like this:

![Alt text](screenshot.png?raw=true "Keras notebook running in a docker container")

