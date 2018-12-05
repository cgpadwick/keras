# The virtualenv requires bash.
SHELL = /bin/bash

BASEDIR := $(dir $(abspath $(lastword $(MAKEFILE_LIST))))
VENV := $(BASEDIR)keras_env/
GPU := 0

virtualenv:
	test -d $(VENV) || ( virtualenv -p python3 --system-site-packages $(VENV) ) 
	. $(VENV)bin/activate && pip install --upgrade pip
ifeq ($(GPU), 1)
	. $(VENV)bin/activate && pip install -r requirements_gpu.txt
else
	. $(VENV)bin/activate && pip install -r requirements_gpu.txt
endif

all: virtualenv
