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
 - Creation of project is defined in Creator app with projectcrationdialog class.
 
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
  
    - `c_qualityType` - quality indicator displayed on trajectory:&emsp;0 - position<br>     &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&nbsp;
1 - heading<br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&nbsp;
2 - PDOP<br>     &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&nbsp;
3 - speed
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
  
> All this input parameters user can change in tab settings.(It is done by minmaxprecisiondialog class in creator app)
 ```js
    std::shared_ptr<Project> nameOfProjectObject=std::make_shared<Project>( int c_qualityType, double c_stdprecision, double c_minstdprecision, double c_stdprecisionHeading, double c_minstdprecisionHeading,double c_minPDOP, double c_maxPDOP,double c_minSpeed, double c_maxSpeed, bool c_smartfilter, bool c_speedfilter, double c_speedfilterThreshold)
  ``` 
  
2. If you want to clear project values based on which informations are displayed in UI,  use:
  
```js
  void Project::clearProject()
```    
<details><summary>&emsp;&emsp; Some required step to create project </summary>  <!--////////////////////////////////////////////////////////////////////// --></br>

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
  
</details>

<details><summary>&emsp;&emsp; Some other methods to initialize the project </summary>  <!--////////////////////////////////////////////////////////////////////// --></br>

1. Setting path to camera files:

     - `newCameraFile` - path to files
     
     -  | VideoType     | 
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
</details>


<details><summary>&emsp;&emsp; Saving and opening/reading project </summary>  <!--////////////////////////////////////////////////////////////////////// --></br>

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
  
</details>

<details><summary>&emsp;&emsp; Manipulating with trajectory/lidar frames/points </summary>  <!--////////////////////////////////////////////////////////////////////// --></br>

####  Frames
    
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

5. To get lidar frame structure use:

    - `whichLidar` - ID of lidar whose frame will be returned
    - `localfile` - lidar file in which the frame will be searched
    - `index` - index of frame, which should be returned
    - `lidToFrame` - lidar transformation
    - `restriction` - restriction to add some points to frame

> It is used for export in pointcloudExporter class

```js
  BaseFrame Project::getLidarFrameFromLidar(int whichLidar,std::ifstream &localfile,int index,CLidarToFrameTrans *lidToFrame,laserFrameRestrictionBase *restriction,int &openedFileID,int colormodel ,double minIntensityColor,double maxIntensityColor )
``` 


6. To save/get selected frames(selected by user in selection mode) to/from file for access in another app use:
&emsp; &emsp;To save use:
```js
  bool saveProjectSelectionToXml();
```  
> Method returns true when saving was successful, else returns false

&emsp; &emsp;To get selected frames from saved file use:
```js
  std::vector<int> Project::getSelectionFromXml()
```  
> Method returns IDs of selected frames

7. To clear selection of trajectory(changing value of all trajectory states to state=0) call:
```js
  void Project::clearTrajectorySelection()
```

8. To receive relational vector between lidar frames and trajectory use:
```js
  std::vector<FrameData>& Project::getFramesTrajectoryRelationsInfoAsReference()
``` 
   &emsp; To get reference on this relational vector call:
```js
  std::vector<FrameData>* Project::getFramesTrajectoryRelationsInfoAsPointer()
``` 

9. To fill/get trajectory frames structure(info about selected frames and so on) call **getTrajectoryRealtionInfoPtr()**. It is used for undostack operations and for some visualizations on map...:
  
&emsp; &emsp;To get this structure use:

```js
  std::shared_ptr<std::vector<framesTrajectoryRelationsInfoStruct>> getTrajectoryRealtionInfoPtr()
```

&emsp; &emsp;To fill this structure use:

```js
  void Project::fillFramesTrajectoryRelationsInfo(int trajectoryType)
```

```diff  
 - It is generated/filled with **trajectoryTransformation** vector that is prepared by trajectory reader in reading methods for opening project
```


10. To obtain trajectory transformations vector, that is prepared by trajectory reader in reading methods for opening project, call:
  
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
<br>

#### Points

1. To find the point that is at given distance before the point with given ID use:

    - `fromWhichpoint` - ID of trajectory point(in framesTrajectoryRelationsInfo structure) from which the point before it is searched.
    - `dist` - distance from "fromWhichpoint" point
 
> It returns transformation ID of point that is at given distance

```js
  int Project::findPointTrajectoryInDistBefore(int fromWhichpoint, double dist)
```
<br>

2. To find the point that is at given distance after the point with given ID use:

    - `fromWhichpoint` - ID of trajectory point(in framesTrajectoryRelationsInfo structure) from which the point behind it is searched.
    - `dist` - distance from "fromWhichpoint" point

> It returns transformation ID of point that is at given distance

```js
  int Project::findPointTrajectoryInDistAfter(int fromWhichpoint, double dist)
```

<br>

3. To find out the distance between two trajectory points use method:

    - `first` - ID of first trajectory point(in framesTrajectoryRelationsInfo structure)
    - `second` - ID of second trajectory point(in framesTrajectoryRelationsInfo structure)

```js
  double Project::findDistBetweenTrajectoryPoints(int first, int second)
```

</details>
  
  
<details><summary>&emsp;&emsp; Wrappers for trajectory matters(inertial explorer filereader)  </summary>  <!--////////////////////////////////////////////////////////////////////// --></br>
  
1. To generate transformations for trajectory call **traj_generateTransformation**. It is used in projectcreationdialog class.
  
```js
  void Project::traj_generateTransformation()
```   
  
2. To read trajectory file and inits values for trajectory reader class(inertialExplorerFileReader) call **traj_readTrajectoryFromFile**. It is used in projectcreationdialog class and in project reading methods.
  
```js
  int Project::traj_readTrajectoryFromFile(QString rawTrajFile)
```     
  
3. To inits relational vector between lidar data and trajectory file use **traj_initFileWithTransformations**. It is used in projectcreationdialog class.
  
```js
  void Project::traj_initFileWithTransformations(int index)
```

4. To obtain trajectory constrains use **traj_getFileConstrains**. It is used in projectcreationdialog class.
  
```js
  void Project::traj_getFileConstrains(InertialExplorerBoxData &constrains)
```

</details>
  
<details><summary>&emsp;&emsp; Creating and manipulating with line cutting segment and relevant zones </summary>  <!--////////////////////////////////////////////////////////////////////// --></br>

  It is displayed in profile mode on map(creator app), when user clicks somewhere on trajectory. Based on this line segment(frames that are inside) is generated pointcloud to display in profiles.:

#### Line cut segment
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
  
  
 4. To clear line cut segment variable that holds zones and points of cutting lines use **clearLineCut** method.
  
```js
  void clearLineCut()
```  

#### Zones
1. To get number of zones in current prepared line cut segment, use:
  
 ```js
  int getLineCutSegmentZonesCount()
``` 
  
2. To get angle between zones of points in prepared cutting line segment call:
  
      - `firstZone` - ID of some zone in cutting line segment
      - `secondZone` - ID of another zone in cutting line segment

> Returns angle in radians. It is used in correction to shift zone in chosen angle.
```js
  double Project::getAngleBetweenLineCutSegmentZones(int firstZone, int secondZone)
```  
  
3. To get GPS timestamp for given zone in prepared cutting line segment use:

      - `i` - ID of zone in cutting line segment

```js
  double Project::getTimeOfLineCutSegmentZone(int i)
```  

4. To receive trajectory ID for given zone of prepared line cut segment use:
  
      - `i` - ID of zone in cutting line segment

```js
  int Project::getTrajectoryIDOfLineCutSegmentZone(int i)
```  
  

</details>
  
  
<details><summary>&emsp;&emsp; Methods for creating and manipulating with corrections of pointcloud </summary>  <!--////////////////////////////////////////////////////////////////////// --></br>

  In profile mode user can click somewhere on trajectory and make corrections there in a few steps. Firstly user can measure in profile views with measurement tool(measurement button in menu among profile views), where pointcloud should be corrected. After measurement, user can create correction by pressing FIT button that is next to the measurement tool button.
  
1. To add fit point use method **addFitPoint**.  It is point(with correction structure) on trajectory, where user wants to create correction. Correction structure holds user measurements, trajectory time and so on:

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
>  - Endpoints in fitpoints std::map variable have for distinctions negative value of trajectory ID. Therefore, when you want to access to trajectory by ID use absolut value of this map key.
  

```js
  void Project::addEndPointsToFitCorrections(double connectedDistance,double fadeDistance)
```    

4. To calculate and apply corrections based on added fitpoints use:
  
      - `connectedDistance` - how far trajectory points can be from each other to be connected to same correction.
      - `holdDistance` - if fit points are not connected, how far to hold the correction value
      - `fadeDistance` - how far end points should be from relevant fit point
  
> This prepare **modifiedtrajectoryTransformation** variable which can be obtained by method **getTrajectoryTransformation** described in section ** Manipulating with trajectory, lidar frames**:
  
```js
  void Project::calcCorrectionFromFitPoints(double connectedDistance,double holdDistance,double fadeDistance)
```   
  
5. To find out whether corrections were created use:
  
> It returns true when some corrections were created, else returns false  
```js
  bool correctionExists()
```   
  
6. To get reference of created fitpoints for access to them use:
  
```js
  std::shared_ptr<std::map<int,FITpointCorection>> Project::getFitpointsAsReference()
```  
7. To save all crated correction fit points for future reconstruction of corrections use:
  
```js
  bool Project::saveFitPoints()
```  
  
8. To load saved fitpoints for reconstruction of created corrections call>
  
> It returns true when fitpoints were loaded, false when loading of fitpoint file was incorrect 
  
```js
  bool Project::loadFitPoints()
``` 
  
  
9. To obtain calculated trajectory corrections for zone in prepared cutting line segment use:
  
      - `holdDistance` - ID of zone in prepared cutting line segment
  
> Creating and manipulating with cutting line segment is described in section **Creating and manipulating with line cutting segment**
  
```js
  correction Project::getTrajectoryCorrectionForZone(int i)
``` 
  
</details>


  
<details><summary>&emsp;&emsp; Manipulating with visual parameters (contains filters and auxliary methods that can be used) </summary>  <!--////////////////////////////////////////////////////////////////////// --></br>

#### User can change visual parameters in settings tab

```diff
- Most of this visual parameters are described in object creation method of this class.
```

1. These visual parameters, cutting line segment and more you can obtain in structure by method **getParamsForMapStruct**:

> This structure is used in mymapcontrol class to draw trajectory, line cutting segment and other stuff on map.

```js
  ParametersForMapInteraction Project::getParamsForMapStruct()
``` 
 <br>  <br> 
 
2. To get value of specific visual parameter use:

```js
  {return type} Project::getVisualParameter{name of paramter}()
``` 
  
  <br> 
  
3. To set value of specific visual parameter use:

```js
  void setVisualParameter{name of parameter}(value)
``` 
  
  <br> 
  
4. For obtaining whether shake filter is enabled use:

```js
  bool getUseShakeFilter()
``` 
  
&emsp; &emsp;To set it use:
```js
  void setUseShakeFilter(bool usefilt)
```  
 
 <br> 
 
5. This sets prepared quality parameter to framesTrajectoryRelationsInfo structure. (It is used for coloring the trajectory by quality type in mymapcontrol class).

  
```js
  void Project::setVisualQualityParameter()
```
```diff
- Use setVisualQualityParameter method when quality parameter was changed
```

<br> 

6. To use trajectory disabling based on filters in usage use:
```js
  void Project::setTrajectoryDisabling()
```

&emsp;It contains **disableTrajectoryPartsByDiff** method that represents shake filter:<br> 
&emsp;&emsp;    - `secAfter` - seconds after problem point. 

> Points that are in given time(secAfter input) from problem point, will be also disabled.

```js
void Project::disableTrajectoryPartsByDiff(int secAfter)
```

<br> 
   
7. To clear all trajectory disabling use:
```js
  void Project::clearTrajectoryDisabling()
```

<br>

8. To get indexes of lidar lines based on preset value call:
  
    - `whichlidar` - ID of lidar
    -   | whichlines    | 
        | :-------------| 
        | All           |
        | Central       | 
        | EverySecond   | 
        | HighRes       |
        | UltraHighRes  |

```js
  std::vector<int> Project::getUnusedLaserLinesForLidar(int whichlidar, BaseLidarReader::LidarLinesPresets whichlines)
```     


</details>
  
  
  
<details><summary>&emsp;&emsp; Manipulating with transformations, rotations and time offsets of/between devices(lidar, camera, body) </summary>  <!--////////////////////////////////////////////////////////////////////// --></br>

#### Transformations
1. Transformation of lidar device:<br>
  To set this transformation:
```js
  void Project::setLidarTransformation(Transformation newTransform, int lidarIndex, double gain)
```  
&emsp; &emsp;To get this transformation:
```js
  Transformation Project::getLidarTransformation(int lidarIndex, double gain)
```   
 <br> 
 
2. Transformation of camera device:<br>
  To set this transformation:
```js
  void Project::setCameraTransformation(Transformation newTransform, int cameraIndex, double gain)
```  
&emsp; &emsp;To get this transformation:
```js
  Transformation Project::getCameraTransformation(int cameraIndex, double gain)
```   
   
 <br> 
 
3. Transformation between lidar and IMU:<br>
  To set this transformation use:

```js
  void Project::setTransformationLidar_IMU(Transformation newTransform, int lidarIndex, double gain)
```  
&emsp; &emsp;To get this transformation use:
```js
  Transformation Project::getTransformationLidar_IMU(int lidarIndex,double gain)
```  
&emsp; &emsp;To clear this transformation use:
```js
  void Project::clearTransformationLidar_IMU(int lidarIndex)
```  
<br> 

4. Transformation between camera and IMU:<br>
  To get this transformation use:
```js
  Transformation Project::getTransformationCamera_IMU(int cameraIndex)
```  
 <br> 
 
5. Transformation between IMU and vehicle(what the devices are connected to):
```js
  Transformation Project::getTransformationIMU_Vehicle()
```  


6. To obtain transformation ID of given frame for given lidar use:
    - `whichDevice` - ID of lidar
    - `whichFrame` - ID of frame for which the ID of transformation should be returned
   
> It is used in pointcloudexporter class
  
```js
  int Project::getTransformationPostionOfDeviceFrame(int whichDevice, int whichFrame)
```   
  
  
7. To obtain transformation ID of given frame for given camera use:
    - `whichDevice` - ID of camera
    - `whichFrame` - ID of frame for which the ID of transformation should be returned

> It is used in pointcloudexporter class

```js
  int Project::getTransformationPostionOfDeviceFrame(int whichDevice, int whichFrame)
```   
#### Rotations

1. Rotation  between IMU and vehicle(what the devices are connected to):
  To get this transformation use:
```js
  Transformation Project::getIMUtoVehicleRotation()
```  
  <br> 
  
2. Rotation of lidar device:
  To get this rotation:
```js
  double Project::getLidarRotation(int lidarIndex)
```   
 <br> 

3. Rotation of camera device:<br>
  To set this rotation:
```js
  void Project::setCameraRotation(int cameraIndex,double rotation)
```  
&emsp; &emsp;To get this rotation:
```js
  double Project::getCameraRotation(int cameraIndex)
```   

 <br> 

4. To get boresight rotation use :
```js
  Transformation Project::getBoresightRotation()
``` 
#### Time offsets

1. Time offset of lidar device:<br>
  To get this offset:
```js
  double getLidarTimeOffset(int lidarIndex)
```   

 <br> 

2. Time offset of camera device:<br>
  To set this offset:
```js
  void setCameraTimeOffset(int cameraIndex,double newOffset);
```  
&emsp; &emsp;To get this offset:
```js
  double getCameraTimeOffset(int cameraIndex);
```   


</details>
  
<details><summary>&emsp;&emsp; Adding RTK points </summary>  <!--////////////////////////////////////////////////////////////////////// --></br>

```diff
- This methods serves to add RTK points, when user load them, to project file.
- Loading of RTK points is done in CreatorMainWindow class speciffically by AddRtkPointDialog widget class
- Manipulating with RTK points and making some corrections by them is done in class rtkpoints also described in this README.
```

1. To add/prepare RTK point to RTK vector variable use:

> This is used in **reconstructProfile method** to reconstruct profiles from fitpoint that user choosed. Fitpoint is there added to RTK point vector.

```js
  void addRTKpoint(RtkPoint newPoint);
```   

2. This saves all RTK points(previous added RTK points with appended input RTK points) to project file :

    - `pointsToAdd` - These points will be appended to RTK vector

```js
  void Project::addRTKpoint(std::shared_ptr<std::vector<RtkPoint>> pointsToAdd)
```   

3. To filter given RTK by trajectory boundaries use:

    - `pointsToFilter` - RTK Points to filter. These vector after filtering will hold only RTK points inside the boundaries

> This method returns number of filtered RTK points

```js
  int Project::filterRTKpointsByProjectBoundaries(std::shared_ptr<std::vector<RtkPoint>> pointsToFilter)
```   


4. To get reference of prepared/loaded RTK points vector use:
```js
  std::shared_ptr<std::vector<RtkPoint>> Project::getRTKpointsAsReference()
```   


5. To clear vector that holds RTK points use:
```js
  void Project::clearRtkPoints()
```   
  
  --- 
  
</details>


 <details><summary>&emsp;&emsp; Camera files preparation</summary>  <!--////////////////////////////////////////////////////////////////////// --></br>
 
  
 1. To prepare structure that holds camera data, info use:<br>
 
    - `cameraIndex` - ID of camera for which all should be prepared
    - `filename` - path to camera files
    -   | video_type-type of camera     | 
        | :-------------                | 
        | garmin_virb                   |
        | labpano                       | 
        | gopro                         | 
        | sony                          |

 > This method load camera files and prepare relational vector between video frames and trajectory. It is used for having access to specific video frame at given trajectory position and so on
 
```js
    int prepareAllVideoStuffForOneCamera(int cameraIndex, QString filename="", VideoType video_type=VideoType::garmin_virb);
```   
  &emsp;  Based on type of camera one of this relevant method is called in previous method :
  
   &emsp; **For Garmin camera preparation**
```js
    int prepareAllVideoStuffForOneCameraAsGarmin(int cameraIndex,QString filename="");
```   

   &emsp; **For LabPano camera preparation**
```js
    int prepareAllVideoStuffForOneCameraAsLabPano(int cameraIndex,QString filename="");
```   

   &emsp; **For Sony camera preparation**
```js
    int prepareAllImageStuffForOneCameraAsSony(int cameraIndex,QString filename="");
```   


2. To obtain filename of first video for given camera call:
```js
    QString getFirstVideoFilenameForCamera(int cameraIndex)
```   
  
3. To obtain transformation timestamp of given camera:
    - `cameraIndex` - ID of camera 
    - `id` - ID in relation vector between video frames and trajectory(cameraDataToTrajectoryRelVec)

```js
    double getTransformationTimestampForCameraRelVecID(int cameraIndex, int id)
```   
  
4. To obtain transformation ID of given camera:
    - `cameraIndex` - ID of camera 
    - `id` - ID in relation vector between video frames and trajectory(cameraDataToTrajectoryRelVec)

```js
    double getTransformationIDForCameraRelVecID(int cameraIndex, int id)
```   



</details>
---   
  
</p>
</details>

<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>rtkpoints</summary>
<p>
  
 #### Real-time kinematic positioning(RTK). These points are from other receiver-fixed base station. <br>
- It is used to provide corrections and increase the accuracy of points GNSS positions. <br>
- This class enables loading RTK points from file and transform them from/to chosen coordinate system to/from UTM. <br>
- Loading of RTK points is implemented by addrtkpointsdialog class in creator app. Dialog will open when user choose in profile mode the GCP in right menu and there press **add points** button.
 
  ### Getting Started
  
1. To start, simply create object of this class and then you can use corresponding methods.
```js
  rtkPoints::rtkPoints()
```
  
2. To set transformation(datum, coordinate grid system) use:
  
    - `gridname` - name of cadastral coordinate grid
    - `geoidname` - name of geoid model
    - `transfromationID` - ID of selected datum transformation. 
    - `utmzone` - number of UTM zone
    - `UTMzoneText` - UTM zone

> It is used in addrtkpointsdialog. There user can choose the datum. By this method the transformation(datum), chosen by user, will be set and then applied on RTK points.
  
```js
  void rtkPoints::setTransformation(std::string gridname,std::string geoidname, int transfromationID,int utmzone,char UTMzoneText[])
```
  
```diff
- Transformation(datum) have to be set by this method before loading RTK points with method **loadRtkPoints**
- Datums that user can choose in **addrtkpointsdialog UI** are manage in **proj4transforms class** and overall in lib Datums.
```

3. To load RTK points from file use:
    - `RTKpoints` - there will be append the RTK points loaded from file
    - `filename` - path to file with RTK points
    - `trajectoryTransformation` - trajectory transformations. Can be get by method **getTrajectoryTransformation** in project class
    -   | format - format of points that  should be used   | 
        | :-------------                                  | 
        | IDXYZ                                           |
        | IDYXZ                                           |

    - `ignorefirstLine` - number of lines in file that should be ignored/skipped

> - If points were loaded correctly, this method will return number of RTK points that are outside of trajectory boundaries(points that were not added).
> - If file was not opened correctly, returns -1
  
```js
  int rtkPoints::loadRtkPoints(std::shared_ptr<std::vector<RtkPoint>> RTKpoints,std::string filename,std::vector<Transformation> &trajectoryTransformation,fileFormat format, int ignorefirstLine)
```
  
 <br> 
  
   &emsp; All RTK points are in this method transformed from chosen transformation(datum) to UTM by:
  
```js
int rtkPoints::transformPoint(RtkPoint &point)
```

```diff
- before loading points by method **loadRtkPoints** have to be set Transformation(datum) using method **setTransformation** described in step 2.
```
  
</p>
</details>

