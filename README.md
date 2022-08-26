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
2. To show this view with painted cloud points call **addAndShowCloud**:
  
    - `inputcloud` - generated point cloud of selected frames
    - `llp1` - right centered point of selection rectangle(on right side in the direction of trajectory)
    - `llc1` - centered point of selection rectangle defined by user
    - `llp2` - left centered point of selection rectangle(on left side in the direction of trajectory)
    - `cutwidth` - value defined by user(with double spin box "Buffer" in right menu)
    - `newusedZones` - used zones

```js
void QCloudAerialView::addAndShowCloud(cloudViz inputcloud,pcl::PointXYZRGB llp1,pcl::PointXYZRGB llc1,pcl::PointXYZRGB llp2,double cutwidth,std::map<int, bool> newusedZones)
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

  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
#### You can get, set or clear all transformation records for given device:<br />
> Example for (DEV_IMU) pose. Except of the function getTransformationIMU_VEH, where parameter gain is missing, have functions for another poses the same input parameters.
  - Get all transformation records of DEV_IMU pose for given device:
```js
Transformation getTransformationDEV_IMU(deviceCalibration device,double gain)
```

 - Set all transformation records of DEV_IMU for given device. DEV_IMU pose will be set according to new given transformation:
```js
void setTransformationDEV_IMU(deviceCalibration &device,Transformation newTransform, double gain)
```

 - Clear all transformation records of DEV_IMU for given device:
```js
void clearTransformationDEV_IMU(deviceCalibration &device)
```
 - If you want to clear all device records at once:
```c
void DevicesCalibrations::initAllValuesToZero(deviceCalibration &device)
```

#### <br /> When you want to set all records from or save these records to calibration file:<br />
  - Set all calibration records of devices from calibration file(DevicesCalibrations object has to be created):
```c
bool DevicesCalibrations::setValuesFromCalibFile(const char *filename)
```

  - When you want to save all calibration records of devices to calibration file:<br />
```c
void DevicesCalibrations::saveValuesToCalibFile(const char *filename)
```
#### <br /> Except of poses you can also get or set time offset of the synchronization between the data from the device and the trajectory, and also rotation of device:<br />
  - Get time offset:
```c
double getTimeOffset(deviceCalibration device)
```
  - Set time offset:
```c
void setTimeOffset(deviceCalibration &device,double timeoffset)
```
  - Get rotation of device:
```c
double getDeviceRotation(deviceCalibration device)
```
<p>Example of calibration file: <a href="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/20111_UAV_M300_Loket.clb">Show File</a>.
</p><br /><br />

### This library is used in:
#### 1. LidarAndTrans library, specifically in globaltramsformation.h and baselidarreader.h. 
- In **globaltramsformation.h**  are used from this library info about camera model(enum cameraModel) and data info(struct CameraDataInfo).
#### 2. project.h
- In **Project** library is created object of this class which includes all calibration data about devices of actual project.
