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
The images and masks are converted into RGB formats and uniform image size of 256x256 or 512x512. The masks are converted in 2D labels. The mask is evaluated to identify unidentified patterns in the images. There are some pattern with negative value annotation which attributes to the missing values and data consistencies. Based on the annotation, two dimensional labels are generated. The image is normalized with mean and standard deviation for pre-trained models. The histogram equalization of images is performned for further analysis of image bands.

For over fitting, data augmentation techniques such as random horizontal flip, random vertical flip, color jitter are applied. The image and masks are converted into tensor for easy processing. The data is split into training and testing using train_test_split. For efficient data processing pytorch data loader is used for sequential processing of image and mask which reads each image/mask and transform it before model training or testing.

### Modeling
#### Unet
The deep learning architecture of convolution neural networks(CNNs).  This is a specialized architecture of encoders and decoders with skip connections which retains the spatial information needed for localization of small patterns.  These networks are used for learning complex patterns from raw image data such as image pixels. 

##### U-Shape Architecture
The encoder down samples the image to capture context while symmetrical decoder up samples it by creating segmentation map to compare with ground truth. 

##### Skip Connections
These are direct links of feature maps from encoder to the decoder. This helps preserved lost fine grained spatial features such as boundaries during down sampling of encoding phase. 

##### Fully Convolution
This model uses convolution layers allowing it to process images of various sizes and parameter reductions. 

#### DeepLabV3
It utilizes atrous convolution and atrous spatial pyramid pooling to capture multi scale  context by expanding the receptive pixel field without reducing image resolution. It incorporates conditional random fields for boundary refinement for multi label image classification. Atrous spatial pyramid pooling involves complex computation of multiple convolutions. It uses mobilenetV3 as a backbone. 

#### Random Forest Classifier
It is a supervised classification technique as meta estimator which uses number of decision tree classifiers on various sub samples of the dataset,  where individual pixels are classified into distinct categories based on the extracted features.  It is one of the best estimator for identifying features in image by segmenting complex objects. The technique is robust against over fitting and handles high dimensional data well.

#### Kmeans
It is a unsupervised learning algorithm. It groups data into predetermined number of clusters where data is grouped together based on euclidian distance. In case of image segmentation, based on euclidian distance the pixels are grouped together into multiple clusters. Kmeans++ is the default method of initializing centroids of the clusters. 

### Model Evaluation And Metrics
#### CNN based models
##### Activation
Activation used as softmax for multi class segmentation. It is a part of metrics calculation.
##### Metrics
##### Dice Loss
It measures overlap between predicted labels and ground truth. It is measured as
(1-Dice Coefficients). It addresses class imbalance by focusing on the overlap area. It is used in
satellite segmentation where there can be overlap of multiple landscapes or segmentation mask. The
satellite images consist of multiple small target objects and can have imbalance landscape
segmentation.
##### Accuracy/Precision/Recall/F1Scores: 
Default pytorch/tensorflow metrics is used to measure accuracy, precision, recall and F1 scores.
#### Random Forest Classifier
Overall performance of this classifier is best for all image sizes with less mean square error with
respect to predicted mask and ground truth.
#### Kmeans
Kmeans classifier uses predefined mask clusters for predicting mask. It can identify overlaps for
images with complex small object overlaps.

### Models and Performance
Following models are evaluated based on the its performance. 
1. Random Forest Classifier
2. KMeans
3. Unet with resnet50 as encoder.
4. Deeplabv3 with resnet101 as encoder
5. Custom Unet Model. 

##### Random Forest Classifier
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>f1</th>
      <th>accuracy</th>
      <th>mse</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>training</th>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>5.872325</td>
    </tr>
    <tr>
      <th>validation</th>
      <td>0.991748</td>
      <td>0.993413</td>
      <td>3.233793</td>
    </tr>
  </tbody>
</table>

##### KMeans
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>f1</th>
      <th>accuracy</th>
      <th>mse</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>training</th>
      <td>0.740254</td>
      <td>0.587622</td>
      <td>12.723673</td>
    </tr>
    <tr>
      <th>validation</th>
      <td>0.738480</td>
      <td>0.585389</td>
      <td>12.713935</td>
    </tr>
  </tbody>
</table>

##### CNN Pretrained Models

##### Unet with Resnet50 encoder.
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>accuracy</th>
      <th>dice-loss</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>training</th>
      <td>0.6314</td>
      <td>0.5619</td>
    </tr>
    <tr>
      <th>validation</th>
      <td>0.603</td>
      <td> 0.5904</td>
    </tr>
  </tbody>
</table>


##### Deeplabv3 with Resnet101 encoder. 
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>accuracy</th>
      <th>dice-loss</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>training</th>
      <td>0.6514</td>
      <td>0.5631</td>
    </tr>
    <tr>
      <th>validation</th>
      <td>0.6079</td>
      <td>0.6185</td>
    </tr>
  </tbody>
</table>

##### Custom Unet Model. 
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>accuracy</th>
      <th>categorical crossentropy loss</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>training</th>
      <td>0.7142</td>
      <td>0.7945</td>
    </tr>
    <tr>
      <th>validation</th>
      <td>0.6238</td>
      <td>1.1238</td>
    </tr>
  </tbody>
</table>

### Code Organization

```data/```
The data used for model traning and validation.

```metrics/```
Model performance metrics.

```models/```
Trained and validated models. 

```src/```
Source code for multiple model generation, training and validation. 

```README.md```
Details about the project aligned with template. 

### Usage
```
$ git pull <project-dir>
For models
$ cd <models>
$ git lfs pull

```










