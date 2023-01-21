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

#### This class serves for manipulating with project that are created by user. Creation is done in the creator app(by projectcreationdialog class) where user have to load trajectory, lidar and calibration file(That files are required for project creation). Optionally user can load camera file, if were obtained.
Based on these files is created project thanks to which user can interact with all basic stuff(Trajectory displaying, selection, showing informations, profiles generation and more).Most of operations with project are used in creator app(in corresponding classes)

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
  
> All this input parameters user can change in tab settings
 ```js
    std::shared_ptr<Project> nameOfProjectObject=std::make_shared<Project>( int c_qualityType, double c_stdprecision, double c_minstdprecision, double c_stdprecisionHeading, double c_minstdprecisionHeading,double c_minPDOP, double c_maxPDOP,double c_minSpeed, double c_maxSpeed, bool c_smartfilter, bool c_speedfilter, double c_speedfilterThreshold)
  ``` 
  
2. If you want to clear project and all values based on which informations are displayed in UI,  use:
  
```js
  void Project::clearProject()
```    
  
### Some required steps:
1. Setting project path that contains project file name(to this file will be stored project values after saving)
```js
  void Project::setProjectFilename(QString newProjectFile)
``` 

2. Setting trajectory file path:
```js
  void Project::setTrajectoryFilename(QString newTrajectoryFile)
```  
 
3. Setting lidar file path:
      
     - `index` - ID of lidar
    
```js
  void Project::setLidarFilename(QString newLidarFile,int index)
```  
  
4. Setting path to calibration file:
```js
  void Project::setCalibrationFilename(QString newCalibrationFile)
```  
5. Setting calibration values from calibration file:
  It returns true when everything was set correctly, else returns false
```js
  bool Project::setCoreConfigurationFromCalibrationFile(const char *filename)
```  
6. Preparing needed structure that holds all lidar and camera devices info:
  It is used after setting the calibration file
     
```js
void Project::initDevices()
```  
  
### Some other methods to initialize the project:

1. Setting path to camera files:

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
  
2. To check whether path to given camera(video,images) files is correct(whether directory contains relevant files) use:

```js
    int checkWhetherCameraPathCorrect(QString path,Project::VideoType videotype);
```  

3. To set description from user about project use:
  
```js
  void setProjectDescription(std::string descr)
```  
4. You can save this description also to file by:

```js
    void saveDescriptionToFile(std::string path)
```  
  
```diff
- Most of the previous methods you can see in creator app, specificaly in projectcrationdialog class. This dialog box show up when the user selects option to create new project.
```
  
### Saving and opening/reading project:
1. When the required steps have been taken or some modification in project have been made, to save project with all values use **saveProjectFile** method. Project will be saved to XML file with .PRJ sufix.

```js
    void Project::saveProjectFile()
```  
  
> This method contains method **saveProjectFileToXml**, that saves all project values to XML file.

2. To open project file and read all values from it use:

    - `filename` - path to project file

```js
ProjectOpeningStatus Project::openProjectFromFile(QString fileName)
```  
 
> This method contains method **readProjectFileFromXml**(new project version),**readProjectFile**(old project version) that serves to parse all values from lidar, calibration,trajectory and camera files and assigns all needed variables from them.

  
3. To check whether given file is XML file use:
  
```js
  bool Project::isProjectFileXML(QString fileName)
```  

4. To get name of current opened project use:
  
```js
  QString Project::getProjectFilename()
```  
   
5. To get registry name of current opened project (it is used to add project to recent projects and so on) call:
  
```js
QString Project::getRegistryEntryNameOfProject()
``` 
  
  
### Manipulating with trajectory, lidar frames:
  
1. This returns indexes of **trajectory frames** that are selected(has state=2) - trajectory frames that user selects in selection mode :
  
```js
  std::vector<int> Project::getSelectedFrames()
```  
  
2. This returns indexes of **lidar frames**, based on trajectory selections (where state=2) :
  
      - `index` - lidar ID

```js
  std::vector<int> Project::getSelectedFramesForLidarDevice(int index)
```  

3. To get index of **frame from lidar**(with given ID) which is placed at given trajectory position use:
  
      - `whichTrajectoryPoint` - ID of trajectory point at which the ID of lidar frame should be returned
      - `whichLidar` - lidar ID

```js
  int Project::getLidarFrameFromTrajectoryRelationInfo(int whichTrajectoryPoint, int whichLidar)
```    
  
4. To obtain **lidar frames** indexes based on given trajectory indexes and lidar ID use **getSelectedFilteredFramesForLidarDevice**:
 &emsp;If there are some missing trajectory indexes in input, the space in corresponding lidar frames indexes will be filled in return.

    - `preselected` - IDs of trajectory points at which the IDs of lidar frame should be returned
    - `index` - lidar ID

```js
std::vector<int>  Project::getSelectedFilteredFramesForLidarDevice(std::vector<int> &preselected,int index)
``` 

5. To receive relational vector between lidar frames and trajectory use:
```js
std::vector<FrameData>& Project::getFramesTrajectoryRelationsInfoAsReference()
``` 
   &emsp; To get reference on this relational vector call:
```js
std::vector<FrameData>* Project::getFramesTrajectoryRelationsInfoAsPointer()
``` 

6. To get trajectory frames structure(info about selected frames and so on) call **getTrajectoryRealtionInfoPtr()**. It is used for undostack operations and so on:
  
```js
  std::shared_ptr<std::vector<framesTrajectoryRelationsInfoStruct>> getTrajectoryRealtionInfoPtr()
```
  
>  It is generated from trajectoryTransformation vector that is prepared by trajectory reader in reading methods for opening project
  
  
  
7. To obtain trajectory transformations vector, that is prepared by trajectory reader in reading methods for opening project, call:
  
      - `withModification` - when is true and some modification by fit points are done, the modified trajectory transformations will be returned

```js
std::vector<Transformation> &getTrajectoryTransformation(bool withModification=false)
```
   &emsp; To get zero(first) transformation from this vector use:
  
```js
const Transformation &getZeroPositionFromTransformation()
```
   &emsp; To get length of transformation vector(length of trajectory) use method **getTrajectoryLength**. It is used in Graph making:

```js
  int getTrajectoryLength()
```
  
8. To get indexes of lidar lines based on preset value call:
  
    - `whichlidar` - ID of lidar
        | whichlines    | 
        | :-------------| 
        | All           |
        | Central       | 
        | EverySecond   | 
        | HighRes       |
        | UltraHighRes  |

```js
std::vector<int> Project::getUnusedLaserLinesForLidar(int whichlidar, BaseLidarReader::LidarLinesPresets whichlines)
```     
  
9. To clear selection of trajectory(changing value of all trajectory states to state=0) call:
```js
  void Project::clearTrajectorySelection()
```
  
  
###  Wrappers for trajectory matters(inertial explorer filereader) :
  
1. To generate transformations for trajectory call **traj_generateTransformation**. It is used in projectcreationdialog class.
  
```js
  void Project::traj_generateTransformation()
```   
  
2. To read trajectory file and inits values for trajectory reader class(inertialExplorerFileReader) call **traj_readTrajectoryFromFile**. It is used in projectcreationdialog class and in project reading methods.
  
```js
  int Project::traj_readTrajectoryFromFile(QString rawTrajFile)
```     
  
  
### Creating and manipulating with **line cutting segment**. 
  It is displayed in profile mode on map(creator app), when user clicks somewhere on trajectory. Based on this line segment(frames that are inside) is generated pointcloud to display in profiles.:

Line cut segment variable holds points that defines itself:

| Range from                        | Range to                         |      | definition          |
| :-------------                    | :-------------                   |------| :-------------      | 
| lineCutSegment->cutRectangle[0]   |                                  |      | center of line( where user clicked on trajectory)     | 
| lineCutSegment->cutRectangle[1]   | lineCutSegment->cutRectangle[4]  |      | perimeter points of line that indicates width of XY projection-aerial view    |
| lineCutSegment->cutRectangle[5]   | lineCutSegment->cutRectangle[8]  |      | perimeter points of line that indicates width of ZX projection-cut view  |
| lineCutSegment->cutRectangle[9]   | lineCutSegment->cutRectangle[12] |      | perimeter points of line that indicates sideway cut       | 
  
  
1. To prepare line cut segment points use method getPerpendicularLineSegmentAtTrajectory(). Line segment is generated perpendicular to given trajectory place.

      - `trajectoryID` - ID of trajectory point to which the points of perpendicular line segment are calculated
      - `segmentLength` - length of line segment(defined by user)
      - `segmentWidth` - width of XY projection-aerial view(defined by user)
      - `cutWidth` - width of cut (width of ZX projection-cut view defined by user)
  
```js
std::vector<QPointF> Project::getPerpendicularLineSegmentAtTrajectory(int trajectoryID, double segmentLength,double segmentWidth,double cutWidth)
```    
  
  
```diff
- Points of this line cut segment and more visual parameter you can get by method getParamsForMapStruct()
```
  
2. To obtain IDs of trajectory for prepared line segment use method **getFramesForPerpendicularLineSegment**. Based on this IDs is generated pointcloud for projections in creator app:

      - `limits` - limits of line segment(you can use return value from method **getPerpendicularLineSegmentAtTrajectory**)
      - `selectedId` - ID of trajectory where line segment is created
      - `segmentWidth` - width of XY projection-aerial view( set by user)

```js
std::vector<int> Project::getFramesForPerpendicularLineSegment(std::vector<QPointF> limits,int selectedId,double segmentWidth)
```  
  
3. To prepare line cut segment for sideway view(ZY projection) use:

      - `cx` - X position of sideway cutting line center
      - `cy` - Y position of sideway cutting line center
      - `rx` - direction vector of sideway cutting line
      - `ry` - direction vector of sideway cutting line

```js
std::vector<pcl::PointXYZRGB> Project::getPerpedicularLineSegmentForSidewayCut(double cx,double cy, double rx,double ry,int trajectoryID,int rtkID, double segmentLength,double segmentWidth,double cutWidth)

```  
 > It returns points of prepared line segment in order:<br>
    - [0] center point of cutting line<br>
    - [1] right centered point of cut(on right side of trajectory)<br>
    - [2] left centered point of cut(on left side of trajectory)
  
4. To get number of zones in current prepared line cut segment, use:
  
 ```js
int getLineCutSegmentZonesCount()
``` 
  
5. To get angle between zones of points in prepared cutting line segment call:
  
      - `firstZone` - ID of some zone in cutting line segment
      - `secondZone` - ID of another zone in cutting line segment

> Returns angle in radians. It is used in correction to shift zone in chosen angle.
```js
double Project::getAngleBetweenLineCutSegmentZones(int firstZone, int secondZone)
```  
  
6. To get GPS timestamp for given zone in prepared cutting line segment use:

      - `i` - ID of zone in cutting line segment

```js
double Project::getTimeOfLineCutSegmentZone(int i)
```  

7. To receive trajectory ID for given zone of prepared line cut segment use:
  
      - `i` - ID of zone in cutting line segment

```js
int Project::getTrajectoryIDOfLineCutSegmentZone(int i)
```  
  

8. To clear line cut segment variable that holds zones and points of cutting lines use **clearLineCut** method.
  
```js
void clearLineCut()
```  
  
### Methods for creating and manipulating with corrections of pointcloud
  In profile mode user can click somewhere on trajectory and make corrections there in a few steps. Firstly user can measure in profile views with measurement tool(measurement button in menu among profile views), where pointcloud should be corrected. After measurement, user can create correction by pressing FIT button that is next to the measurement tool button.
  
1. To add fit point use method **addFitPoint**.  It is point(with correction structure) on trajectory, where user wants to create correction:

      - `positionID` - ID of trajectory position where correction should be made
      - `correctioninfo` -  correction info structure that will be added
      - `nearestPosible` - treshold to add fitpoint. If given fit point is closer to some existing fitpoint than this value,then this fitpoint will not be added.
      - `connectedDistance` - how far trajectory points can be from each other to be connected to same correction.
      - `fadeDistance` - how far end points should be from relevant fit point

> Returns true when fitpoint was added, else returns false.

```js
bool Project::addFitPoint(int positionID, FITpointCorection correctioninfo,double nearestPosible,double connectedDistance,double fadeDistance)
```  

2. To modify existing fitpoint use:
  
      - `positionID` - ID of trajectory position where correction should be modified
      - `correctioninfo` -  correction info structure that will replace the previous one
      - `nearestPosible` - it is not used there
      - `connectedDistance` - how far trajectory points can be from each other to be connected to same correction.
      - `fadeDistance` - how far end points should be from relevant fit point

> When point for modification does not exist, the new one will be added.

```js
bool Project::modifyFitPoint(int positionID, FITpointCorection correctioninfo,double nearestPosible,double connectedDistance,double fadeDistance)
```   
 
3. To add end points(where corrections on trajectory will end) for each fit point use:
  
      - `connectedDistance` - how far trajectory points can be from each other to be connected to same correction.
      - `fadeDistance` - how far end points should be from relevant fit point
  
> - If distance between neighboring fitpoints is lower than connectedDistance, existing end points will be shifted.<br>
> - If distance between neighboring fitpoints is higher than connectedDistance, new end points will be added<br>
>  - Endpoints in fitpoints std::map variable have for distinctions negative value of trajectory ID. therefore when you want to access to trajectory, use abs of this value.
  

```js
void Project::addEndPointsToFitCorrections(double connectedDistance,double fadeDistance)
```    

  getFitpointsAsReference

  addEndPointsToFitCorrections
  calcCorrectionFromFitPoints
  correctionExists
  saveFitPoints
  loadFitPoints
  getTrajectoryCorrectionForZone
  
  
  getParamsForMapStruct
 setVisualQualityParameter
    setTrajectoryDisabling

  
  getRTKpointsAsReference
  filterRTKpointsByProjectBoundaries
  addRTKpoint
 
  
  getIMUtoVehicleRotation
  getBoresightRotation
  getTrajectoryTimepositions
  
  
  
  
  
  
  
  ### Manipulating with visual parameters - this variables user can choose in settings tab:
```diff
- Most of this visual parameters are described in object creation method of this class.
```
1. To get value of specific visual parameter use:

```js
{return type} Project::getVisualParameter{name of paramter}()
``` 
  
2. To set value of specific visual parameter use:

```js
    void setVisualParameter{name of parameter}(value)
``` 
  
3. For obtaining whether shake filter is enabled use:

```js
  bool getUseShakeFilter()
``` 
  
&emsp; &emsp;To set it use:
```js
  void setUseShakeFilter(bool usefilt)
```  
  
  
  
 
