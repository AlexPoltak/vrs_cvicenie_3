<!-- PROJECT LOGO -->
<br />
<div align="left">
<img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/logo_black.svg#gh-light-mode-only">
<img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/logo_white.svg#gh-dark-mode-only">
</div>
  <h1 align="left">Libs project</h1>

## This library serves to manage the project defined by user.
 - It combines creating, editing, opening project.
 - Data for project are reads from lidar, camera, calibration and trajectory files.
 - Project is XML file with suffix .PRJ
 - Creation of project is defined in Creator app.
 
This library Consists of:

<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>gpsnmeaparser</summary>
<p>

## gpsnmeaparser is class using for parsing GNSS Logs, specifically GGA and RMC logs.

<table>
    <thead>
        <tr>
            <th>Layer 1</th>
            <th>$GPGGA,hhmmss.ss,llll.ll,a,yyyyy.yy,a,x,xx,x.x,x.x,M,x.x,M,x.x,xxxx*hh</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan=4>L1 Name</td>
            <td rowspan=2>L2 Name A</td>
            <td>L3 Name A</td>
        </tr>
        <tr>
            <td>L3 Name B</td>
        </tr>
        <tr>
            <td rowspan=2>L2 Name B</td>
            <td>L3 Name C</td>
        </tr>
        <tr>
            <td>L3 Name D</td>
        </tr>
    </tbody>
</table>
  
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

<details><summary>project</summary>
<p>

### baselidarreader is a template for all lidar file readers which inherit from this class

 All reader inherited from this class should contain methods:
  
1. Open prepared file:
  
    - `pcapfile` - lidar file

&emsp;&emsp;If given file exists **returns true**, else **returns false**.
```js
int openPreparedFile(std::string pcapfile)
``` 
2. Basic Init of prepared file:
  
    - `pcapfile` - lidar file

&emsp;&emsp;If given file was inited **returns number of frames**.
```js
int initFile(std::string pcapfile)
``` 
3. Inits frames data info structure based on the info from lidar file and given transformation:
  
    - `outputData` - holds all frames data info structures, its main output of this method
    - `pcapfile` - lidar file
    - `transformation` - transformation assigned to frames
    - `timeoffset` - timestamp offset
    - `stopcalculating` - disable/enable calculation

&emsp;&emsp;If given file was inited **returns size of output data**.
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
  
  
6. To obtain some lidar frame use method **getLasFrame** on object:
  
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

9. If you want to find out the lidar model use method:
  
```js
int getLaserModelType()
``` 


10. To get id of lines for a preset option call **getUnusedLidarLinesForPresetOption**:
  
| whichLines option | 
| :-------------|
| All           |
| Central       | 
| EverySecond   | 
| HighRes       | 
| UltraHighRes  |
  
&emsp;&emsp;This output is used for filtering, when only some of the laser data are wanted
```js
std::vector<int> getUnusedLidarLinesForPresetOption(LidarLinesPresets whichLines);
```
  
---

</p>
</details>

<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>rtkpoints</summary>
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
  
