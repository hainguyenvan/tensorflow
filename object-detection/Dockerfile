FROM ubuntu:bionic

MAINTAINER hainv.work.it@gmail.com

RUN apt-get update
RUN mkdir -p /tensorflow/models

COPY . /tensorflow/

WORKDIR /tensorflow/models/research

RUN apt-get install -y python3-pip

# Install lib
RUN pip3 install jupyter && pip3 install matplotlib && pip3 install protobuf && pip3 install pillow && pip3 install lxml && pip3 install Cython && pip3 install matplotlib && pip3 install pandas && pip3 install opencv-python && pip3 install tensorflow

RUN ./protoc_3.0/bin/protoc object_detection/protos/*.proto --python_out=.

RUN jupyter notebook --generate-config --allow-root

RUN echo "c.NotebookApp.password = u'sha1:6a3f528eec40:6e896b6e4828f525a6e20e5411cd1c8075d68619'" >> /root/.jupyter/jupyter_notebook_config.py
EXPOSE 8888

CMD ["jupyter", "notebook", "--allow-root", "--notebook-dir=/tensorflow/models/research/object_detection", "--ip=0.0.0.0", "--port=8888", "--no-browser"]

# Training
# Build and install
RUN python3 setup.py build && python3 setup.py install

# Create tfrecords
RUN python3 object_detection/generate_tfrecord.py --csv_input=object_detection/images/train_labels.csv --image_dir=object_detection/images/train --output_path=object_detection/images/train.record
RUN python3 object_detection/generate_tfrecord.py --csv_input=object_detection/images/test_labels.csv --image_dir=object_detection/images/test --output_path=object_detection/images/test.record

# Training
# RUN python3 object_detection/train.py --train_dir=object_detection/training/ --pipeline_config_path=object_detection/training/ssd_mobilenet_v2_quantized_300x300_coco.config