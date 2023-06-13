<!-- PROJECT LOGO -->
<br />
<div align="left">
<img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/logo_black.svg#gh-light-mode-only">
<img src="https://github.com/dekdekan/lidaretto-desktop/blob/completeRefactor_change_cuts/README_images/logo_white.svg#gh-dark-mode-only">
</div>
  <h1 align="left">Creator App</h1>

**Creator is the main/default app, where project is created and modified.**
 - All other applications can be launched from this application with the appropriate button.
 - The main window is **CreatorMainWindow** and app contains multiple dialogs
<br /> More in specific sections. <br /> 

### This App Consists of:

<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>CreatorMainWindow</summary>
<p>
  
## CreatorMainWindow is the main window, where all other apps and actions can be called.

### Getting Started
<details><summary>&emsp;&emsp;  Top main .ui menu </summary>  <!--////////////////////////////////////////////////////////////////////// --></br>


1. To use the application, it is necessary to create a project at the beginning. The project can be created with UI by button **New** in the top main menu. With this button the action  **on_actionNew_triggered** is called:

```js
  void CreatorMainWindow::on_actionNew_triggered(bool checked)
```
  - 1.After calling the **on_actionNew_triggered**, a **ProjectCreationDialog** dialog is opened. In this dialog all necessary files for creating the project are selected. See section **projectcreationdialog**.
```diff
- See section projectcreationdialog
```
  - 2.The project is created based on values in loaded calibration,lidar,camera files.
```diff
- See **Project** library, where project creation and modifications are defined. The project is created and saved to .PRJ file.
```
  - 3.The created project is opened and all ui. elements are inits with the project values. 
  
<br>
  
2. The already created project can be loaded in UI by **Open** button in the top main menu.
With this button the action  **on_actionOpenProj_triggered** is called. In action the **openProject** method is called:
```js
void CreatorMainWindow::on_actionOpenProj_triggered()
```
<br>
  
3. The already created project can be loaded by **openProject** method:
   - `filename` - filename of .PRJ file  
```js
int CreatorMainWindow::openProject(QString filename)
```  
  - 1.The project is opened based on values loaded from .PRJ file.
  - 2.All ui. elements are inits with the project values.

<br>
  
4. Graphs displaying trajectory informations can be created and displayed by **Graphs** button, which calls action  **on_actionGraphs_triggered**. 
```js
void CreatorMainWindow::on_actionGraphs_triggered()
```
<br>

5. **Outputs** button opens the folder, where all the exported files are. It calls action **on_actionOpen_Output_triggered**.
```js
void CreatorMainWindow::on_actionOpen_Output_triggered()
```
<br>

6. With **Google Earth** button is called action **on_actionGoogle_Earth_triggered**, where is created KML file from trajectory. KML file can be displayed in Google Earth.
```js
void CreatorMainWindow::on_actionGoogle_Earth_triggered()
```
<br>
  
7. With **Tools** button can be some tool apps opened:
   - `Gopro Player.`- see **GoProPlayer** app
   - `360 Video Player` - see **GarminPlayer** app
   - `UAV Flight Planner` - see **FlightParameters** app
   - `Cloud Calibrator` - see **CloudCalibration** app
   - `Panorama Generator` - see **ImageGenerator** app

   - The tool actions are inits in **initAppsToButtonsAction** method.
```js
void CreatorMainWindow::initAppsToButtonsAction()
```
<br>
  
8. With **Settings** button is called action **on_actionSettings_triggered**.
```js
void CreatorMainWindow::on_actionSettings_triggered()
```
 
```diff
 In on_actionSettings_triggered the **MinMaxPrecisionDialog** is opened. In this dialog some app settings can be changed. 
- See section minmaxprecisiondialog
``` 
<br>
  
  
9. With **history** button can be displayed all project exports in right tab. It runs action **on_pushButton_OpenHistory_clicked**.
  
```js
void CreatorMainWindow::on_pushButton_OpenHistory_clicked()
```
  - Exports can be opened, delete in history tab 
 
<br>
  
  
10. With **Export** button is called action **on_actionExport_triggered**.
  
```js
void CreatorMainWindow::on_actionExport_triggered()
```
```diff
 In on_actionExport_triggered the **ExportDialog** is opened. In this dialog can be set preferences for cloud export. Everything is exported on dialog confirmation.
- See section exportdialog
``` 
<br>
  
  
  
</details>

<details><summary>&emsp;&emsp; Right .ui menu for manipulating with trajectory </summary> <!
  -/////////////////////////////////////////////// --></br>
  
1. To select/deselect whole trajectory is used **Select/Deselect All** button, which runs **on_toolButton_select_all_clicked** slot:
```js
void CreatorMainWindow::on_toolButton_select_all_clicked()
```


2. To select/deselect some part of trajectory are used **Select/Deselect by** buttons.
Change selection in **Select by**  button trigger **onSelectionButton**  slot.
   - `isModeActive` - whether selection in main menu is chosen 
   - `selectMode` - which subselection is chosen

```js
void CreatorMainWindow::onSelectionButton(bool isModeActive, int selectMode)
```
Based on "selectMode" the four selections can be done: (by hand,time,polygon,rectangle).\n
Selection button is promoted to **SelectButton class**, that inherits from **MultiModeButton class(defined in GuiComponents lib)**

<br>

Change selection in **Dselct by**  button trigger **onDeselectionButton**  slot.
   - `isModeActive` - whether deselection in main menu is chosen 
   - `selectMode` - which subdeselection is chosen

```js
void CreatorMainWindow::onDeselectionButton(bool isModeActive, int deselectMode) //exclude part
```
Based on "deselectMode" the two deselections can be done(by hand,rectangle).\n
Deselection button is promoted to **DeselectButton class**, that inherits from **MultiModeButton class(defined in GuiComponents lib)**

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
4. To display information about trajectory point defined by index, call **setInfo** on map QFrame: <br />
Information is also displayed when mouse mode is set to "PointInfoSelection" and user clickes somewhere on trajectory.

    - `info` - index of point whose information should be displayed

```js
void setInfo(int info)
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
1. To set polygon and draw it, call **setPolygon** on object of this class:
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
1. If you want to use this somewhere, first of all you have to call **createNewProject** on object of this class:
     - `projj` - reference for changing trajectory states 
```js
 void createNewProject(std::shared_ptr<std::vector<framesTrajectoryRelationsInfoStruct>> projj)
```  
  
2. Then call addNewSelection on object of this class, whenever something in the selection changes:
```js
void addNewSelection()
```

3. Then if you want to go through the history of selections, call **redo** to go to upcoming states or **undo** to go to previous states of trajectory:
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


### This library is used in:
##### 1. Creator app. 
-   qcloudaerialview, qcloudcutwindow, qsidewayview will be shown, when user selects **Profiles** from the side menu.
  
##### 2. ColorCalibrator app
##### 3. GarminPlayer app
##### 4. GlobalCloudColorizer app
##### 5. GarminPlayer app
##### 6. GoProPlayer app


