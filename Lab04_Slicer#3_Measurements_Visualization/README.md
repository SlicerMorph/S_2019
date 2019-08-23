# Lab 04 Slicer#3: Measurements (angles, lines, 3D curves, landmarks) and Visualization 
**Overview of markups**
* Stable version of Slicer only supports fiducial markups 
* Preview version adds: lines, angles, open and closed curves
<img src="https://github.com/SlicerMorph/S_2019/blob/master/Lab04_Slicer%233_Measurements_Visualization/images/MarkupWidgets.png">

* Using the preview version is strongly recommended due to the additional markups types and improvements to the fiducial representation  allowing better performance with large numbers of markups and easier user interaction
* Updates to Markups module are ongoing so check back for updates
**Fiducials**

**Angles**

**Lines**

**Open and closed curves**

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
