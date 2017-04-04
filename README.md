![Darknet Logo](http://pjreddie.com/media/files/darknet-black-small.png)

#Darknet#
Darknet is an open source neural network framework written in C and CUDA. It is fast, easy to install, and supports CPU and GPU computation.

For more information see the [Darknet project website](http://pjreddie.com/darknet).

For questions or issues please use the [Google Group](https://groups.google.com/forum/#!forum/darknet).

---
This Repo is used training one specific class detection project.

### Training Steps:
1. Prepare GPU machine(highly recommend): Using AWS g2/2x.large with some community AMI with Cuda, CuDNN installed, or you need to have them installed by yourself.

2. Change the Makefile, set GPU=1 and CUDNN=1.

3. Prepare your dataset, manualy label the data by BBox Label Tools, and convert to the format darknet accept. x, y, width, height are all relative values.
`<object-class> <x> <y> <width> <height>`

4. Create labels names, like `data/voc.names`, list all your classes names.

5. Create data config, like `cfg/voc.data`

	```
	classes= {NUM_CLASSES}
	train  = {DATA_PATH}/train.txt
	valid  = {DATA_PATH}/valid.txt
	names = data/xxx.names
	backup = {DATA_PATH}/backup/
	```
	train.txt and valid.txt are just 2 lists of the image paths, darknet will automatically find labels in the image folder or it's parents folder. Suggest to have 2 folders `/images` and `/labels`.

6. Create your .cfg file, you can modify the existing ones, or create some brand new one. If you choose to modify existing ones, there are serval values you need to change:
	
	`classes={NUM_CLASSES}`
	`filters=num*{NUM_CLASSES+coords+1}` for the last convolutional layers.
	
7. (Optional) Download some pre-trained darknet weights.

8. Run the training by
	`./darknet detector train cfg/xxx.data cfg/xxx.cfg xxx.weights
`

9. Run the test, first you need to change the line 404 in `src/darknet.c` the data file to your training one, and then `make` and run detector with your test images.

10. If you want to use the model on android devices, please see [darkflow](https://github.com/thtrieu/darkflow) for how to generate darknet protobuf.