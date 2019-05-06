#Data Science Intern Questions



##1a

### Aerial imagery
>Aerial photography can create stereoscopic images through the overlap between images, so the damage of the building can be detected more intuitively and easily. Although aerial photography may be affected by weather (eg, cloudy, evening, or cloudy). But through many new sensor technologies (eg, multispectral, hyperspectral/thermal) can effectively ignore weather factors or environmental anomalies.

### Satellite imagery
>Satellite imagery can play a large role in a wide range of disaster identification, but the accuracy and spatial resolution of small areas cannot be accurately assessed for disasters. According to the type of satellites, the frequency of access to the same location and the range of the image area is also different. However, most satellite images cannot present stereoscopic images, and the types of devices in the sensors are also fixed, so the information obtained is relatively fixed.

- For satellite imagery and aerial imagery, I analyze and compare several main parameters.


|    | Satellite imagery      | Aerial imagery |
|   -----------  | ----------- | ----------- |
|Resolution|Highest resolution about 50cm |Ranges from 90cm up to 2.5cm|
| Weather  | Although satellite maps can access data through the cloud, data credibility still will be affected by the cloud and atmospheric conditions.|Although aerial imagery may be affected by the weather, it can adjust flight altitude and angle based on local weather conditions|
|Location accuracy| Few meters | accuracy of two pixels   |
|Sensors type | Satellite imagery has a variety of data sets and convenient associated auxiliary analysis software in standard sensors. But if you want to use a non-standard sensor type (eg, thermal, LiDAR), it will be difficult and expensive. |   Aerial imagery can be easily matched with a variety of new sensing technologies, and even multi-angle shooting can be used to model stereo images.|
|Location accessibility| Can acquire the images from all parts of the earth | Some areas may not be easily accessible due to terrain or regulations       |
| Speed    | Can get a wide range of information in a short time. But it can be difficult to capture time-critical events due to the way satellites orbit the earth  | Although it can capture time-critical events. However, because the range is small, it need takes a lot of images for the disaster of the big range, and the image processing time will be longer. |
|Coverage  |  Large      | Small       |
| Cost     |Costs are usually more straightforward to calculate, but using  non-standard sensing can be expensive   | Cost depends on many variables such as mobilization cost, resolution, accuracy.        |

|    | Satellite imagery      | Aerial imagery |
|   -----------  | ----------- | ----------- |
| Pros | 1. Coverage Large <br> 2. Collecting amounts of data in short time <br>3. Costs is straightforward to calculate  | 1. High Resolution <br> 2. High accuracy <br> 3. Can produce stereoscopic images|
| Cons | 1. Low Resolution <br> 2. Hight cost in non-standard sensing| 1. Costs unstable <br> 2. Coverage small |

### Conclusion
> I will use satellite imagery to make a rough assessment of a wide range of disasters in a short period of time, then make detailed disaster estimates for small areas through aerial images, and judge the damage of the area through 3D building generation. Because urban damage assessment is more complex, aerial images can perform damage analysis on buildings, roads, and so on. The satellite images can be used to make terrain for the suburbs and to assess large-scale natural disasters. This also saves the cost and time of a wide range of aerial images.


##1b

**Optional 1 : Semantic Segmentation Model + Disaster Impact Index**

- Model flow chart:

![](/Users/you-jun/Desktop/Resume/Untitled Diagram.jpg)

- Model concept:

> Using satellite images to capture photos before and after the disaster, we can use Semantic Segmentation Model to extract features of buildings, roads, etc. Comparing features before and after the disaster, we can get the change of the city. After that, we can calculate the Damage Impact Index and use it to do normalization to detect the damage level in each place.


Disaster Impact Index (DII): 

>It is a metric used to quantify the impact of disasters, presented by Facebook and CrowdAI in 2018. 

>Calculate the number of pixels in the grid which have the
feature detected in the pre-disaster mask but not in the post-disaster mask and then divided by the whole region average of the number of feature pixels predicted in each grid pre-disaster 


||Description|
|   -----------  | ----------- | 
| Pros | Ability to classify disasters in various regions in a short time and on a large scale  |
| Cons | It is hard for the assessment of damage to buildings or small areas and there is a tendency to use the area of the disaster area as a measure of disaster| 

**Optional 2 : Aerial imagery(RGB channels) + CNN**

- Model flow chart:

![](/Users/you-jun/Desktop/Resume/CNN.png)

- Model concept:

>Train CNN model through post-disaster and pre-disaster data set from different regions and terrains.
After that, the aerial imagery(RGB channels) before and after the disaster are thrown into the trained CNN model, and each patch will be classified into 0, 1 (disaster, no disaster) in the Fully-Connected Layer by comparing the changes before and after the disaster. 

>Finally, by comparing the ground truth images that follow the raster scan to calculate the precision. 

||Description|
|   -----------  | ----------- | 
| Pros | Compare images before and after disasters with RGB channels color information can produce better results|
| Cons | Since the CNN model is greatly affected by the color factor, if the color change of the training set is similar that will affect the prediction result. Assuming that the training images data is all from daylight, the credibility of the prediction will reduce if the testing image is taken at night. |

**Optional 3 : LiDAR ＋ Aerial imagery**

- Model flow chart:

![](/Users/you-jun/Desktop/Resume/Lidar.png)

- Model concept:

>Through LiDAR, we can generate DSM information before and after the disaster, and compare the DSM information before and after the disaster to understand the change of building height. And then projected the DSM onto the aerial imagery, and the Aerial imagery is used to distinguish between the characteristics of the building and the non-building, and the non-building area is filtered out. Finally, the damaged rank of the building area is classified according to the DSM change information.

##1c Organizing the data for processing

>For the data I get, I will divide it into a training set and a test set.
Use the training set to train module and use the testing set to verify the result. In addition, the image of the training set can be rotated, distortion, shift or flip. Changing the nature or connotation of the image itself and increasing the image data after deformation can make the deep learning model learn more key features, making the model better.
>For the information obtained, I can do the data cleaning to remove the noisy data and the abnormal value and then do the normalization.



##2

```
import pandas as pd
import numpy as np

def find_most_damage(filename):
    """
    Calculate the average disaster rank for each region and 
    find the region with the highest average disaster rank.
    
    :param filename: filename which we want to analysis
    :type filename: string
    :rtype: location name, string
    """
    damage_classification_rank = pd.read_csv(filename, index_col=0 )
    damage_mean = damage_classification_rank.groupby(['location']).mean()
    max_location = damage_mean['damage_classification_rank'].idxmax()
    
    return max_location
```
![](/Users/you-jun/Desktop/Resume/螢幕快照 2019-05-04 下午9.44.55.png)

**We can calculate mean, std, min, max, count, 25%, 50%, 75%**

![](/Users/you-jun/Desktop/Resume/螢幕快照 2019-05-04 下午9.48.57.png)

> From the above data you can know
> 
1. The worst severity of the three cities is 5 but there is a minimum damage level of 1 in Rockport, so Rockport's std is also the largest. Indicates that the damage level in Rockport varies greatly, and hurricane must also pass through certain areas of Rockport.
2. Rockport's mean and count are the biggest, indicating that the overall Rockport disaster is more serious.

**The limitations in this data set:**

1. Because this data set only shows the damage rank in a wide range of areas, the same area has multiple levels of damage, which can lead to deviations in the analysis of disasters in a certain area. Therefore, if the data set can increase the information of the area of the damaged range, it will be able to more accurately analyze the disaster level of each small area.

2. Because there are many ways to calculate the disaster rank, we can't understand the assessment criteria of the disaster level behind the data set. For example, compare large areas of forest damage, and small areas of urban building damage. I think that although the urban damage area is small, it should still be a higher disaster level. Because of the destruction of urban buildings might be considered as high damage. 

3. There is also a large difference in the amount of data in each region, which may also lead to analysis of the deviation of the disaster situation.

##2a

![](/Users/you-jun/Desktop/Resume/螢幕快照 2019-05-04 下午10.22.55.png)

![](/Users/you-jun/Desktop/Resume/螢幕快照 2019-05-04 下午10.25.37.png)

![](/Users/you-jun/Desktop/Resume/螢幕快照 2019-05-05 上午10.55.00.png)

**Combine with google map API**

Red circle size depends on the average of damage rank
![Combine with google map](/Users/you-jun/Desktop/Resume/螢幕快照 2019-05-04 下午9.27.35.png)

##Extra Credit:
My github account name:GaryWu0512

I add the image of double-eyed hurricanes.

##Extra Extra Credit:

1. Support Vector Machines(SVM): SVM can be used to classify buildings structure and non-buildings structure in DSM or to classify the building into debris class or intact class.

2. Alexnet CNN model: Alexnet convolutional neural network can be used with the aerial image (RGB) to obtain the characteristic of disaster.

3. ResNet CNN model: ResNet can be used in satellite imagery to do binary segmentation to cut out roads and buildings in the image.
















 
 