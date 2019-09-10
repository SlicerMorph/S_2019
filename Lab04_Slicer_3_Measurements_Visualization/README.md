# Lab 04 Slicer#3: Measurements (angles, lines, 3D curves, landmarks) and Visualization 
## Overview of markups
* Stable version of Slicer only supports fiducial markups 
* Preview version adds: lines, angles, open and closed curves (planes will be added to the list in the upcoming months)
<img src="./images/MarkupWidgets.png">

* Using the preview version is strongly recommended due to the additional markups types and improvements to the fiducial representation  allowing better performance with large numbers of markups and easier user interaction
* Updates to Markups module are ongoing so check back for updates
## Markup Types
**ROI:**
Place two points sequentially that specify corners of a rectangular cube defining the region of interest. The size and shape of the rectangle can be adjusted after placement.

**Fiducials:**
Place a single landmark point.

**Lines:**
Sequentially place two points, creating a line between them.

**Angles:**
Place three points sequentially. This forms two vectors where the second point placed is the vertex. The angle beween the two vectors is displayed.

**Open and closed curves:**
Sequentially place points. A curve will be fit to the points and updated as additional points are added. If the closed curve is selected, the first and last points placed will be connected.

<img src="./images/MarkupTypes.png">

## Markup Placement
  * Slicer has two mouse modes: Transform, and Place. 
  * Transform mode is the default interaction mode. This mode allows interaction with loaded data (pan, zoom, rotate)
  * The icons in the mouse mode toolbar at the top of the main GUI allow to switch between these modes
  * Place mode allows to place one object then switches modes back to Transform mode. Fiducial is the default object.
  * Place mode can be made persistent by clicking the checkbox in the mouse mode toolbar.
<img src="./images/FiducialPersistence.png">

## Markup Management
Fiducial points and anchor points of lines and curves can be accessed and manipulated using the Markups Module. 
<img src="./images/markupsModule.png">
* In the display menu, set the visibility, opacity, glyph and text size of a markup node
* In the markups table, adjust visibility, labels, and position of individual fiducials or anchor points

## Example: Using Markups for Measurement
In this example, we will place a closed curve on one slice of a CT scan, measure the area of the curve, and visualize the region. For more detail and discussion, see the Slicer discourse thread [here](https://discourse.slicer.org/t/how-can-i-calculate-an-area-on-a-ct-image-i-can-calculate-volumes-mm-3-but-not-areas-mm-2/1549/7).

1. Select the Sample Data module and load the MRIHead volume. 
 <img src="./images/sampleData.png">

2. Select the closed curve markup mode and place a curve around the brain tissue in the red view window (axial slice). You can change the Slicer layout to red window only for better detail.
<img src="./images/CurveOnRed.png">

3. Open the Python Interactor and paste the following snippet of code to calculate the area of the curve:
```
curve=slicer.util.getNodesByClass("vtkMRMLMarkupsClosedCurveNode")[0]
crossSectionSurface = vtk.vtkPolyData()
areaMm2 = slicer.modules.markups.logic().GetClosedCurveSurfaceArea(curve, crossSectionSurface)
print("Curve {0}: surface area = {1:.2f} mm2".format(curve.GetName(), areaMm2))
```
<img src="./images/pythonInteract.png">


4. If you switched to the red slice view layout in the Slicer application, switch back to the conventional layout. To visualize the area of the curve, type the following code snippet in the Python Interactor:
```
crossSectionSurfaceModel = slicer.modules.models.logic().AddModel(crossSectionSurface)
crossSectionSurfaceModel.SetName("{0} surface".format(curve.GetName()))
crossSectionSurfaceModel.CreateDefaultDisplayNodes()
crossSectionSurfaceModel.GetDisplayNode().BackfaceCullingOff()
crossSectionSurfaceModel.GetDisplayNode().SetColor(curve.GetDisplayNode().GetColor())
crossSectionSurfaceModel.GetDisplayNode().SetOpacity(0.5)
crossSectionSurfaceModel.SetDescription("Area[mm2] = {0:.2f}".format(areaMm2))
```
 <img src="./images/VisualizingCurveArea.png">

## Volume Rendering
The Volume Rendering module provides interactive visualization of 3D image data. For full documentation of the panel and functions, see [here](https://www.slicer.org/wiki/Documentation/Nightly/Modules/VolumeRendering#Panels_and_their_use).
* Only scalar volumes can be used for volume rendering. Vector volumes (eg jpg, png, bmp, or other classic 2D formats) can be converted to scalar volumes using the [VectorToScalarVolume module](https://www.slicer.org/wiki/Documentation/Nightly/Modules/VectorToScalarVolume).
* 3D Slicer uses volume ray casting to computes 2D images from 3D volumetric data sets. Unlike surface reconstruction, there is no estimation of object surfaces or segmentation.
* The values displayed are calculated using a transfer function that incorporates voxel intensities, material properties, and illumination.
* The opacity and color of the image can be adjusted by modifying their transfer functions in the Volume Rendering module.

 <img src="./images/volumeRenderTF.png">
 
* Slicer supports both CPU and GPU volume rendering. CPU based will always work, whether you are on a computer without a dedicated graphics card, or on a remote connection (which may not support hardware accelerated graphics), but it is slow. GPU requires you have a dedicated graphics card with 1GB or more videoRAM, but it is much faster. 
* If you have a dedicated graphics card, you may want to set the default visualization method to GPU rendering using the menu option in: Edit->Preferences 
* Always set the rendering quality to normal 
* The physical limits to the size of the volumes that can be rendered are determined by the graphics card RAM and MAX_3D_TEXTURE_SIZE. Every dimension of the image must be less than the value of the MAX_3D_TEXTURE_SIZE and the full dataset must fit into GPU’s RAM. For the full discussion on these limits, see the Slicer discourse thread [here](https://discourse.slicer.org/t/what-spec-gpu-is-required-for-gpu-volumentric-rendering/1596).
* Driver issues: To configure laptops with two GPUS see [this discussion](https://discourse.slicer.org/t/can-i-choose-which-gpu-to-use/3149)
* Crop 3D view vs Crop Volume confusion
## Example: Volume Rendering 
1. Load the MRIHead volume from the Sample Data module.
2. Open the Volume Rendering module. In the **Volume** field, make sure the volume MRHead is selected. Click the eyeball next to the **Volume** field to display the image. You can change the 3D Slicer layout to 3D only.

<img src="./images/initialDisplay.png">

3. Expand the **Advanced** tab to view the opacity and color transfer functions. You can click on these functions to move or add additional control points.
<img src="./images/initialTF.png">

4. Under the **Display** tab, click on the **Select a Preset** menu. This menu contains saved transfer functions that work well for common data types. Select **MRI Default** (row 4, column 5). Try adjusting the color and opacity functions of this suggested display setting.
<img src="./images/colorPreset.png">

## Example: Displaying Mesh Data
Mesh data in Slicer is displayed using the Models Module. It can not be rendered using the Volume Render Module.
1. Load the Gorilla Skull Reference Model under the SlicerMorph tab of the Sample Data module (you will need SLicerMorph installed to see this option in the menu).

<img src="./images/sampleDataGorilla.png">

2.Center the dataset in the 3D viewing window using the button at the top left of the window. Optionally, change to the 3D only layout.

<img src="./images/centerGorilla.png">

3. Open the Models module. Experiment with changing the color and opacity of the skull.
<img src="./images/Models.png">


## Bonus content: SlicerAnimator (if we have time)
1. Install Sequences extension from Extension Manager.
2. git clone https://github.com/pieper/SlicerAnimator
3. add the directory SlicerAnimator/Animator to the Modules tab of Settings (Edit->Application Settings)
4. Restart Slicer and search for Animator module 

* [Demo video](https://youtu.be/9GBekYcJR4E)
