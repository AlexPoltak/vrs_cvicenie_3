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
  * 1.After calling the **on_actionNew_triggered**, a **ProjectCreationDialog** dialog is opened. In this dialog all necessary files for creating the project are selected. See section **projectcreationdialog**.
```diff
- See section projectcreationdialog
```
  * 2.The project is created based on values in loaded calibration,lidar,camera files.
```diff
- See **Project** library, where project creation and modifications are defined. The project is created and saved to .PRJ file.
```
  * 3.The created project is opened and all ui. elements are inits with the project values. 
  
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

2. To select/deselect some part of trajectory is used **Select/Deselect by** button. <br />
Change selection in **Select by**  button trigger **onSelectionButton**  slot.

    * `isModeActive` - whether selection in main menu is chosen
    * `selectMode` - which subselection is chosen

```js
void CreatorMainWindow::onSelectionButton(bool isModeActive, int selectMode)
```
&emsp; Based on "selectMode" the four selections can be done: (by hand,time,polygon,rectangle).<br />
&emsp; Selection button is promoted to **SelectButton class**, that inherits from **MultiModeButton class(defined in GuiComponents lib)**

<br>
  
3. To select/deselect some part of trajectory is used **Select/Deselect by** button. <br />
Change selection in **Deselect by**  button trigger **onDeselectionButton**  slot.

    * `isModeActive` - whether selection in main menu is chosen
    * `selectMode` - which subselection is chosen
  
```js
void CreatorMainWindow::onDeselectionButton(bool isModeActive, int deselectMode) //exclude part
```
&emsp;&ensp;Based on "deselectMode" the two deselections can be done(by hand,rectangle).\n
&emsp;&ensp;Deselection button is promoted to **DeselectButton class**, that inherits from **MultiModeButton class(defined in GuiComponents lib)**
<br>

4. The **Split Point** button trigger **on_toolButton_split_point_clicked** slot:<br>
  Enables/disables to add split point on trajectory, by clicking somewhere on trajectory.
```js
void CreatorMainWindow::on_toolButton_split_point_clicked()
```
  <br>

5. The **Undo** button trigger **on_toolButton_undo_clicked** slot:<br>
  Removes the last trajectory selection made. 
```js
void CreatorMainWindow::on_toolButton_split_point_clicked()
```
```diff
 For getting previous selection is used UndoSelectionStack.
- See undoselectionstack in MapInteraction lib
``` 
  <br>

6. The **Info** button trigger **on_toolButton_info_clicked** slot:<br>
  Enables/disables to show information about trajectory point(selected by clicking on the trajectory). 
```js
void CreatorMainWindow::on_toolButton_info_clicked()
```
  <br>

7. The **Profiles** button trigger **on_toolButton_profiles_clicked** slot:<br>
  ```diff
  Opens views, where profiles can be displayed. In profile mode can be done some corrections and so on.
- See section Profile mode
``` 
```js
void CreatorMainWindow::on_toolButton_profiles_clicked()
```
</details>


<details><summary>&emsp;&emsp; 
Profile mode(Projections, corrections,RTK points) </summary> <!--/////////////////////////////////////////////////////////////////////// --></br>

> __Note__ 
> Profile mode is activated by "Profiles" button in right menu 

- In default cut(ZX projection), aerial(XY projection) profiles are displayed.
- Sideway(ZY projection) is enabled with sideway button and displayed with click somewhere in aerial or cut vindow.
```diff
- Projections are managed in qcloudaerialview, qcloudcutwindow, qsidewayview classes
- See it in **MapInteraction lib**
``` 


1. With click on trajectory in profile mode is triggered a **lineCutFinished** slot: 
   - `lineCutPoint` - ndex of trajectory point, where was clicked  

```js
void CreatorMainWindow::lineCutFinished(int lineCutPoint)
```
&emsp;&emsp; - Prepares perpendicular line segment to trajectory(displayed on map) at given point, where was clicked .<br>
&emsp;&emsp; - Takes frames that corresponds to prepared line segment and generates cloud.<br>
&emsp;&emsp; - This cloud then displays to projection views(cut and aerial view)

  <br>

2. Sideway(ZY projection) is enabled with sideway button and displayed with click somewhere in aerial or cut vindow. Click trigger slot **sideWayCutSelected**:
   - `cx` - X position of cutting line center
   - `cy` - Y position of cutting line center  
   - `rx` - direction vector of cutting line 
   - `ry` - direction vector of cutting line

```js
void CreatorMainWindow::sideWayCutSelected(double cx, double cy, double rx, double ry)
```
&emsp;&emsp; - Slot that prepares and show cloud in sideway view.

  <br>
  
3. Width, length of cutting line can be changed in right menu and confirm with **Recalc** button:
   - `retainZoomAndOffsets` - whether zoom and offset should be retained

```js
void CreatorMainWindow::recalcProfile(bool retainZoomAndOffsets)
```
&emsp;&emsp; - Method **recalcProfile** recalculates profiles based on values of cutting line and used zones

  <br>

4. Measurements can be enabled with measurement button and done with clicks in profile windows.
  Measurement button trigger slot **on_pushButton_measureProfiles_clicked**, which enables measurements in profile windows.

```js
void CreatorMainWindow::on_pushButton_measureProfiles_clicked()
```
  <br>

 5. After measurements can be created corrections of cloud. Correction can be created with FIT button after measurement is done. FIT button trigger slot **on_pushButton_FitProfiles_clicked**.

```js
void CreatorMainWindow::on_pushButton_FitProfiles_clicked()
```
&emsp;&emsp; - Firstly opens dialog with list of zones that corresponds to cutting line and waits for selection
and confirmation of a some zone(correction will be made for this zone).<br>
&emsp;&emsp; - Based on measurements prepares and makes correction of chosen zone.
  
> __Note__ 
> Corrections can be edited in FIT tab.

  <br>
  
 6. RTK points can be add in GCP tab with **Add Points** button.
```js
void CreatorMainWindow::on_pushButton_addRTKPoints_clicked()
```
&emsp;&emsp; - Opens file dialog to load RTK point from file.<br>
&emsp;&emsp; - After loading of RTK points file the **addrtkpointsdialog**  dialog is opened to manage and add this points.
```diff
- See section addrtkpointsdialog
```  
&emsp;&emsp; - On confirmation of dialog adds RTK points to project.

> __Note__ 
> RTK points can be edited in GCP tab.

  <br>

 7. Range filter can be created with button next to range filter dropbox in right menu. This button trigger slot 
**on_pushButton_profiles_rangeFilters_clicked**
```js
void CreatorMainWindow::on_pushButton_profiles_rangeFilters_clicked()
```
&emsp;&emsp; - 1.Firstly the **ChooseFromExistingFiltersDialog** dialog is opened to choose existing range filter, or select to create new one.<br>

&emsp;&emsp; - 2.Opens **LidarFilterDialog** dialog to define ranges for filter.<br>

&emsp;&emsp; - 3.On confirmation the new range filter is created to project.<br>
  <br>

</details>  

---

</p>
</details>






<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>projectcreationdialog</summary>
<p>

## This dialog shows in creator app when "New" project button is pressed. Dialog serves to manage creation of new project.
  
### Getting Started
1. In this dialog should be set files needed for project creation.(Trajectory,lidar,calibration and optional camera file).<br>
 After confirmation of this dialog the dialog **saveprojectdialog** displays, where name,description and output path to new project can be entered. On confirmation the **createProjectDataAndSave** method ic called:
```js
void ProjectCreationDialog::createProjectDataAndSave()
```
  &emsp;&emsp; - Method **createProjectDataAndSave** starts thread to generate new project based on trajectory,lidar,calibration,camera files, that were chosen in .ui. The project is opened after creation. <br>

 <br>

---

</p>
</details>
  
  
  
  
  
  
<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>minmaxprecisiondialog</summary>
<p>

##  Class that serves to manage some basic settings .<br>
   - This dialog shows in creator app when "Settings" button is pressed.
   - In this dialog can be set language, quality indicator, usage of some filters and more.
  
### Getting Started
1. In settings can be also changed appearance(font size,dark/light mode)
   - Dark style is defined in **:/qdarkstyle/light/darkstyle.qss** file
   - Light style is defined in **:/qdarkstyle/light/lightstyle.qss** file
  

> __Note__ 
> In qss files are defined styles for each ui element. The right way to write qss and all properties you can see in this link https://doc.qt.io/qt-6/stylesheet-reference.html

 <br>

---

</p>
</details>


<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>exportdialog</summary>
<p>

##  Class serves to display settings of export before export.\n
   - This dialog shows in creator app when "export" button is pressed.\n
   - In this dialog can be set colorization of exported cloud, some filters, usage of FIT corrections and many more.
  
### Getting Started
1. To set project that should be exported use method **setWorkingProject** on object:
```js
void ExportDialog::setWorkingProject(std::shared_ptr<Project> newproject)
```
&emsp;&emsp; - Inits all .ui elements with project settings<br>
  
  
2. In dialog can be set datum for export. Button to manage datums is next to Coordinate System dropdown, where the datum can be selected.
```diff
- See section transformationpickerdialog
``` 

  
3. In dialog can be set range filter for export. Button to manage range filters is next to Range Filter dropdown, where the range filter can be selected.
  
&emsp;&emsp; - 1.Firstly the **ChooseFromExistingFiltersDialog** dialog is opened to choose existing range filter, or select to create new one.<br>
```diff
- See section choosefromexistingfiltersdialog
``` 
&emsp;&emsp; - 2.Opens **LidarFilterDialog** dialog to define ranges for filter.<br>
```diff
- See section lidarfilterdialog
``` 
  
 4. In dialog can be set image filters for export. Button to manage image filters is next to Image Filter dropdown, where the image filter can be selected.
  
&emsp;&emsp; - **imageRestrictionSettingsDialog** is shown after the button is pressed 
```diff
- See section imagerestrictionsettingsdialog
``` 

5. Clicks on **Advanced Filtering** button displays **simpleoctomapfiltersettingsdialog**, where some others settings for export can be defined.

  
> __Note__ 
> On confirmation the export settings are saved to registry


 <br>

---

</p>
</details>



<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->

<details><summary>imagerestrictionsettingsdialog</summary>
<p>

## In this dialog can be defined image filters
  
### Getting Started
1. New filter can be add with + button.
```js
void imageRestrictionSettingsDialog::on_pushButton_add_filter_clicked()
```
   <br>

2. Filters can be also imported from saved file.
```js
void imageRestrictionSettingsDialog::on_pushButton_import_clicked()
```
   <br>

3. Right click on filter trigger **showContextMenu** slot that shows menu
```js
void imageRestrictionSettingsDialog::showContextMenu(const QPoint &pos)
```
&emsp;&emsp; - In menu the filter can be deleted or new filter zone can be added.
  
 <br>
  
4. Right click on filter zone trigger **deleteZone** slot that deletes zone
```js
void imageRestrictionSettingsDialog::deleteZone(std::vector<QTreeWidgetItem*> fitID)
```  
 <br>
  
5. Filter zone can be add by right click on filter and choose **Add Zone** from menu.
  - Then the zones can be selected in video view
  - The zones can be saved to registry with **save** button which will show.
 <br>
  
6. Image filter can be exported to file with **export** button, that trigger **on_pushButton_export_clicked** slot.
```js
void imageRestrictionSettingsDialog::on_pushButton_export_clicked()
```  
&emsp;&emsp; - Filters are saved to defined XML file with suffix .IRF

  
  
---

</p>
</details>
  
  
  
  
<!-- //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// -->
  
<details><summary>licensedialog</summary>
<p>

## This dialog serves to control user license before starting the creator. It is used iin main.cpp
  Firstly user need to send us a code based on which we can generate serial key. Based on serial key user can activate creator. 
> __Note__ 
> Example of final code, that is send from customer to us, can be **1024228818019aw3CGkj1tO961Ll**
  
### Code is created with some steps:
  - 1.Firstly we generate some random 16chars string to registry, for example **9aw3CGkj1tO961Ll**
  - 2.Getting CPUID of user PC. CPU ID example is **906A3**.
  - 3.Appending some random hex chars to have 8 chars(seed) along with CPUID. Example **906A3E09**
  - 4.We change this seed to decimal values. Decimal values from step 3. for example is **2422881801**
  - 5.The final code that is send to us has at the beginning length of seed to recover hexadecimal values from decimal when we will generate serial code. The first 2 chars defines length. In example code **~~10~~24228818019aw3CGkj1tO961Ll** , **10** is the length of seed. When length would be **9** at the beginning would be **09**.
  - 6.The next chars(with defined length in step 5., specifically **10**) in code  are seed in decimal form from step 4. **10~~2422881801~~9aw3CGkj1tO961Ll**
  - 7.The random code from step 1. is attached at the end of final code. **102422881801~~9aw3CGkj1tO961Ll~~**
  
  
  ### The creation of serial, to send customer for activation, is done in **licenseCreator** app. Serial code is generated based on code described above.


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

