# Lab 04 Slicer#3: Measurements (angles, lines, 3D curves, landmarks) and Visualization 
* thorough review of existing functionality in preview version (and lacking in stable)
* Linear transformations (explain what hardening does to IJK2RAS matrix, and why some other programs won't see it e.g., Fiji) https://discourse.slicer.org/t/image-with-hardened-transform-returns-to-original-when-opened-outside-slicer/431

* [calculate area of a from CT slice](https://discourse.slicer.org/t/how-can-i-calculate-an-area-on-a-ct-image-i-can-calculate-volumes-mm-3-but-not-areas-mm-2/1549/5)
* check the status of measure planes extension 

## Volume Rendering
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
