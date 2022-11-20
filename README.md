<!-- PROJECT LOGO -->
<br />
<div align="left">
<img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/logo_black.svg#gh-light-mode-only">
<img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/logo_white.svg#gh-dark-mode-only">
</div>
  <h1 align="left">Libs LidarAndTrans</h1>

## This library is used in three main ways: 
1. For reading lidar file and manipulating with data obtainded from this file
2. For transformation between devices(lidar,camera,imu) and also between  what the device is connected to(drone, car, pedestrian).
3. It also includes methods for colorization of points by camera frames.<br />
### More in specific sections below. <br /><br />
This library Consists of:

<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>baseframe</summary>
<p>

## baseframe is class where we can store lidar frame with points.
We can get some frame/s from lidar file by some file reader of this library(fileReader, hesaifilereader, OptechFileReader, optechinternalfilereader) and manipulate with this frame(points), colorize them and so on.

  
### Getting Started
1. To start, simply create object of this class and then you can use corresponding methods.
2. You can also create and init object of this class by using conscructor:
```js
new BaseFrame();
```
2. To add point to frame use method **addPoint** on object:
  
  
    - `pointtoadd` - point( Structure that holds point info, which we can get from lidar file- it is defined in common.h file ) that should be added
    - `r ,g, b` - defines a RGB color of point

```js
void BaseFrame::addPoint(basepointinfo &pointtoadd,int r,int g, int b)
```
3. You can also add point by another method **addPoint** on object:
    - `pointtoadd` - point( Structure that holds point info, which we can get from lidar file- it is in common.h file ) that should be added
    - `recalcRGB` - whether point should be colored according to its intensity

```js
void BaseFrame::addPoint(basepointinfo &pointtoadd, bool recalcRGB)
```
  
---  
  
</p>
</details>


<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>baselidarreader</summary>
<p>

### baselidarreader is a template for all readers which inherit from this class

 All reader inherited from this class should contain methods:
  
1. Open prepared file:
  
    - `pcapfile` -  name of lidar file

&emsp;&emsp;If given file exists **returns true**, else **returns false**.
```js
int openPreparedFile(std::string pcapfile)
``` 
2. Basic Init of prepared file:
  
    - `pcapfile` -  name of lidar file

&emsp;&emsp;If given file was init **returns number of frames**.
```js
int initFile(std::string pcapfile)
``` 
3. Inits frames data info structure based on the info from lidar file and given transformation:
  
    - `outputData` - holds all frames data info structures, its main output of this method
    - `pcapfile` - lidar file
    - `transformation` - transformation assigned to frames
    - `timeoffset` - timestamp offset
    - `stopcalculating` - disable/enable calculation

&emsp;&emsp;If given file was init **returns size of output data**.
```js
int initFileWithTransformations(std::vector<FrameData> &outputData, std::string pcapfile, std::vector<Transformation> &transformation, int timeoffset, bool *stopcalculating = nullptr)
``` 
4. To set corrections for lidar data:
  
    - `corrections` -  new corrections

```js
void setLaserMeasurementCorrections(std::vector<compensationValues> corrections)
``` 
  
5. To set info about frames- position("index") in lidar file and timestamp of each frame use method:
  
    - `newFrames` -  new info to be assigned

```js
void setFramesIDs(std::vector<FrameFileInfo> newFrames)
``` 
  
  
6. To get some lidar frame use method **getLasFrame** on object:
  
    - `localfile` - lidar file in which the frame will be searched
    - `index` - index of frame, which should be returned
    - `lidToFrame` - lidar transformation
    - `restriction` - restriction to add some points to frame
    - `openedFileID` - openedFileID
    - `colormodel` - colormodel
    - `minIntensityColor` - minimum intensity color
    - `maxIntensityColor` - maximum intensity color

```js
BaseFrame getLasFrame(std::ifstream &localfile,int index,CLidarToFrameTrans *lidToFrame,laserFrameRestrictionBase *restriction,int &openedFileID,int colormodel,double minIntensityColor,double maxIntensityColor);
```

7. To get actual reading position in lidar file use method:
  
```js
uint64_t getactualfilepos();
``` 

8. To get maximum position in lidar file use method:
  
```js
 uint64_t getmaxfilepos();
``` 

9. To get model of lidar use method:
  
```js
int getLaserModelType()
``` 


10. To get id of lines for a preset option call **getUnusedLidarLinesForPresetOption**:
  
    - `pcapfile` -  file

&emsp;&emsp;This output is used for filtering when only some of the laser data are wanted
```js
std::vector<int> getUnusedLidarLinesForPresetOption(LidarLinesPresets whichLines);

---

</p>
</details>

<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>clidartoframetrans</summary>
<p>

## This class is used for transformation of lidar to IMU based on transformation structures.
  
### Getting Started
1. To generate transformation matrix of lidar data to imu, call constructor of this class:
    - `laserToBodyCalib` - calibration structure of transformation from lidar to imu
    - `laserToBodyTransf` - transformation structure of lidar to imu
    - `lidoffset` - offset of lidar

```js
CLidarToFrameTrans(const Transformation &laserToBodyCalib,const Transformation &laserToBodyTransf,double lidoffset);

```
2. To transform some point from lidar to imu call **rotatePointToFrame** on object of this class:
  
    - `point` - point that should be transformed
  
&emsp;&emsp;It returns **basepointinfo structure**( Structure that holds transformed point info-it is defined in common.h file )
```js
basepointinfo rotatePointToFrame(basepointinfo point)
```
 
3. To get lidar to IMU transformation matrix call:
  
```js
Eigen::Affine3f getLidarToImuRotation()
``` 

4. To get lidar offset call:
  
```js
double getLidarRotOffset()
``` 
 
</p>
</details>

<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->
  
<details><summary>fileReader</summary>
<p>

##  This class is used for reading and manipulating with Velodyne lidar data.
  Most of the methods are inherited from baselidarreader class and implemented here.
    
```diff
- see baselidarreader section
```
  You can call all this inherited method on object of this class.
 This is implemented for models VLP-16, Hi-Res and Ultra.
  
### Getting Started
1. Most of the methods are inherited from baselidarreader class.
  
2. To start use constructor of this class:

    - `pcapfile` - lidar file
  
```js
fileReader::fileReader(std::string pcapfile)
```
3. Inherited method **getLaserModelType** for this class returns:
  
    - `0` - when model is VLP16
    - `1` - when model is Ultra
    - `2` - when model is Hi-Res

```js
int getLaserModelType()
```
  
  <br>Another methods of this class:<br>
  
4. To return requested lidar frame in sphere, call:
  
    - `localfile` - lidar file in which the frame will be searched
    - `index` - index of frame, which should be returned
    - `lidToFrame` - lidar transformation to imu
    - `restriction` - restriction to add some points to frame( See laserFrameRestriction section )
    - `videodata` - video data( Relation with trajectory and so on )
    - `cap` - video capture
    - `openedFileID` - its used for colorization( It is not used yet )
    - `colormodel` - color model

```js
    BaseFrame getLasSphere(std::ifstream &localfile,int index,CLidarToFrameTrans *lidToFrame,laserFrameRestrictionBase *restriction,VideoInfo &videodata,cv::VideoCapture &cap,int &openedFileID,int colormodel );

```
 
5. To get completed sphere call:
  
    - `timestamp` - time stamp
    - `spheresize` - size of sphere
    - `lidToFrame` - lidar transformation to imu
    - `restriction` - restriction to add some points to frame( See laserFrameRestriction section )
    - `videodata` - video data( Relation with trajectory and so on )
    - `cap` - video capture
    - `openedFileID` - its used for colorization( It is not used yet )
    - `colormodel` - color model

```js
BaseFrame getLasCompleteSphere(int timestamp,int spheresize,CLidarToFrameTrans *lidToFrame,laserFrameRestrictionBase *restriction,VideoInfo &videodata,cv::VideoCapture &cap,int &openedFileID,int colormodel );

```
  
6. To colorize frame with video from 360 camera call( It is not done yet ):
  
    - `frame` - frame that should be colored
    - `videodata` - video data( Relation with trajectory and so on )
    - `cap` - video capture
    - `openedFileID` - ID of opened file

```js
    void colorizeFrameWith360video(BaseFrame &frame,VideoInfo &videodata, cv::VideoCapture &cap,int &openedFileID);
```

7. To get number of frames call:

```js
int getNumberOfFrames()
```

8. To get Ids of frames( Position of frames in lidar file ) call:
  
```js
std::vector<FrameFileInfo> getFramesIDs()
```


9. To check whether file is lidar file of this class:
  
    - `pcapfile` -  file

```js
bool fileReader::isFileThisLidar(std::string pcapfile)
```

10. To calculate and return timestamp offsets from file call:
  
```js
std::vector<long long> calculateTimestampOffset()
```

11. To get ID of transformation based on given timestamp use:
  
    - `pointTimestamp` -  timestamp of point
    - `transformation` -  vector of transformations, where is looking for specific transformation based on timestamp
    - `previousID` -  previous transformation ID

```js
int getTransformationIdFromTimestamp(long long pointTimestamp,const std::vector<Transformation> &transformation,int previousID)
```

</p>
</details>
  
<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>globaltramsformation</summary>
<p>

## This class is used for two reasons: 
- For transformation of frame( frame points ) or camera to global coordinates. Its returns especially frame object(baseframe) - see baseframe section or returns transformation matrixes<br>
- It is also used for colorization of points by camera frames.
Colorization is implemented for camera models Garmin, LabPano and Sony.
  
### First we are getting started with Transformations:
  
1. To start use constructor of this class:

    - `transformation` - lidar file
    - `bodyToVehicle` - transformation structure between IMU and what the imu is attached to(drone, car, pedestrian)
    - `boresighToVehicle` - transformation structure for boresight
    - `iecalibparams` - transformation structure for compensation of bodyToVehicle rotation
    - `restriction` - restriction to add some points into frame( See laserframerestriction section )
    - `offset` - timestamp offset

  
```js
globaltramsformation::globaltramsformation(std::vector<Transformation> *transformation,const Transformation &bodyToVehicle,const Transformation &boresighToVehicle,const Transformation &iecalibparams,laserFrameRestrictionBase *restriction,const int offset)
```
2. To get transformed frame( frame points ) in global coordinates use method:

    - `frame` - frame that will be transformed
    - `frameID` - ID of frame
    - `colormode` - color model

- At first, frame is transformed from IMU to what the IMU is attached to(drone, car, pedestrian).
* Then this transformed frame is transformed to global coordinates.
```js
    BaseFrame *transformFrame(BaseFrame &frame,int frameID=0,int colormode=0);
```

3. You can get transformed frame(frame points) in global coordinates also by using method:

    - `frame` - frame that will be transformed
    - `lidarToImuRot` - transformation matrix between lidar and IMU
    - `frameID` - ID of frame
    - `colormode` - color model

- At first, frame is transformed from lidar to IMU.
* Then this transformed frame is transformed from IMU to what the imu is attached to(drone, car, pedestrian).
+ Finally the frame is transformed into global coordinates

```js
    BaseFrame *transformFrame(BaseFrame &frame,Eigen::Affine3f lidarToImuRot, int frameID=0,int colormode=0);
```

4. To get transformed frame( frame points ) of given area in global coordinates use:
  
    - `frame` - frame that will be transformed
    - `frameID` - ID of frame
    - `colormode` -color model
    - `areaToCheck` - only frame points in this area will be transformed and returned
  
```js
    BaseFrame *transformFrameToArea(BaseFrame &frame,int frameID,int colormode,ExportAreaUTM areaToCheck);
```
 
5. To get transformed frame( frame points ) to what the IMU is attached to(drone, car, pedestrian) use method:
  
    - `frame` - frame that will be transformed

```js
    BaseFrame *transformFrameToVehicle(BaseFrame &frame);
```
  

6. To get ID of transformation structure by given timestamp call:

    - `pointTimestamp` - timestamp based on which is looking for ID

```js
    BaseFrame *transformFrameToVehicle(BaseFrame &frame);
```

7. To obtain transformation structure based on given id use:

    - `id` - id of transformation which should be returned

```js
  Transformation getTransformationStructFromID(double id)
```
  
8. To obtain transformation structure based on given timestamp use:

    - `pointTimestamp` - Based on this timestamp is looking for transformation structure

```js
    Transformation getTransformationStructFromTimestamp(long long pointTimestamp);
```


9. To obtain transformation matrix based on given timestamp call:

    - `pointTimestamp` - timestamp based on which is looking for matrix

```js
    Eigen::Affine3f getTransformationFromTimestamp(long long pointTimestamp);
```



10. If you want to interpolate some data ( By Cubic Hermite Interpolation ) you can use: 

    - `A` - 1.control point
    - `B` - 2.control point
    - `C` - 3.control point
    - `D` - 4.control point
    - `t` - is a value that goes from 0 to 1 to interpolate in a continuous way across uniformly sampled data points. When t is 0, method will return B.  When t is 1, method will return C.
  
&emsp;&emsp;[More about this interpolation you can read there](https://blog.demofox.org/2015/08/08/cubic-hermite-interpolation/)
```js
    double CubicHermiteL (const double &A, const double &B, const double &C, const double &D, const double &t);
```


### Camera Transformations

1. To set transformation matrix of camera to what the camera is attached to(drone, car, pedestrian) call:

    - `CameraToVehicle` -  transformation structure of camera to what the camera is attached to(drone, car, pedestrian).
    - `baseRot` -  rotation of camera which ensures that the axes of rotation about the Z axis coincide with the imu
    - `offset` -  camera time offset

```js
    void setCameraToVeh(const Transformation &CameraToVehicle,double baseRot,const double offset);
```

2. To get global transformation matrix of camera for given timestamp use:

    - `time` - transformation is searched  based on this timestamp

```diff
- First you have to set transformation matrix of camera to what the camera is attached to(method setCameraToVeh)
```
```js
    Eigen::Affine3f getCameraTransf(long long time);
```

3. To get inverse global transformation matrix of camera for given timestamp use:

    - `time` - transformation is searched  based on this timestamp

```diff
- First you have to set transformation matrix of camera to what the camera is attached to(method setCameraToVeh)
```
```js
    Eigen::Affine3f getInverseCameraTransf(long long time);
```

4. To get inverse global transformation matrix of camera based on given global transformation use:

    - `globaltr` - global transformation structure

```diff
- First you have to set transformation matrix of camera to what the camera is attached to(method setCameraToVeh)
```
```js
    Eigen::Affine3f getInverseCameraTransf(Transformation globaltr);
```

5. To get inverse global transformation matrix of camera you can use also :

    - `globaltr` - global transformation structure
    - `CameraToVehicle` - transformation structure of camera to what the camera is attached to(drone, car, pedestrian...)

```js
    Eigen::Affine3f getInverseCameraTransf(Transformation globaltr,Transformation CameraToVehicle);
```

### Colorization of points by camera frames:
 
1. To generate color image from video for defined frame use:

    - `retfr` - assigned color image for given frame( Main output of this method )
    - `frame` - the frame for which the image is generated
    - `videodata` - Data info for the given camera( Relational vector between video frames and trajectory and so on )
    - `cap` - video capture
    - `openedFileID` - ID of opened video file

```js
    void getColorimageForFrameByVirb(ColorizingInfo* retfr,BaseFrame &frame,CameraDataInfo &videodata, cv::VideoCapture &cap,int &openedFileID);
```

2. Second method to generate color image from video is:
  
    - `retfr` - assigned color image for given frame( Main output of this method )
    - `frameID` - frame ID( Used for indexing in videodata ), ID of frame for which the image is generated
    - `videodata` - Data info for the given camera( Relational vector between video frames and trajectory and so on )
    - `cap` - video capture
    - `openedFileID` - ID of opened video file

```js
    void getColorimageForFrameByVirb(ColorizingInfo* retfr,int imageID,CameraDataInfo &videodata, cv::VideoCapture &cap,int &openedFileID);
```

3. If you want to colorize frame by image from video use method **colorizeFrame**:
  
    - `frame` - frame that will be colorized
    - `imgfr` - image from video for given frame. Frame is colorized based on this image( You can get it by method getColorimageForFrameByVirb )
    - `videodata` - Data info for the given camera(Relational vector between video frames and trajectory and so on)
    - `cap` - video capture
    - `openedFileID` - ID of opened video file
    - `timeshift` - time shift
    - `imgFrameRestriction` - restriction to colorize zones of points( See imageframerestriction section )
    - `hsv_saturation` - saturation of colors would be changed based on this value
    - `hsv_brightness` - brightness of colors would be changed based on this value
    - `removeOtherColor` - removeOtherColor whether actual colors of points in frame restriction zones would be replaced by color based on intensity

&emsp;&emsp;This method returns info structure about colorizing( Indexes of points that were colorized and position of camera )

```js
    ColorizedDataInfo colorizeFrame(BaseFrame &frame,ColorizingInfo *imgfr,CameraDataInfo &videodata, cv::VideoCapture &cap,int &openedFileID,double timeshift,imageFrameRestriction *imgFrameRestriction,double hsv_saturation,double hsv_brightness,bool removeOtherColor=true);
```
4. If you want to colorize frame by all images from video use method **colorizeFrameByAllImages**:
  
    - `frame` - frame that will be colorized
    - `videodata` - Data info for the given camera(Relational vector between video frames and trajectory and so on)
    - `cap` - video capture
    - `openedFileID` - ID of opened video file
    - `timeshift` - time shift
    - `imgFrameRestriction` - restriction to colorize zones of points(see imageframerestriction section)
    - `hsv_saturation` - saturation of colors would be changed based on this value
    - `hsv_brightness` - brightness of colors would be changed based on this value
    - `removeOtherColor` - removeOtherColor whether actual colors of points in frame restriction zones would be replaced by color based on intensity

&emsp;&emsp;This method returns info structure about colorizing( Indexes of points that were colorized and position of camera )

```js
    ColorizedDataInfo colorizeFrame(BaseFrame &frame,ColorizingInfo *imgfr,CameraDataInfo &videodata, cv::VideoCapture &cap,int &openedFileID,double timeshift,imageFrameRestriction *imgFrameRestriction,double hsv_saturation,double hsv_brightness,bool removeOtherColor=true);
```


5. If you want to colorize cloud by images from video use method **colorizeCloudByImages**:
  
    - `octree` - octree of cloud that stores colors of points after assignment
    - `colors` - stores colors of points after assignment
    - `mutexes` - mutexes that protect shared data from being simultaneously accessed by multiple threads
    - `videodata` - Data info for the given camera(Relational vector between video frames and trajectory and so on)
    - `cameraIds` - ids of cameras from which are obtaining frame images
    - `cap` - video capture
    - `openedFileID` - ID of opened video file
    - `timeshift` - time shift
    - `imgFrameRestriction` - restriction to colorize zones of points
    - `hsv_saturation` - saturation of colors would be changed based on this value
    - `hsv_brightness` - brightness of colors would be changed based on this value
    - `progress` - progress of colorization


```js
    void colorizeCloudByImages(octree_data<pcl::PointXYZI> &octree,std::vector<std::vector<std::uint32_t>> &colors,std::vector<std::mutex> &mutexes,CameraDataInfo &videodata,std::vector<int> cameraIds, cv::VideoCapture &cap,int &openedFileID,double timeshift,imageFrameRestriction *imgFrameRestriction,double hsv_saturation,double hsv_brightness,std::atomic_int &progress);

```

6. If you want to draw marker circle to frame from camera, use:
  
    - `marker` - marker that should be painted to image
    - `frame` - marker will be painted to this frame
    - `newCam` - transformation structure of camera to what the camera is attached to(drone, car, pedestrian...)
    - `framedata` - info about frame. Based on this data is looking for camera transformation
    - `offset` - camera time offset

```js
    void colorFrameByDifferendVirb(MarkerLocations marker, cv::Mat &frame,Transformation newCam, FrameData &framedata,double &offset);
```
7. To draw marker you can use also method:
  
    - `marker` - marker that should be painted to image
    - `frame` - marker will be painted to this frame
    - `newCam` - transformation structure of camera to what the camera is attached to(drone, car, pedestrian...)
    - `framedata` - info about frame. Based on this data is looking for camera transformation
    - `offset` - camera time offset

```js
    void colorByTemporalMarker(MarkerLocations marker, cv::Mat &frame,Transformation newCam, FrameData &framedata,double &offset);
```

8. To change saturation and value of color use:
  
    - `r` - red part of rgb for enhancement, range <0,255>
    - `g` - green part of rgb for enhancement, range <0,255>
    - `b` - blue part of rgb for enhancement, range <0,255>
    - `saturationchange` - change in the saturation
    - `valuechange` - change in value - brightness

```js
    void enhanceColor(unsigned char &r, unsigned char &g,unsigned char &b,double saturationchange,double valuechange);
```
9. If you want to transform rgb color to hsv color call method:

    - `r` - red part of the rgb in range <0,1>
    - `g` - green part of the rgb in range <0,1>
    - `b` - blue part of the rgb in range <0,1>
    - `h` - hue part of the hsv in range <0,360>
    - `s` - saturation part of the hsv in range <0,1>
    - `v` - value part of the hsv in range <0,1>

```js
    void rgb2hsv(double r, double g, double b, double &h,double &s, double &v);
```

10. If you want to transform hsv color to rgb color call method:

    - `h` - hue part of the hsv in range <0,360>
    - `s` - saturation part of the hsv in range <0,1>
    - `v` - value part of the hsv in range <0,1>
    - `r` - red part of the rgb in range <0,1>
    - `g` - green part of the rgb in range <0,1>
    - `b` - blue part of the rgb in range <0,1>

```js
    void hsv2rgb( double h,double s, double v,double &r, double &g, double &b);
```

---  
</p>
</details>

<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>hesaifilereader</summary>
<p>

##  This class is used for reading and manipulating with Hesai lidar data.
  Most of the methods are inherited from baselidarreader class and implemented here.
    
```diff
- see baselidarreader section
```
  You can call all this inherited method on object of this class.
  This is implemented for models XT-32, XT-16 and XT32M2.
  
### Getting Started
1. Most of the methods are inherited from baselidarreader class.
  
2. To start use constructor of this class and create object:

    - `pcapfile` - lidar file
  
```js
HesaiFileReader::HesaiFileReader(std::string pcapfile)
```
3. Inherited method **getLaserModelType** for this class returns:
  
    - `0` - when model is XT32
    - `1` - when model is xt16
    - `2` - when model is xt32m2x

```js
int getLaserModelType()
```
  
  <br>Another methods of this class:<br>
  
  
4. To colorize frame with video from 360 camera call(It is not done yet):
  
    - `frame` - frame that should be colored
    - `videodata` - video data(relation with trajectory and so on)
    - `cap` - video capture
    - `openedFileID` - ID of opened file

```js
    void colorizeFrameWith360video(BaseFrame &frame,VideoInfo &videodata, cv::VideoCapture &cap,int &openedFileID);
```

5. To get number of frames call:

```js
int getNumberOfFrames()
```

6. To get Ids of frames(position of frames in lidar file) call:
  
```js
std::vector<FrameFileInfo> getFramesIDs()
```


7. To check whether file is lidar file of this class:
  
    - `pcapfile` -  file

```js
bool fileReader::isFileThisLidar(std::string pcapfile)
```

8. To get ID of transformation based on given timestamp use:
  
    - `pointTimestamp` -  timestamp of point
    - `transformation` -  vector of transformations, where is looking for specific transformation based on timestamp
    - `previousID` -  previous transformation ID

```js
int getTransformationIdFromTimestamp(long long pointTimestamp,const std::vector<Transformation> &transformation,int previousID)
```

---  
</p>
</details>


<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>imageframerestriciton</summary>
<p>

## imageframerestriciton is the class that manages filtering image parts for colorization.
Filters are stored in registry and each filter can have multiple zones.
Filters are defined as polygones where if a points falls into, it shouldnt be colored by this pixel.
```diff
- Please make sure, your application has correctly set OrganizationName and ApplicationName. Otherwise you wont find any filters.
```

The structure of registry entry is:<br> <br>HKEY_CURRENT_USER/SOFTWARE/ORGANIZATION_NAME/APPLICATION_NAME/image_restriction_zones/ID
<br><br> Each image_restriction_zones entry has a name and an array of regions, each region has an array of points defined as x,y pairs, each in range<0,1>

### Getting Started
1. To start use constructor of this class and create object:
  
```js
imageFrameRestriction::imageFrameRestriction()
```
2. To load some filter from registry by given name call:

     - `filtername` - name of filter to be loaded, if not existing or "" filter is created empty
```js
void loadFilter(std::string filtername)
```  

3. To load some filter from registry by given id call:

     - `filterid` - id of filter to be loaded, if not existing or less than 0, filter is created empty
```js
void loadFilter(int filterid)
```  

4. To check whether point is in filter zone and should be filtered call:
     - `x` - x coordinate of point
     - `y` - y coordinate of point
     
&emsp;&emsp;It **returns true** when point is in filter zone, else **return false**
```js
bool isPointInZone(double x, double y)
```  



---  
  
</p>
</details>
  
  
  <!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>laserframerestriciton</summary>
<p>

## laserframerestriciton is the class that defines restrictions to add point into frame.
Filters are stored in registry.

```diff
- Please make sure, your application has correctly set OrganizationName and ApplicationName. Otherwise you wont find any filters.
```

The structure of registry entry is:<br> <br>HKEY_CURRENT_USER/SOFTWARE/ORGANIZATION_NAME/APPLICATION_NAME/filters/ID

<br><br> Each filters entry has a name and an array of regions.

### Getting Started
1. To start use constructor of this class and create object:
  
```js
laserFrameRestrictionBase::laserFrameRestrictionBase()
```

or

     - `lidtrans` - lidar transformation to body data(see clidartoframetrans section)
```js
laserFrameRestrictionBase::laserFrameRestrictionBase(CLidarToFrameTrans *lidtrans)
```

or

     - `whichRestrictions` - id of filter that will be used
     - `lidtrans` - lidar transformation to body data(see clidartoframetrans section)


```js
laserFrameRestrictionBase::laserFrameRestrictionBase(int whichRestrictions, CLidarToFrameTrans *lidtrans)
```

2. To determine if point is in laser limits and inside the defined region use:
   
     - `point` - point(structure that holds point info, which we can get from lidar file- it is defined in common.h file) which is observed

&emsp;&emsp;It **returns true** when point is in ranges, else **return false** - it should be filtered

```js
bool pointInRange(basepointinfo point)
```





---  
  
</p>
</details>
  
  
  <!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>OptechFileReader</summary>
<p>

##  This class is used for reading and manipulating with Optech lidar data.
  Most of the methods are inherited from baselidarreader class and implemented here.
    
```diff
- see baselidarreader section
```
  You can call all this inherited method on object of this class.
  This is implemented for model CL-360
  
### Getting Started
1. Most of the methods are inherited from baselidarreader class.
  
2. To start use constructor of this class:
  
```js
OptechFileReader::OptechFileReader()
```
2. Inherited method **getLaserModelType** for this class returns:

    - `1` - when model is CL-360

```js
int getLaserModelType()
```
  
  <br>Another methods of this class:<br>
  

3. To get Ids of frames(position of frames in lidar file) call:
  
```js
std::vector<FrameFileInfo> getFramesIDs()
```


4. To check whether file is lidar file of this class:
  
    - `pcapfile` -  file

```js
bool fileReader::isFileThisLidar(std::string pcapfile)
```

5. To get ID of transformation based on given timestamp use:
  
    - `pointTimestamp` -  timestamp of point
    - `transformation` -  vector of transformations, where is looking for specific transformation based on timestamp
    - `previousID` -  previous transformation ID

```js
int getTransformationIdFromTimestamp(long long pointTimestamp,const std::vector<Transformation> &transformation,int previousID)
```

---  
  
</p>
</details>
  
  
  <!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>optechinternalfilereader</summary>
<p>

##  This clss is used for reading and manipulating with Optech lidar data, specifically stored on internal disk. 
 These data are in
 specific format. For manipulating with these data in this class, it is
 necessary to change the internal data format to our format. This is done using
 app InternalOptechToCreatorOptechModificator.
 This class inherited methods from baselidarreader class.
    
```diff
- see baselidarreader section
```
  You can call all this inherited method on object of this class.
  This is implemented for model CL-360
  
### Getting Started
1. Most of the methods are inherited from baselidarreader class.
  
2. To start use constructor of this class:
  
```js
OptechInternalFileReader::OptechInternalFileReader()
```
2. Inherited method **getLaserModelType** for this class returns:

    - `1` - when model is CL-360

```js
int getLaserModelType()
```
  
  <br>Another methods of this class:<br>
  

3. To get Ids of frames(position of frames in lidar file) call:
  
```js
std::vector<FrameFileInfo> getFramesIDs()
```


4. To check whether file is lidar file of this class:
  
    - `pcapfile` -  file

```js
bool fileReader::isFileThisLidar(std::string pcapfile)
```

5. To get ID of transformation based on given timestamp use:
  
    - `pointTimestamp` -  timestamp of point
    - `transformation` -  vector of transformations, where is looking for specific transformation based on timestamp
    - `previousID` -  previous transformation ID

```js
int getTransformationIdFromTimestamp(long long pointTimestamp,const std::vector<Transformation> &transformation,int previousID)
```

  
---  
  
</p>
</details>
 
<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->


#### This library is used in:
##### 1. Creator app. 
-   qcloudaerialview, qcloudcutwindow, qsidewayview will be shown, when user selects **Profiles** from the side menu.

