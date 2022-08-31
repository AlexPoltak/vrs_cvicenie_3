<!-- PROJECT LOGO -->
<br />
<div align="left">
<img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/logo_black.svg#gh-light-mode-only">
<img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/logo_white.svg#gh-dark-mode-only">
</div>
  <h1 align="left">Libs MAPInteraction</h1>

**This library is used to display and interact with the map.**<br />
More in specific sections. <br /><br />
This library Consists of:

<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>cvwidget</summary>
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

<details><summary>mymapcontrol</summary>
<p>
  
## mymapcontrol is used to interact with the map. This class is part of an open-source cross-platform map widget QMapControl. 
  - QMapControl Contact e-mail: kaiwinter@gmx.de
  - QMapControl github: https://github.com/kaiwinter/QMapControl
  - Changes were made by Martin Dekan for the purpose of trajectory selection</br>
  
 QMapControl is implemented in external libs of lidaretto project.


### Getting Started
<details><summary>&emsp;&emsp; Needed steps to show map </summary>  <!--////////////////////////////////////////////////////////////////////// --></br>

1. To use this map control, first of all you have to add some container with QFrame class to .ui file.
2. Then promote this QFrame to class **MyMapControl**.
3. Add this <a href="https://github.com/alexpoltak/vrs_cvicenie_3/blob/main/documents/Includes.txt">Includes</a> to .pro file of app.
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
8. To add layer created in step 6, or another layer, to layers of map call **addLayer** on map QFrame:
```js
void addLayer(Layer* layer)
```
</details>

<details><summary>&emsp;&emsp; Methods for manipulation with map position, size</summary> <!--/////////////////////////////////////////////// --></br>
  
1. To set the middle of the map to the given coordinate, call **setView** on map QFrame:
```js
void setView(const QPointF& coordinate)
```
2. To Keep the center of the map on the Geometry, even when it moves, call **followGeometry** on map QFrame:
```js
void followGeometry ( const Geometry* geometry )
```
3. If the view is set to follow a Geometry this method stops the trace:
```js
void stopFollowing ( const Geometry* geometry )
```
4. To move smoothly the center of the view to the given Coordinate, call **moveTo** on map QFrame:
```js
oid moveTo	( QPointF coordinate )
```
5. To get the coordinate of the center of the map, call **currentCoordinate** on map QFrame:
```js
QPointF	currentCoordinate()
```
6. To scroll the view to the left, call **scrollLeft** on map QFrame:
```js
void scrollLeft ( int pixel)
```
7. To scroll the view to the right, call **scrollRight** on map QFrame:
```js
void scrollRight ( int pixel)
```
8. To scroll the view up, call **scrollUp** on map QFrame:
```js
void scrollUp ( int pixel)
```
9. To scroll the view down, call **scrollDown** on map QFrame:
```js
void scrollDown ( int pixel)
```
10. To scroll the view by the given point, call **scroll** on map QFrame:
```js
void scroll ( const QPoint scroll )
```

11. To resize the map to the given size, call **resize** on map QFrame:
```js
void resize(const QSize newSize)
```
</details>


<details><summary>&emsp;&emsp; 
Methods for displaying add-ons on the map </summary> <!--/////////////////////////////////////////////////////////////////////// --></br>
1. To set another cursor, call **setCursorFromList** with "true" in input on map QFrame: 

| name of cursor                  | image of cursor                                                       |   
| :-------------                  | :-------------                                                        |
| default_hand_open               | <p align="center"><img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/cursor/default_hand_open.png"></p>                | 
| default_hand_closed             | <p align="center"><img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/cursor/default_hand_closed.png"></p>              | 
| circle_selecting_hand_open      | <p align="center"><img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/cursor/selecting_hand_open.png"></p>              | 
| circle_selecting_hand_closed    | <p align="center"><img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/cursor/selecting_hand_closed.png"></p>            | 
| circle_selecting_notSelecting   | <p align="center"><img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/cursor/selecting_noselect.png"></p>               |  
| circle_selecting_selecting      | <p align="center"><img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/cursor/selecting.png"></p>                        | 
| circle_selecting_polygon        | <p align="center"><img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/cursor/polygonSelecting.png"></p>                 | 
| circle_selecting_rectangle      | <p align="center"><img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/cursor/rectangleSelect.png"></p>                  | 
| circle_selecting_time           | <p align="center"><img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/cursor/timeSelection.png"></p>                    | 
| circle_deselecting_hand_open    | <p align="center"><img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/cursor/deselecting_hand_open.png"></p>            | 
| circle_deselecting_hand_closed  | <p align="center"><img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/cursor/deselecting_hand_closed.png"></p>          | 
| circle_deselecting_notSelecting | <p align="center"><img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/cursor/deselecting_noselect.png"></p>             | 
| circle_deselecting_selecting    | <p align="center"><img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/cursor/deselecting.png"></p>                      | 
| circle_deselecting_rectangle    | <p align="center"><img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/cursor/rectangleDeSelect.png"></p>                | 
| adding_splitpoint_hand_open     | <p align="center"><img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/cursor/splitpoint_hand_open.png"></p>             | 
| adding_splitpoint_hand_closed   | <p align="center"><img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/cursor/splitpoint_hand_closed.png"></p>           | 
| adding_splitpoint_adding        | <p align="center"><img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/cursor/SplitPoint.png"></p>                       | 
| info_point_select               | <p align="center"><img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/cursor/basic.png"></p>                            |

```js
void setCursorFromList(int index)
```

2. To display the scale within the widget, call **showScale** with "true" in input on map QFrame: 
```js
void showScale ( bool visible )
```
3. To display crosshairs, call **showCrosshairs** with "true" in input on map QFrame:
```js
void showCrosshairs ( bool visible )
```
4. To display information about trajectory point defined by index, call **setInfo** on map QFrame: 
information is also displayed when mouse mode is set to "PointInfoSelection" and user clickes somewhere on trajectory.

    - `info` - index of point whose information should be displayed

```js
void setInfo(int info)
```

  
</details>  



<details><summary>&emsp;&emsp; Methods of selecting trajectory points </summary> <!--/////////////////////////////////////////////////////////////////////// --></br>

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


2. To select all points among defined points, call **selectInTrajectory** on map QFrame: 

    - `start` - index of start point
    - `goal` - index of end point

```js
void selectInTrajectory(int fromPoint,int toPoint)
```
3. To deselect all points among defined points, call **deselectInTrajectory** on map QFrame: 

    - `start` - index of start point
    - `goal` - index of end point

```js
void deselectInTrajectory(int fromPoint,int toPoint)
```
4.  To select or deselect all points among defined points, based on mouse mode, call **doWithTrajectoryBetweenPoints** on map QFrame: 
    - `mouse mode is Selecting ` - selectInTrajectory is called
    - `mouse mode is Deselecting` - deselectInTrajectory is called
```js
void MyMapControl::doWithTrajectoryBetweenPoints(int lastIndex,int newindex)
```

5. To select all prepared points(to change points states from "during selection" to "selected"), call **SelectPreparedPoints** on map QFrame: 
```js
void SelectPreparedPoints()
```
6. To deselect all prepared points(to change points states from "during selection" to "not selected"), call **DeselectPreparedPoints** on map QFrame: 
```js
void DeselectPreparedPoints()
```

7. To select all points of trajectory(to change points states to "selected"), call **selectWholeTrajectory** on map QFrame: 
```js
void selectWholeTrajectory()
```
8. To deselect all points of trajectory(to change points states to "not selected"), call **deselectWholeTrajectory** on map QFrame: 
```js
void deselectWholeTrajectory()
```
 
</details>  

<details><summary>&emsp;&emsp; Methods for checking whether the points are in the defined area</summary> <!--/////////////////////////////////////////////////////////////////////// --></br>

1. To find points that are in the selection rectangle and changes their state from 0-"not selected" to 1-"during selection" state, call **checkColisionWithRectangle** on map QFrame: 

```js
void checkColisionWithRectangle()
```

2. To points that are in the deselection rectangle and changes their state from 2-"selected" to 1-"during selection" state, call **checkDeColisionWithRectangle** on map QFrame: 

```js
void checkDeColisionWithRectangle()
```

3. To find points that are in the selection polygon and changes their state from 0-"not selected" to 1-"during selection" state, call **checkColisionWithPolygon** on map QFrame: 

```js
void checkColisionWithPolygon()
```
4. To find out if some trajectory point is in defined circle and get its index, call **checkColisionWithCircle** on map QFrame: 

    - `center` - center of area
    - `radius` - radius of area
    - `previousIndexOfInterest` 
 
| previousIndexOfInterest condition        | Description                                           |   
| :-------------                           | :-------------                                        |
| When (previousIndexOfInterest is -1      | checks if some trajectory point is in defined area    | 
| When (previousIndexOfInterest is not -1) | checks if some trajectory point is in defined area and whether is close to the previous one point (at previousIndexOfInterest)                                                                           | 

```js
int MyMapControl::checkColisionWithCircle(QPoint center,double radius,int previousIndexOfInterest)
```

5. To find out if some split point is in defined area and get its index, call **checkSplitpointAroundPoint** on map QFrame: 
```js
int MyMapControl::checkSplitpointAroundPoint(int indexintraj,int areaofinterest)
```
6. To find out if some RTK point is in defined area and get its index, call **checkColisionWithRtkPoint** on map QFrame:

    - `center` - center of area
    - `radius` - radius of area
```js
int MyMapControl::checkColisionWithRtkPoint(QPoint center,double radius)
```




7. To find points that are not "selected" among the defined indexes and change their state to "during selection", call **CheckPointsBetweenPoints** on map QFrame: 

    - `start` - index of start point
    - `goal` - index of end point
    
```js
void MyMapControl::CheckPointsBetweenPoints(int start, int goal)
```

</details>  



<details><summary>&emsp;&emsp; Methods for manipulation with map layers </summary> <!--/////////////////////////////////////////////////////////////////////// --></br>
If multiple layers are added, they are painted in the added order.

1. To add new layer, call **addLayer** on map QFrame: 
```js
void addLayer ( Layer* layer )
```
2. To remove layer, call **removeLayer** on map QFrame: 
```js
void removeLayer ( Layer* layer )
```
3. To get layer by given name, call **layer** on map QFrame: 
```js
Layer* layer ( const QString& layername )
```
4. To get names of all layers, call **layers** on map QFrame: 
```js
QList<QString> layers()
```
5. To get number of layers, call **numberOfLayers** on map QFrame: 
```js
int numberOfLayers()
```
6. To set the center of the view to the center point of the trajectory and also set the zoom to maximum to display the entire trajectory, call **setCenterAndMaxZoomForProject** on map QFrame: 
```js
void setCenterAndMaxZoomForProject()
```
  
</details>  

<details><summary>&emsp;&emsp; Methods for manipulation with zoom </summary> <!--/////////////////////////////////////////////////////////////////////// --></br>

1. To set zoom limit, call **setImageZoomLimit** on map QFrame: 
```js
void setImageZoomLimit(int newLimit)
```
2. To get zoom limit, call **getImageZoomLimit** on map QFrame: 
```js
int getImageZoomLimit()
```
3. To set current zoom, call **setZoom** on map QFrame: 
```js
void setZoom ( int zoomlevel )
```
4. To zoom in one step, call **zoomIn** on map QFrame: 
```js
void zoomIn()
```
5. To zoom out in one step, call **zoomOut** on map QFrame: 
```js
void zoomOut()
```
6. To set the center of the view to the center point of the trajectory and also set the zoom to maximum to display the entire trajectory, call **setCenterAndMaxZoomForProject** on map QFrame: 
```js
void setCenterAndMaxZoomForProject()
```
  
</details>  


---

</p>
</details>

<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>qcloudaerialview</summary>
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

3. If you want to set colorization pallete, call **setColorizationPallete** on this frame:</br>
  types of palletes</br>
                    - `QCloudAerialView::intenzity`</br>
                    - `QCloudAerialView::zone`</br>
                    - `QCloudAerialView::elevation`
```js
void setColorizationPallete(ColorPalette palette)
```

4. If you want to set mouse mode, call **setMouseMode** on this frame:</br>
  types of mouse mode</br>
                    - `Dragging`- To move with the content in the frame</br>
                    - `Measuring`- To enable measuring in this frame</br>
                    - `sideWayPicker`- to select cutting line
```js
void setMouseMode(MouseMode newmode)
```

5. To get mouse mode, call **getMouseMode** on this frame:</br>
```js
MouseMode getMouseMode()
```

6. To set parameters and enable cutting line painting, call **setSidewayCutParams** on this frame:
  
    - `cx` - X position of center
    - `cy` - Y position of center
    - `rx` - direction vector
    - `ry` - direction vector


```js
void setSidewayCutParams(double cx,double cy,double rx,double ry)
```

7. To hide cutting line, call on this frame function:
```js
void hideSidewayCut()
```

8. To set visual parameters of this frame, call **setVisualParams** on this frame:
  
    - `PiZoom` - actual zoom in frame
    - `PiXoff` - X position of image center(recalculates when user moves or zooms in/out)
    - `PiYoff` - Y position of image center(recalculates when user moves or zooms in/out)

```js
void setVisualParams(double PiZoom,double PiXoff,double PiYoff)
```

9. To get visual parameters of this frame, call **getVisualParams** on this frame:
  
    - `PiZoom` - actual zoom in frame
    - `PiXoff` - X position of image center(recalculates when user moves or zooms in/out)
    - `PiYoff` - Y position of image center(recalculates when user moves or zooms in/out)

```js
void getVisualParams(double &PiZoom,double &PiXoff,double &PiYoff)
```

10. To set RTKPoints, call **setRtkPoints** on this frame:
-
    - `newPoints` - new RTK points
    - `lc1` - centered point of cut, defined by user
    - `lp1` - right centered point of cut(on right side of trajectory)
    - `lp2` - left centered point of cut(on left side of trajectory)
    - `widthd` - distance from cut
    - 
```js
void setRtkPoints( std::shared_ptr<std::vector<RtkPoint>> newPoints, pcl::PointXYZRGB lc1, pcl::PointXYZRGB lp1, pcl::PointXYZRGB lp2, double widthd)
```

- Or only:
  
    - `newPoints` - new RTK points
    - `widthd` - distance from cut
```js
void setRtkPoints( std::shared_ptr<std::vector<RtkPoint>> newPoints,double widthd)
```

11. To set used zones, call **setUsedZones** on this frame:

```js
void setUsedZones(std::map<int, bool> newusedZones)
```

---

</p>
</details>
  
<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->
  
<details><summary>qcloudcutwindow</summary>
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

3. To set parameters and enable cutting line painting, call **setSidewayCutParams** on this frame:
  
    - `cx` - X position of center
    - `cy` - Y position of center

```js
void setSidewayCutParams(double cx,double cy)
```

4. To get distance of two points, selected by user in Measuring mode, call **getDists** on this frame:
  
    - `x` - distance in x axis
    - `y` - distance in y axis

```js
void getDists(double &x,double &y )
```
All this methods are same like in qcloudaerialview: 
  - setColorizationPallete
  - setMouseMode
  - getMouseMode
  - setVisualParams
  - getVisualParams
  - setRtkPoints
  - setUsedZones

---

</p>
</details>
  
<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>qsidewayview</summary>
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

3. To clear cloud and measured distances from view, call **removeCloud** on this frame:
```js
void removeCloud()
```

4. To get distance of two points, selected by user in Measuring mode, call **getDists** on this frame:
  
    - `x` - distance in x axis
    - `y` - distance in y axis

```js
void getDists(double &x,double &y )
```
All this methods are same like in qcloudaerialview: 
  - setColorizationPallete
  - setMouseMode
  - getMouseMode
  - setVisualParams
  - getVisualParams
  - setRtkPoints
  - setUsedZones
  - hideSidewayCut

---

</p>
</details>

<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>qpolygonrubberband</summary>
<p>

## qpolygonrubberband is class for painting polygon.

### Getting Started
1. To set polygon which should be drawn call **setPolygon** on object of this class:
```js
void setPolygon(std::vector<QPoint> polygonPoints)
```
&emsp;&emsp;Or :
```js
void setPolygon(std::vector<QPoint> polygonPoints,QPoint lastPoint)
```
  
2. To change color of polygon  call **changeColor** on object of this class:
```js
void changeColor(QColor newcolor)
```
  
---  
  
</p>
</details>


<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>undoselectionstack</summary>
<p>

## undoselectionstack is the class which holds history of selections, so you can go through this history.

### Getting Started
1. When you want to use this somewhere, first of all you have call **createNewProject** on object of this class:
     - `projj` - reference for changing states of trajectory
```js
 void UndoSelectionStack::createNewProject(std::shared_ptr<std::vector<framesTrajectoryRelationsInfoStruct>> projj)
```  
  
2. Then call addNewSelection on object of this class, whenever something in the selection changes:
```js
void addNewSelection()
```

3. Then if you want to go through the history of selections, call **redo** to go to upcoming states or **undo** to go to previous states:
```js
void redo()
```
```js
void undo()
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


