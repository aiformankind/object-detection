# Object-Detection

In this tutorial, we will show you how to build an object detector for a custom dataset. You can find the custom dataset in the input/images folder. The goal is to detect the square, circle, and triangle shapes in the images.

<img src=docs/object-detect.png />

We adapted the Dockerfile provided by Tensorflow and prepared a docker container preloaded with all the necessary libraries to get you started quickly to build your own object detection using custom dataset. We modified the scripts provided by Tensorflow and from other excellent online tutorials eg. https://github.com/bourdakos1/Custom-Object-Detection to give an easy to follow step by step tutorial.

Install Docker:

https://docs.docker.com/v17.12/docker-for-mac/install/#install-and-run-docker-for-mac

Clone the repository (https://github.com/aiformankind/object-detection):
```
git clone https://github.com/aiformankind/object-detection.git
```

Go to the repository directory that you just clone:
```
cd object-detection
```

Build the Tensorflow docker (this job will pull the latest tensorflow images and set up the environment) :
```
docker build -t aiformankind/objectdetection:0.0.1 .
```

Start the Tensorflow container (this job will spin up the objectdetection container):
```
docker run -it -p 8888:8888 -p 6006:6006 --name=objectdetection aiformankind/objectdetection:0.0.1
```
Train model:
```
python models/research/object_detection/legacy/train.py --logtostderr --train_dir=/tensorflow/train/ --pipeline_config_path=/tensorflow/train/faster_rcnn_resnet101.config
```

Create a frozen inference graph:
Replace model.ckpt-XXX with the largest(latest) checkpoint file number. Look it up in the /tensorflow/train folder

```
python models/research/object_detection/export_inference_graph.py --input_type image_tensor --pipeline_config_path /tensorflow/train/faster_rcnn_resnet101.config --trained_checkpoint_prefix /tensorflow/train/model.ckpt-XXX --output_directory /tensorflow/inference_graph
```

Start jupyter notebook
```
jupyter notebook --ip 0.0.0.0 --no-browser --allow-root
```

You can access the jupyter notebook on browser. Find the object_detection_shapes.ipynb in /tensorflow/models/research/object-detection folder.

Run the notebook to see the our model in action. In the last cell, we use the model to detect and identify the square, circle, triangle in the test images.
