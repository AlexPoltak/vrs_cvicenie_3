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
  ### Getting Started
1. To start, simply create object of this class and then you can use corresponding methods.
2. You can also create and init object of this class by using conscructor:
```js
gpsNMEAparser();
```
&emsp;Or:
```js
gpsNMEAparser(std::string GGASentence,std::string RMCSentence)
```  
  
  GGA sentence looks like as follow:
  <img src="https://github.com/AlexPoltak/vrs_cvicenie_3/blob/main/Src/GPGGA.png">
  
1. To check whether some GGA sentence is valid use **isValidGGA** on object:
  
    - Returns true when sentence is valid GGA sentence, else returns false
```js
bool isValidGGA(const std::string GGASentence)
```
2. To set class values parsed from GGA sentence use:

```js
void setValuesGGA(std::string GGA)
```
  
  RMC sentence looks like as follow:
  <img src="https://github.com/AlexPoltak/vrs_cvicenie_3/blob/main/Src/GPMRCAsset%201.png">
  
1. To check whether some RMC sentence is valid use **isValidRMC** on object:
  
    - Returns true when sentence is valid RMC sentence, else returns false
```js
bool isValidRMC(const std::string RMCSentence)
```
2. To set class values parsed from RMC sentence use:

```js
void setValuesRMC(const std::string RMCSentence)
```
---  
  
</p>
</details>


<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>project</summary>
<p>

#### This class serves for manipulating with project that are created by user. Creation is done in the creator app where user have to load trajectory and lidar file(That files are required for project creation). Optionally user can load calibration and camera file, if were obtained.
Based on these files is created project thanks to which user can interact with all basic stuff(Trajectory displaying, selection, showing informations, profile generation and more).

  ### Getting Started
  
1. For access to all methods first create object of this class:
  
    - `c_qualityType` - quality indicator displayed on trajectory(0 - position,1 - heading, 2 - PDOP, 3 - speed)
    - `c_stdprecision` - maximum precision of position 
    - `c_minstdprecision` - minimum precision of position 
    - `c_stdprecisionHeading` - maximum heading precision
    - `c_minstdprecisionHeading` - minimum heading precision
    - `c_minPDOP` - disable/enable calculation
    - `c_maxPDOP` - disable/enable calculation
    - `c_minSpeed` - minimum speed precision
    - `c_maxSpeed` - maximum speed precision
    - `c_smartfilter` - whether smart filter is enabled(remove scans while standing)
    - `c_speedfilter` - whether speed filter is enabled
    - `c_speedfilterThreshold` - speed threshold for speed filter
  
All this input parameters user can change in tab settings
 ```js
    std::shared_ptr<Project> nameOfProjectObject=std::make_shared<Project>( int c_qualityType, double c_stdprecision, double c_minstdprecision, double c_stdprecisionHeading, double c_minstdprecisionHeading,double c_minPDOP, double c_maxPDOP,double c_minSpeed, double c_maxSpeed, bool c_smartfilter, bool c_speedfilter, double c_speedfilterThreshold)
  ``` 
  
#### Then some required steps:
1. Setting project path that contains project file name(to this file project will be saved)
```js
  void Project::setProjectFilename(QString newProjectFile)
```  

2. Setting trajectory file path
```js
  void Project::setTrajectoryFilename(QString newTrajectoryFile)
```  
 
3. Setting lidar file path
      
     - `index` - ID of lidar
    
```js
  void Project::setLidarFilename(QString newLidarFile,int index)
```  
##### Then you can do:

1. Setting path to calibration file:
```js
  void Project::setProjectFilename(QString newProjectFile)
```  
2. Setting path to camera files:

     - `newCameraFile` - path to files
     
      | VideoType     | 
      | :-------------| 
      | garmin_virb   |
      | labpano       | 
      | gopro         | 
      | sony          |
     
```js
void Project::setCameraFilename(QString newCameraFile,VideoType type)
```  



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
    - `restriction` - restriction to add some points to 
