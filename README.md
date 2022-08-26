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

### cvwidget is widget class where defined image is rendered.
There is included QT class QOpenGLWidget: <a href="https://doc.qt.io/qt-6/qopenglwidget.html">Show documentation</a>, thanks to which we can display OpenGL graphics.
  
#### Getting Started
- When you want to use this widget somewhere, first of all you have to add OpenGL widget with class CQtOpenCVViewerGl to .ui file.
- Defined image in widget will be rendered, also on resizing, when you will just call on widget this function:

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
1. When you want to use this view somewhere, first of all you have to add frame with class QCloudAerialView to .ui file.
2. To show this view with painted cloud points call **addAndShowCloud** on this frame:
  
    - `inputcloud` - generated point cloud of selected frames
    - `llp1` - right centered point of selection rectangle(on right side in the direction of trajectory)
    - `llc1` - centered point of selection rectangle defined by user
    - `llp2` - left centered point of selection rectangle(on left side in the direction of trajectory)
    - `cutwidth` - value defined by user(with double spin box "Buffer" in right menu)
    - `newusedZones` - used zones

```js
void QCloudAerialView::addAndShowCloud(cloudViz inputcloud,pcl::PointXYZRGB llp1,pcl::PointXYZRGB llc1,pcl::PointXYZRGB llp2,double cutwidth,std::map<int, bool> newusedZones)
```
3. If you want to set colorization pallete call **setColorizationPallete** on this frame:</br>
  types of palletes</br>
                    - `QCloudAerialView::intenzity`</br>
                    - `QCloudAerialView::zone`</br>
                    - `QCloudAerialView::elevation`
```js
void setColorizationPallete(ColorPalette palette)
```
4. If you want to set mouse mode call **setMouseMode** on this frame:</br>
  types of mouse mode</br>
                    - `Dragging`- To move with the content in the frame</br>
                    - `Measuring`- To enable measuring in this frame</br>
                    - `sideWayPicker`- to select cutting line
```js
void setMouseMode(MouseMode newmode)
```

5. To hide cutting line call on this frame function:
```js
void hideSidewayCut()
```

6. To get visual parameters of this frame call **getVisualParams** on this frame:
  
    - `PiZoom` - actual zoom in frame
    - `PiXoff` - X position of image center(recalculates when user moves or zooms in/out)
    - `PiYoff` - Y position of image center(recalculates when user moves or zooms in/out)

```js
void getVisualParams(double &PiZoom,double &PiXoff,double &PiYoff)
```

7. To set RTKPoints call **setRtkPoints** on this frame:
  
    - `newPoints` - new RTK points
    - `lc1` - centered point of cut, defined by user
    - `lp1` - right centered point of cut(on right side of trajectory)
    - `lp2` - left centered point of cut(on left side of trajectory)
    - `widthd` - distance from cut
    - 
```js
void setRtkPoints( std::shared_ptr<std::vector<RtkPoint>> newPoints, pcl::PointXYZRGB lc1, pcl::PointXYZRGB lp1, pcl::PointXYZRGB lp2, double widthd)
```

 Or only:
  
    - `newPoints` - new RTK points
    - `lc1` - centered point of cut, defined by user
    - `lp1` - right centered point of cut(on right side of trajectory)
    - `lp2` - left centered point of cut(on left side of trajectory)
    - `widthd` - distance from cut
    - 
```js
void setRtkPoints( std::shared_ptr<std::vector<RtkPoint>> newPoints, pcl::PointXYZRGB lc1, pcl::PointXYZRGB lp1, pcl::PointXYZRGB lp2, double widthd)
```
</p>
</details>

  
<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->
  
<details><summary>qcloudcutwindow</summary>
<p>

### Is frame class which projects cloud points into one coordination plane. Specifically to plane ZX(cloud cut).</br>
This class also takes care of the interaction during measurement(in this frame) or selection cutting line(emits to each projection frame except ZX-side way).
  
#### Getting Started
- When you want to use this widget somewhere, first of all you have to add OpenGL widget with class CQtOpenCVViewerGl to .ui file.
- Then you just call only function showImage(const cv::Mat& image) on this widget, and defined image in widget will be rendered, also on resizing.
- If you want to get position on image, where was clicked, call function getImageClickPos(QPoint widgetpos).

</p>
</details>
  
<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>qsidewayview</summary>
<p>

### Is frame class which projects cloud points into one coordination plane. Specifically to plane ZX(side way).</br>
This class also takes care of the interaction during measurement(in this frame).
  
#### Getting Started
- When you want to use this widget somewhere, first of all you have to add OpenGL widget with class CQtOpenCVViewerGl to .ui file.
- Then you just call only function showImage(const cv::Mat& image) on this widget, and defined image in widget will be rendered, also on resizing.
- If you want to get position on image, where was clicked, call function getImageClickPos(QPoint widgetpos).

</p>
</details>

<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>qpolygonrubberband</summary>
<p>

### cvwidget is widget class where defined image is rendered.
There is included QT class QOpenGLWidget: <a href="https://doc.qt.io/qt-6/qopenglwidget.html">Show documentation</a>, thanks to which we can display OpenGL graphics.
  
#### Getting Started
- When you want to use this widget somewhere, first of all you have to add OpenGL widget with class CQtOpenCVViewerGl to .ui file.
- Then you just call only function showImage(const cv::Mat& image) on this widget, and defined image in widget will be rendered, also on resizing.
- If you want to get position on image, where was clicked, call function getImageClickPos(QPoint widgetpos).

</p>
</details>





<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>undoselectionstack</summary>
<p>

### cvwidget is widget class where defined image is rendered.
There is included QT class QOpenGLWidget: <a href="https://doc.qt.io/qt-6/qopenglwidget.html">Show documentation</a>, thanks to which we can display OpenGL graphics.
  
#### Getting Started
- When you want to use this widget somewhere, first of all you have to add OpenGL widget with class CQtOpenCVViewerGl to .ui file.
- Then you just call only function showImage(const cv::Mat& image) on this widget, and defined image in widget will be rendered, also on resizing.
- If you want to get position on image, where was clicked, call function getImageClickPos(QPoint widgetpos).

</p>
</details>
  
<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->


### This library is used in:
#### 1. LidarAndTrans library, specifically in globaltramsformation.h and baselidarreader.h. 
- In **globaltramsformation.h**  are used from this library info about camera model(enum cameraModel) and data info(struct CameraDataInfo).
#### 2. project.h
- In **Project** library is created object of this class which includes all calibration data about devices of actual project.
