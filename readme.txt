################# Training pipeline ###################

1-> Dataset annotation
	to annotate your data, you will use coco-annotator tool. It generates a json file that contains the annotations in coco format,
	which is necessary to train the model.
	To run coco-annotator, you need to have docker and docker-compose installed.
	1.In the terminal: go to the directory of coco-annotator and launch the docker build files (type the following command: docker-compose up)
	2.The instance can now be run through localhost at http://localhost:5000/
	3. Start by adding dataset images and creating the key-points classes.
	4. Finally, after annotating your data, you can export the annotations in coco format ans create the json file.


2-> Training the model
  1.Choose/Create model
	trt_pose already contains several models with different backbone CNN such as ResNet and DensNet with their different variants. The head branch predicts
	confidence maps and part affinity fields. Under trt_pose/trt_pose/models/ you will find all defined models. Each model takes at least two parameters,
        cmap_channels and paf_channels corresponding to the number of heatmap channels and part affinity field channels. The number of part affinity field
        channels is 2x the number of links, because each link has a channel corresponding to the x and y direction of the vector field for each link. By default, 
	these values are set corresponding to human pose task. If you wish to create a new task for different keypoint detection, you have to correspondly change
	these parameters.
  2. Train a model
	To train the model, you need to create a json file, which contains all training parameters and dataset/annotation directory paths.
	An example for this json file is also included(trainingConfig.json). Subsequently, this json file should be passed as parameter to the training script,
 	which can be found in trt_pose/train.py to start training.

	-Note: if you wish to fine-tune a pre-trained model, you must include a path to the pre-trained weights file("initial_state_dict" in json file).
	-Note: if you wish to fine-tune the included pre-trained human pose estimation models, you first need to preprocess your annotations to add an additional cmap.
		This adds an additional "neck" keypoint. To od this. simply run python preprocess_coco_person.py ANNOTATIONS_PATH MODIFIED_ANNOTATIONS_PATH
		