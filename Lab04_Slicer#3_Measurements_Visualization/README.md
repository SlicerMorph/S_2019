# Lab 04 Slicer#3: Measurements (angles, lines, 3D curves, landmarks) and Visualization 
* thorough review of existing functionality in preview version (and lacking in stable)
* Linear transformations (explain what hardening does to IJK2RAS matrix, and why some other programs won't see it e.g., Fiji) https://discourse.slicer.org/t/image-with-hardened-transform-returns-to-original-when-opened-outside-slicer/431

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