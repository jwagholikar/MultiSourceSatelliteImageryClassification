## MultiSource Satellite Imagery Segmentation Classification. 

### Problem Statement
Satellite image or data classification is intended for land usage and land cover classification task. The model can be used to analyze landscape patterns and land use changes over time. The broad classification involves various categories such as residential, industrial, river, forest, sea, agriculture land classification.

### Goals
The goal is to classify images with semantic segmentation mask which provides pixel level classification of different classes. This gives opportunity for multi class classification of a single image which contains multiple labels such as industrial, residential, forest etc. It provides pixel level understanding that enables models to interpret image by creating detailed maps used in satellite analysis to identify objects, landscapes with their precise locations and boundaries. 

### Overview
#### Business and Real World Context 
Land satellite data classification has numerous business use cases in the sectors of agriculture, forestry, utilities, in resource management and monitoring. It can be used to estimate crop yields, water usage or monitor forest health or growth. They are also useful for asset monitoring and in energy or security sectors.

### Data  
The [data](https://www.kaggle.com/datasets/hammadjavaid/multi-source-satellite-imagery-for-segmentation) used here is multi source satellite imagery for segmentation. This dataset contains high resolution satellite images which are annotated to identify six classes. There are 
total 203 images which span diverse urban and natural environments for developing or testing segmentation models. It has standardized masks and labels for multi class classification. With data pre
processing this can be used to train and test segmentation models.

#### Data Preprocessing [Link](https://github.com/jwagholikar/MultiSourceSatelliteImageryClassification/blob/main/src/MultiSourceSatelliteImagery_data_analysis.ipynb)
The images and masks are converted into RGB formats and uniform image size of 256x256 or 512x512. The masks are converted in 2D labels. The mask is evaluated to identify unidentified patterns in the images. There are some pattern with negative value annotation which attributes to the missing values and data consistencies. Based on the annotation, two dimensional labels are generated. The image is normalized with mean and standard deviation for pre-trained models. The histogram equalization of images is performned for further analysis of image bands. The image is normalized with mean and standard deviation for pre-trained models.

For over fitting, data augmentation techniques such as random horizontal flip, random vertical flip, color jitter are applied. The image and masks are converted into tensor for easy processing. The data is split into training and testing using train_test_split. For efficient data processing pytorch data loader is used for sequential processing of image and mask which reads each image/mask and transform it before model training or testing.




