<!-- PROJECT LOGO -->
<br />
<div align="left">
<img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/logo_black.svg#gh-light-mode-only">
<img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/logo_white.svg#gh-dark-mode-only">
</div>
  <h1 align="left">Libs LidarAndTrans</h1>

## This library is used in two main ways: 
1. For transformation between devices(lidar,camera,imu) and also between  what the device is connected to(drone, car, pedestrian).
2. For reading lidar file and manipulating with data obtainded from this file.<br />
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
  
  
    - `pointtoadd` - point(structure that holds point info, which we can get from lidar file- it is defined in common.h file) that should be added
    - `r ,g, b` - defines a RGB color of point

```js
void BaseFrame::addPoint(basepointinfo &pointtoadd,int r,int g, int b)
```
3. You can also add point by another method **addPoint** on object:
    - `pointtoadd` - point(structure that holds point info, which we can get from lidar file- it is in common.h file) that should be added
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



10. To get id of lines for a preset option. This output is used for filtering when only some of the laser data are wanted:
  
```js
std::vector<int> getUnusedLidarLinesForPresetOption(LidarLinesPresets whichLines)
``` 

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
  
&emsp;&emsp;It returns basepointinfo structure(structure that holds transformed point info-it is defined in common.h file)
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

##  This library is used for reading and manipulating with Velodyne lidar data.
  Most of the methods are inherited from baselidarreader class and implemented here.
  
  You can call all this inherited method on object of this class. 
  
  [click on this link](#baselidarreader-is-a-template-for-all-readers-which-inherit-from-this-class)
  This is implemented for models VLP-16, Hi-Res and Ultra.
  
### Getting Started
1. Most of the methods are inherited from baselidarreader class

2. To show this view with painted cloud points, call **addAndShowCut** on this frame:
  
    - `inputcloud` - the entire cloud that generated the backend for display
    - `llp1` - right centered point of cut(on right side of trajectory)
    - `llc1` - centered point of cut, defined by user
    - `llp2` - left centered point of cut(on left side of trajectory)
    - `cutwidth` - distance from cut
    - `newusedZones` - zones which are used

```js
void addAndShowCut(cloudViz inputcloud,pcl::PointXYZRGB lp1,pcl::PointXYZRGB lc1,pcl::PointXYZRGB lp2,double cutwidth,std::map<int, bool> newusedZones);
```
  
  
```render-babel
class Demo extends React.Component {
  componentDidMount() {
    setInterval(() => this.forceUpdate(), 500)
  }
  render() {
    return <h1>This is cool times {Date.now()}</h1>
  }
}

ReactDOM.render(
  <Demo/>,
  DOM_NODE
)
```

</p>
</details>
  
<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>globaltramsformation</summary>
<p>

## Is frame class which projects cloud points into one coordination plane. Specifically to plane ZX(side way).</br>
This class also takes care of the interaction during measurement(in this frame).
  
### Getting Started
1. When you want to use this view somewhere, first of all you have to add frame promoted to class **qcloudcutwindow** to .ui file.

2. To show this view with painted cloud points, call **addAndShowCut** on this frame:
  
    - `inputcloud` - the entire cloud that generated the backend for display
    - `llp1` - right centered point of cut(on right side of trajectory)
    - `llc1` - centered point of cut, defined by user
    - `llp2` - left centered point of cut(on left side of trajectory)
    - `cutwidth` - distance from cut
    - `newusedZones` - zones which are used

```js
void addAndShowCut(cloudViz inputcloud,pcl::PointXYZRGB lp1,pcl::PointXYZRGB lc1,pcl::PointXYZRGB lp2,double cutwidth,std::map<int, bool> newusedZones);
```

</p>
</details>

<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>hesaifilereader</summary>
<p>

## qpolygonrubberband is class for painting polygon.

### Getting Started
1. To set polygon and draw it, call **setPolygon** on object of this class:
```js
void setPolygon(std::vector<QPoint> polygonPoints)
```
&emsp;&emsp;Or :
```js
void setPolygon(std::vector<QPoint> polygonPoints,QPoint lastPoint)
```
  
</p>
</details>


<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>imageframerestriciton</summary>
<p>

## undoselectionstack is the class which holds history of selections, so you can go through this history.

### Getting Started
1. If you want to use this somewhere, first of all you have to call **createNewProject** on object of this class:
     - `projj` - reference for changing trajectory states 
```js
 void createNewProject(std::shared_ptr<std::vector<framesTrajectoryRelationsInfoStruct>> projj)
```  
  
---  
  
</p>
</details>
  
  
  <!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>laserframerestriciton</summary>
<p>

## undoselectionstack is the class which holds history of selections, so you can go through this history.

### Getting Started
1. If you want to use this somewhere, first of all you have to call **createNewProject** on object of this class:
     - `projj` - reference for changing trajectory states 
```js
 void createNewProject(std::shared_ptr<std::vector<framesTrajectoryRelationsInfoStruct>> projj)
```  
  
---  
  
</p>
</details>
  
  
  <!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>OptechFileReader</summary>
<p>

## undoselectionstack is the class which holds history of selections, so you can go through this history.

### Getting Started
1. If you want to use this somewhere, first of all you have to call **createNewProject** on object of this class:
     - `projj` - reference for changing trajectory states 
```js
 void createNewProject(std::shared_ptr<std::vector<framesTrajectoryRelationsInfoStruct>> projj)
```  
  
---  
  
</p>
</details>
  
  
  <!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>optechinternalfilereader</summary>
<p>

## undoselectionstack is the class which holds history of selections, so you can go through this history.

### Getting Started
1. If you want to use this somewhere, first of all you have to call **createNewProject** on object of this class:
     - `projj` - reference for changing trajectory states 
```js
 void createNewProject(std::shared_ptr<std::vector<framesTrajectoryRelationsInfoStruct>> projj)
```  
  
---  
  
</p>
</details>
 
<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->


#### This library is used in:
##### 1. Creator app. 
-   qcloudaerialview, qcloudcutwindow, qsidewayview will be shown, when user selects **Profiles** from the side menu.
  
##### 2. ColorCalibrator app
##### 3. GarminPlayer app
##### 4. GlobalCloudColorizer app
##### 5. GarminPlayer app
##### 6. GoProPlayer app

<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
<script src="https://unpkg.com/react@15.6.1/dist/react.min.js"></script>
<script src="https://unpkg.com/react-dom@15.6.1/dist/react-dom.min.js"></script>
