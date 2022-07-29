<!-- PROJECT LOGO -->
<br />
<div align="left">
<img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/logo_black.svg#gh-light-mode-only">
<img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/logo_white.svg#gh-dark-mode-only">
</div>
  <h1 align="left">Libs MAPInteraction</h1>

This library is used for transformation of **poses**.<br /><br />
This library Consists of classes:
- cvwidget
- mymapcontrol
- qcloudaerial
- qcloudcutwindow
- qpolygonrubberband
- qsidewayview
- undoselectionstack

<//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>cvwidget</summary>
<p>

### cvwidget is widget class where defined image is rendered.
There is included QT class QOpenGLWidget: <a href="https://doc.qt.io/qt-6/qopenglwidget.html">Show documentation</a>, thanks to which we can display OpenGL graphics.
  
#### Getting Started
- When you want to use this widget somewhere, first of all you have to add OpenGL widget with class CQtOpenCVViewerGl to .ui file.
- Then you just call only function showImage(const cv::Mat& image) on this widget, and defined image in widget will be rendered, also on resizing.
- If you want to get position on image, where was clicked, call function getImageClickPos(QPoint widgetpos).

</p>
</details>

<//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>mymapcontrol</summary>
<p>

### cvwidget is widget class where defined image is rendered.
There is included QT class QOpenGLWidget: <a href="https://doc.qt.io/qt-6/qopenglwidget.html">Show documentation</a>, thanks to which we can display OpenGL graphics.
  
#### Getting Started
- When you want to use this widget somewhere, first of all you have to add OpenGL widget with class CQtOpenCVViewerGl to .ui file.
- Then you just call only function showImage(const cv::Mat& image) on this widget, and defined image in widget will be rendered, also on resizing.
- If you want to get position on image, where was clicked, call function getImageClickPos(QPoint widgetpos).

</p>
</details>

<//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>qcloudaerial</summary>
<p>

### cvwidget is widget class where defined image is rendered.
There is included QT class QOpenGLWidget: <a href="https://doc.qt.io/qt-6/qopenglwidget.html">Show documentation</a>, thanks to which we can display OpenGL graphics.
  
#### Getting Started
- When you want to use this widget somewhere, first of all you have to add OpenGL widget with class CQtOpenCVViewerGl to .ui file.
- Then you just call only function showImage(const cv::Mat& image) on this widget, and defined image in widget will be rendered, also on resizing.
- If you want to get position on image, where was clicked, call function getImageClickPos(QPoint widgetpos).

</p>
</details>

  
<//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->
  
<details><summary>qcloudcutwindow</summary>
<p>

### cvwidget is widget class where defined image is rendered.
There is included QT class QOpenGLWidget: <a href="https://doc.qt.io/qt-6/qopenglwidget.html">Show documentation</a>, thanks to which we can display OpenGL graphics.
  
#### Getting Started
- When you want to use this widget somewhere, first of all you have to add OpenGL widget with class CQtOpenCVViewerGl to .ui file.
- Then you just call only function showImage(const cv::Mat& image) on this widget, and defined image in widget will be rendered, also on resizing.
- If you want to get position on image, where was clicked, call function getImageClickPos(QPoint widgetpos).

</p>
</details>

  
<//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

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

  
<//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>qsidewayview</summary>
<p>

### cvwidget is widget class where defined image is rendered.
There is included QT class QOpenGLWidget: <a href="https://doc.qt.io/qt-6/qopenglwidget.html">Show documentation</a>, thanks to which we can display OpenGL graphics.
  
#### Getting Started
- When you want to use this widget somewhere, first of all you have to add OpenGL widget with class CQtOpenCVViewerGl to .ui file.
- Then you just call only function showImage(const cv::Mat& image) on this widget, and defined image in widget will be rendered, also on resizing.
- If you want to get position on image, where was clicked, call function getImageClickPos(QPoint widgetpos).

</p>
</details>

<//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

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
  
<//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
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
