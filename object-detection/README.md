# Object Detection
## Install tensorflow
* CentOS 7
* Python 3
### Python3
* Run commands
```
# yum install centos-release-scl
```
* Install Python 3.6
```
# yum install rh-python36
```
* Verify version
```
# scl enable rh-python36 bash
# python --version
```
* Verify version pip3
```
$ pip --version
```

### Build & Traning Model
* Run commands
```
$ pip install protobuf
$ pip install pillow
$ pip install lxml
$ pip install Cython
$ pip install jupyter
$ pip install matplotlib
$ pip install pandas
$ pip install opencv-python 
$ pip install tensorflow
$ pip install tensorflow-gpu

- If error Illegal instruction(core dumped) tensorflow
$ pip install tensorflow==1.15.0
$ pip install tensorflow-gpu==1.15.0
```

* Build protoc
```
# protoc object_detection/protos/*.proto --python_out=.
or
# cd models/research
# mkdir protoc_3.0
# cd protoc_3.0
# wget -O protobuf.zip https://github.com/google/protobuf/releases/download/v3.0.0/protoc-3.0.0-linux-x86_64.zip
# unzip protobuf.zip
# cd ..
# ./protoc_3.0/bin/protoc object_detection/protos/*.proto --python_out=.
```
* Add Libraries to PYTHONPATH
```
# nano ~/.bashrc
- Add path
export PYTHONPATH=$PYTHONPATH:/home/tts/project/tensorflow/object-detection/models/research:/home/tts/project/tensorflow/object-detection/models/research/slim
# source ~/.bashrc
```
* Check PYTHONPATH
```
$ echo $PYTHONPATH
```
* Comleteing setup
```
# python setup.py build
# python setup.py install
```
* Training
```
# cd model/research/object_detection
# python generate_tfrecord.py --csv_input=images/train_labels.csv --image_dir=images/train --output_path=images/train.record
# python generate_tfrecord.py --csv_input=images/test_labels.csv --image_dir=images/test --output_path=images/test.record
# python train.py --train_dir=training/ --pipeline_config_path=training/ssd_mobilenet_v2_quantized_300x300_coco.config
```
* Note
```
- Edit batch_size: 6
- Eidt num_steps: 200
```
* Export tensorflow lite
```
# python3 export_tflite_ssd_graph.py --pipeline_config_path=/tensorflow/object-detection/models/research/object_detection/training/ssd_mobilenet_v2_quantized_300x300_coco.config --trained_checkpoint_prefix=/tensorflow/object-detection/models/research/object_detection/training/model.ckpt-200 --output_directory=/tensorflow/object-detection/tflite-model --add_postprocessing_op=true
```

## Refs
* https://towardsdatascience.com/tensorflow-object-detection-with-docker-from-scratch-5e015b639b0b
* https://www.geeksforgeeks.org/ml-training-image-classifier-using-tensorflow-object-detection-api/
* https://medium.com/@teyou21/setup-tensorflow-for-object-detection-on-ubuntu-16-04-e2485b52e32a
* https://medium.com/@teyou21/training-your-object-detection-model-on-tensorflow-part-2-e9e12714bdf?