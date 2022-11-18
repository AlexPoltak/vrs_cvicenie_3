<!-- PROJECT LOGO -->
<br />
<div align="left">
<img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/logo_black.svg#gh-light-mode-only">
<img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/logo_white.svg#gh-dark-mode-only">
</div>
  <h1 align="left">Libs MAPInteraction</h1>

## This library is used in two main ways: 
1. For transformation between devices(lidar,camera,imu) and also between  what the device is connected to(drone, car, pedestrian).
2. For reading lidar file and manipulating with data obtainded from this file.<br />
### More in specific sections below. <br /><br />
This library Consists of:

<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>baseframe</summary>
<p>

## cvwidget is widget class where defined image is rendered.
There is included QT class QOpenGLWidget: <a href="https://doc.qt.io/qt-6/qopenglwidget.html">Show documentation</a>, thanks to which we can display OpenGL graphics.
  
### Getting Started
1. When you want to use this widget somewhere, first of all you have to add widget promoted to class **CQtOpenCVViewerGl** to .ui file.
  
2. Then you just call only function **showImage** on this widget, and defined image in widget will be rendered, also on resizing.</br>
If image shows properly this function **returns true**, else **returns false**. Function **showImage**:
```js
bool showImage(const cv::Mat& image)
```
3. If you want to get position on image, where user clicked, call **getImageClickPos** on widget:

    - `widgetpos` - position of widget from global
 ```js
QPoint getImageClickPos(QPoint widgetpos)
``` 
4. If you want to get position of point, which should be at the same position on image, also when widget is resized, call **getImagePosToWidgetPos** on widget: :
 ```js
QPoint getImagePosToWidgetPos(QPoint imagepos)
```
  
---  
  
</p>
</details>



<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>baselidarreader</summary>
<p>
  
## mymapcontrol is used to interact with the map. This class is part of an open-source cross-platform map widget QMapControl. 
  - QMapControl Contact e-mail: kaiwinter@gmx.de
  - QMapControl github: https://github.com/kaiwinter/QMapControl
  - Changes were made by Martin Dekan for the purpose of trajectory selection</br>
  
 QMapControl is implemented in external libs of lidaretto project.


### Getting Started
<details><summary>&emsp;&emsp; Needed steps to show map </summary>  <!--////////////////////////////////////////////////////////////////////// --></br>

1. To display the map and control it, first of all you have to add some container with QFrame class to .ui file.
2. Then promote this QFrame to class **MyMapControl**.
3. Add this <a href="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/Includes_for_mymapcontrol.txt">Includes</a> to .pro file of app.
4. Include to header file of application:
    - `#include "mymapcontrol.h"`
    - `#include <osmmapadapter.h>`
    - `#include <maplayer.h>`
    - `#include "common.h"`

5. You need to create new map adapter(example is for OpenStreetMap):
    
    - `MapAdapter* mapadapter;`
```js
mapadapter = new OSMMapAdapter();
```
6. Create new layer with map adapter created in previous step:
    
    - `Layer* mainlayer;`
```js
mainlayer = new MapLayer("OpenStreetMap-Layer", mapadapter);
```
7. Call **__init()** on map QFrame (created in 1. and 2. step) to initialize all needed values:
```js
 void __init()
```
8. To add layer created in step 6(or another layer) to layers of map, call **addLayer** on map QFrame:
```js
void addLayer(Layer* layer)
```
</details>


<details><summary>&emsp;&emsp; Methods of selecting trajectory points </summary> <!--/////////////////////////////////////////////////////////////////////// --></br>
Points of trajectory can be in 4 different states:

| state number  | state                          |
| :-------------| :-------------                 | 
| 0             | not selected                   |
| 1             | during selection(prepared)     |
| 2             | selected                       |
| 3             | split point                    |

1. To set type of mouse mode and type of selection, call **setMouseMode** on map QFrame: 

| enum MouseMode      | enum MouseMode-Description                          |      | enum SelectionType  |
| :-------------      | :-------------                                      |------| :-------------      | 
| Panning             | The map can be moved                                |      | CircleSelection     | 
| Dragging            | Selection rectangular area can be drawn in the map  |      | PolygonSelection    |
| None                | Mouse move events have no efect to the map          |      | RectangleSelection  |
| Selecting           | Selecting a trajectory                              |      | TimeSelection       | 
| Deselecting         | Deselecting a trajectory                            |      | AreaSelection       | 
| InsertSplitPoint    | Inserting split point                               |      | CircleDeselection   |
| PointInfoSelection  | Painting info about selected point on trajectory    |      | RectangleDeselection|
| LineForCut_Selecting| It is used to select point for cutting line         |      | TimeDeselection     |


```js
void setMouseMode(MouseMode mousemode,SelectionType selectiontype )
```


2. To select all points among defined points(to change state to "selected"), call **selectInTrajectory** on map QFrame: 

    - `start` - index of start point
    - `goal` - index of end point

```js
void selectInTrajectory(int fromPoint,int toPoint)
```
3. To deselect all points among defined points(to change state to "not selected"), call **deselectInTrajectory** on map QFrame: 

    - `start` - index of start point
    - `goal` - index of end point

```js
void deselectInTrajectory(int fromPoint,int toPoint)
```

</details>  

---

</p>
</details>

<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>clidartoframetrans</summary>
<p>

## Is frame class which projects cloud points into one coordination plane. Specifically to the plane XY(aerial).</br>
This class also takes care of the interaction during measurement(in this frame) or selection cutting line(emits to each projection frame except ZX-side way).
  
### Getting Started
1. When you want to use this view somewhere, first of all you have to add frame promoted to class **QCloudAerialView** to .ui file.

2. To show this view with painted cloud points, call **addAndShowCloud** on this frame:
  
    - `inputcloud` - the entire cloud that generated the backend for display
    - `llp1` - right centered point of cut(on right side of trajectory)
    - `llc1` - centered point of cut, defined by user
    - `llp2` - left centered point of cut(on left side of trajectory)
    - `cutwidth` - distance from cut
    - `newusedZones` - zones which are used

```js
void QCloudAerialView::addAndShowCloud(cloudViz inputcloud,pcl::PointXYZRGB llp1,pcl::PointXYZRGB llc1,pcl::PointXYZRGB llp2,double cutwidth,std::map<int, bool> newusedZones)
```


<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->
  
<details><summary>fileReader</summary>
<p>

## Is frame class which projects cloud points into one coordination plane. Specifically to plane ZX(cloud cut).</br>
This class also takes care of the interaction during measurement(in this frame) or selection cutting line(emits to each projection frame except ZX-side way).
  
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


