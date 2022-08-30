<!-- PROJECT LOGO -->
<br />
<div align="left">
<img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/logo_black.svg#gh-light-mode-only">
<img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/logo_white.svg#gh-dark-mode-only">
</div>
  <h1 align="left">Libs MAPInteraction</h1>

**This library is used to interact with the map.**<br />
qcloudaerialview, qcloudcutwindow, qsidewayview are frames which will be shown, when user selects **Profiles** from the side menu. More in specific sections. <br /><br />
This library Consists of:

<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>cvwidget</summary>
<p>

### cvwidget is widget class where defined image is rendered.
There is included QT class QOpenGLWidget: <a href="https://doc.qt.io/qt-6/qopenglwidget.html">Show documentation</a>, thanks to which we can display OpenGL graphics.
  
#### Getting Started
1. When you want to use this widget somewhere, first of all you have to add widget with class **CQtOpenCVViewerGl** to .ui file.
  
2. Then you just call only function **showImage** on this widget, and defined image in widget will be rendered, also on resizing. If image shows properly this funtcion **return true**, else **return false**. Function **showImage**:
```js
bool CQtOpenCVViewerGl::showImage(const cv::Mat& image)
```
3. If you want to get position on image, where user clicked:  (parameter widgetpos is position of widget from global)
 ```js
QPoint CQtOpenCVViewerGl::getImageClickPos(QPoint widgetpos)
``` 
4. If you want to get position of point, which should be at the same position on image, when widget is resized:
 ```js
QPoint CQtOpenCVViewerGl::getImagePosToWidgetPos(QPoint imagepos)
``` 
</p>
</details>

<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>mymapcontrol</summary>
<p>

### mymapcontrol is used to interact with the map. This class is part of an open-source cross-platform map widget QMapControl. 
  - Contact e-mail: kaiwinter@gmx.de
  - Changes were made by Martin Dekan for the purpose of trajectory selection
  - github: https://github.com/kaiwinter/QMapControl</br>
  
 QMapControl is implemented in external libs of lidaretto project.

  
#### Getting Started
###needed steps to show map
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
7. Call **__init()** on map QFrame (created in 1. and 2. step) to initialize all needed values.
8. To add layer created in step 6, or another layer, to layers of map call **addLayer** on map QFrame:
```js
void MyMapControl::addLayer(Layer* layer)
```


- Then you just call only function showImage(const cv::Mat& image) on this widget, and defined image in widget will be rendered, also on resizing.
- If you want to get position on image, where was clicked, call function getImageClickPos(QPoint widgetpos).

</p>
</details>

<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>qcloudaerialview</summary>
<p>

### Is frame class which projects cloud points into one coordination plane. Specifically to the plane XY(aerial).</br>
This class also takes care of the interaction during measurement(in this frame) or selection cutting line(emits to each projection frame except ZX-side way).
  
#### Getting Started
1. When you want to use this view somewhere, first of all you have to add frame with class **QCloudAerialView** to .ui file.

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

</p>
</details>

  
<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->
  
<details><summary>qcloudcutwindow</summary>
<p>

### Is frame class which projects cloud points into one coordination plane. Specifically to plane ZX(cloud cut).</br>
This class also takes care of the interaction during measurement(in this frame) or selection cutting line(emits to each projection frame except ZX-side way).
  
#### Getting Started
1. When you want to use this view somewhere, first of all you have to add frame with class **qcloudcutwindow** to .ui file.

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
void qcloudcutwindow::getDists(double &x,double &y )
```
All this methods are same like in qcloudaerialview: 
  - setColorizationPallete
  - setMouseMode
  - getMouseMode
  - setVisualParams
  - getVisualParams
  - setRtkPoints
  - setUsedZones

</p>
</details>
  
<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>qsidewayview</summary>
<p>

### Is frame class which projects cloud points into one coordination plane. Specifically to plane ZX(side way).</br>
This class also takes care of the interaction during measurement(in this frame).
  
#### Getting Started
1. When you want to use this view somewhere, first of all you have to add frame with class **qcloudcutwindow** to .ui file.

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
void qcloudcutwindow::getDists(double &x,double &y )
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

</p>
</details>

<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>qpolygonrubberband</summary>
<p>

### qpolygonrubberband is class for painting polygon.

#### Getting Started
1. To set polygon which should be drawn call **setPolygon** on object of this class:
```js
void QPolygonRubberBand::setPolygon(std::vector<QPoint> polygonPoints)
```
&emsp;&emsp;Or :
```js
void QPolygonRubberBand::setPolygon(std::vector<QPoint> polygonPoints,QPoint lastPoint)
```
  
2. To change color of polygon  call **changeColor** on object of this class:
```js
void changeColor(QColor newcolor)
```
</p>
</details>





<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>undoselectionstack</summary>
<p>

### undoselectionstack is the class which holds history of selections, so you can go through this history.

#### Getting Started
1. When you want to use this somewhere, first of all you have call **createNewProject** on object of this class:
     - `projj` - reference for changing states of trajectory
```js
 void UndoSelectionStack::createNewProject(std::shared_ptr<std::vector<framesTrajectoryRelationsInfoStruct>> projj)
```  
  
2. Then call addNewSelection on object of this class, whenever something in the selection changes:
```js
void UndoSelectionStack::addNewSelection()
```

3. Then if you want to go through the history of selections, call **redo** to go to upcoming states or **undo** to go to previous states:
```js
void UndoSelectionStack::redo()
```
```js
void UndoSelectionStack::undo()
```
</p>
</details>
 
<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->


### This library is used in:
#### 1. LidarAndTrans library, specifically in globaltramsformation.h and baselidarreader.h. 
- In **globaltramsformation.h**  are used from this library info about camera model(enum cameraModel) and data info(struct CameraDataInfo).
#### 2. project.h
- In **Project** library is created object of this class which includes all calibration data about devices of actual project.
