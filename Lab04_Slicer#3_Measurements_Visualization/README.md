# Lab 04 Slicer#3: Measurements (angles, lines, 3D curves, landmarks) and Visualization 
## Overview of markups
* Stable version of Slicer only supports fiducial markups 
* Preview version adds: lines, angles, open and closed curves (planes will be added to the list in the upcoming months)
<img src="https://github.com/SlicerMorph/S_2019/blob/master/Lab04_Slicer%233_Measurements_Visualization/images/MarkupWidgets.png">

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

<img src="https://github.com/SlicerMorph/S_2019/blob/master/Lab04_Slicer%233_Measurements_Visualization/images/MarkupTypes.png">

## Markup Placement
  * Slicer has two mouse modes: Transform, and Place. 
  * Transform mode is the default interaction mode. This mode allows interaction with loaded data (pan, zoom, rotate)
  * The icons in the mouse mode toolbar at the top of the main GUI allow to switch between these modes
  * Place mode allows to place one object then switches modes back to Transform mode. Fiducial is the default object.
  * Place mode can be made persistent by clicking the checkbox in the mouse mode toolbar.
<img src="https://github.com/SlicerMorph/S_2019/blob/master/Lab04_Slicer%233_Measurements_Visualization/images/FiducialPersistence.png">

## Markup Management
Fiducial points and anchor points of lines and curves can be accessed and manipulated using the Markups Module. 
<img src="https://github.com/SlicerMorph/S_2019/blob/master/Lab04_Slicer%233_Measurements_Visualization/images/markupsModule.png">
* In the display menu, set the visibility, opacity, glyph and text size of a markup node
* In the markups table, adjust visibility, labels, and position of individual fiducials or anchor points

## Example: Using Markups for Measurement
In this example, we will place a closed curve on one slice of a CT scan, measure the area of the curve, and visualize the region. For more detail and discussion, see the Slicer discourse thread [here](https://discourse.slicer.org/t/how-can-i-calculate-an-area-on-a-ct-image-i-can-calculate-volumes-mm-3-but-not-areas-mm-2/1549/7).

1. Select the Sample Data module and load the MRIHead volume. 
 <img src="https://github.com/SlicerMorph/S_2019/blob/master/Lab04_Slicer%233_Measurements_Visualization/images/sampleData.png">

2. Select the closed curve markup mode and place a curve around the brain tissue in the red view window (axial slice). You can change the Slicer layout to red window only for better detail.
<img src="https://github.com/SlicerMorph/S_2019/blob/master/Lab04_Slicer%233_Measurements_Visualization/images/CurveOnRed.png">

3. Open the Python Interactor and paste the following snippet of code to calculate the area of the curve:
```
curve=slicer.util.getNodesByClass("vtkMRMLMarkupsClosedCurveNode")[0]
crossSectionSurface = vtk.vtkPolyData()
areaMm2 = slicer.modules.markups.logic().GetClosedCurveSurfaceArea(curve, crossSectionSurface)
print("Curve {0}: surface area = {1:.2f} mm2".format(curve.GetName(), areaMm2))
```
<img src="https://github.com/SlicerMorph/S_2019/blob/master/Lab04_Slicer%233_Measurements_Visualization/images/pythonInteract.png">


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
 <img src="https://github.com/SlicerMorph/S_2019/blob/master/Lab04_Slicer%233_Measurements_Visualization/images/VisualizingCurveArea.png">


* Linear transformations (explain what hardening does to IJK2RAS matrix, and why some other programs won't see it e.g., Fiji) https://discourse.slicer.org/t/image-with-hardened-transform-returns-to-original-when-opened-outside-slicer/431

* [calculate area of a from CT slice](https://discourse.slicer.org/t/how-can-i-calculate-an-area-on-a-ct-image-i-can-calculate-volumes-mm-3-but-not-areas-mm-2/1549/5)
* check the status of measure planes extension 

## Volume Rendering
* [Volume Rendering panel and functions](https://www.slicer.org/wiki/Documentation/Nightly/Modules/VolumeRendering#Panels_and_their_use)
* Volume rendering vs surface rendering
* opacity transfer function
* color adjustments
* GPU vs CPU raycasting performance differences
* always set the rendering quality to normal 
* physical limits to volume rendering (discussion on Discourse)
* driver issues: how to configure laptops with two GPUS (discrete and onboard intel ones)
* remind that volume rendering is not a segmentation. 
* Crop 3D view vs Crop Volume confusion

## Bonus content: SlicerAnimator (if we have time)
1. Install Sequences extension from Extension Manager.
2. git clone https://github.com/pieper/SlicerAnimator
3. add the directory SlicerAnimator/Animator to the Modules tab of Settings (Edit->Application Settings)
4. Restart Slicer and search for Animator module 

* [Demo video](https://youtu.be/9GBekYcJR4E)
