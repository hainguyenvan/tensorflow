FROM python:3

MAINTAINER hainv.work.it@gmail.com

ENV PYTHONUNBUFFERED 1

RUN mkdir /code

WORKDIR /code

COPY . /code/

RUN pip install -r requirements.txt
RUN cd models/research/

# Build protoc
RUN protoc object_detection/protos/*.proto --python_out=.

# Build and install
RUN python setup.py build && python setup.py install

# Test build and install
RUN python object_detection/builders/model_builder_test.py

# EXPOSE 8000