# Lab 05 Slicer#4: Segmentation, mesh conversion, 3D printing 

Welcome to Slicer Lab #4: Segmentation :scissors: :weight_lifting_woman:

## Segmentation

Segmentation is one of the most useful things you can do with Slicer! It could be automated / semi-automated or fully manual! Most of the time, you will need a semi-automated approach. 

<img src="https://www.slicer.org/w/images/c/c8/WholeHeartSegYoutube.png">

A segmentation is an abstract representation of stuctures in an image. It usually contains more than one segment (usually non-overlapping) annotated with different colors in 2D and 3D. In Slicer, Segmentations and Segment Editor modules will be our friends during this lab. 

### Segmentation vs. LabelMap vs. Model
A segmentation is a group of segments. It needs a "Master Volume" to work on. You can create segmentation based on the intensities of voxels (or pixels) and their temporal/spatial properties (nearby voxels/pixels with similar intensities belong to the same segment). 

A *label map* is an *image* where the value of each voxel/pixel is the label of the segment that voxel/pixel belongs to. A label map for an image is the same size/dimensions (and even physical space if the format of the image allows it) as the image and assigns exactly one label to all voxels/pixels. Usually 0 is the background. A segmentation does not have these constraints, segments can overlap, do not need to cover all pixels/volxels and can be represented in different ways (it doesn't need to be an image). Label maps are very useful for storing segmentations (they are the same size as images and usually have very few pixel values, a 5-segment segmentation needs only 6 integers to encode and can be compressed efficiently). Label maps are also useful for some analysis methods. 

A *model* is a *surface mesh* with points and polygons (usually triangles) and a very useful representation for segments. Usually, the outer surface of a 3D segment is represented with a closed surface mesh. Models are good for visualization and some analysis methods. However, they can be quite large and cumbersome to read and write. (Side note: existing mesh formats are inefficient for I/O and they don't have "units" and "physical space" which are critical for medical applications. One needs to be very careful playing with meshes converted from volumes.)

In Slicer, you will first create a Segmentation - which can be converted to LabelMap or a Model depending on your analysis.

## Segment Editor 

:pencil: Load MRBrainTumor1 data from SampleData module. Let's segment that tumor. 

<img src="images/sample.data.png">

* Find Segment Editor module from the list, through search function or from the Core Modules shortcuts. 

<img src="images/segment.editor.png">

* Name your segment (if you'd like - it is nice to keep track of things if you are working with a lot of data in one session) and select your master volume, MRBrainTumor1 in this case. 

<img src="images/segment.editor2.png">

* Add a segment by clicking +Add button. It will be named "Segment_1" if it is the first one you created and the numbers will keep increasing as you add more segments. Let's segment the tumor in Segment_1 so rename the segment properly (as Tumor for example). You can rename the segment by double-clicking on its name. As soon as you add the segment, the tools in the Segment Editor are enabled. Click on the first one, "Paint".

<img src="images/segment.editor3.png">

* Each Effect (or tool) is useful in a different scenario (some more than others) and you will have favorites soon. But first things first: 
  * Slicer gives you parameters and options for each tool. Use them. 
  * Undo/Redo buttons allow you to take back some steps but they are limited (I guess 8?).
  * Masking area is important. Right now you have one segment but you will have more than one and if you are not careful, you can erase or cut or overwrite other segments. Set these options carefully EACH TIME before you apply an effect /tool. I like to keep the "Overwrite other segments" option in "Visible segments" and make all other segments invisible if I am not working with them. This is one mistake that's hard to recover from... :sob:
    * None: none of the other segments are overwritten so blue segment is overlapping with the green one.
    <img src="images/overwrite.png">
    * All segments: The new segment can overwrite the others if it overlaps with them.
    <img src="images/overwrite2.png">
    * Visible segments: I made the green segment invsible, painted with the light reddish one, and it overwrote the blue. But when I turned the visibility of the green segment back on, it was overlapping with the new segment and was not overwritten. Expected effect. 
    <img src="images/overwrite3.png">
    
    <img src="images/overwrite4.png"> 
  
  :Pencil: Explore some of the Effects (tools). 


* Segment Editor 
* Segment Editor Extra Effects extension (flood fill, mask/split volume, etc)
* Logical operations on segments
* Segmentations module (to export/import segmentation)
* segment statistics
* segment registration (extension, just make them aware)

* Surface Toolbox (for smoothing, decimation, etc, but not as many functions as Meshlab or Blender)
* Models Module (for displaying 3D models, manipulating color, opacity, stats)
* Bonus pointer to discussion on Segmentation to Finite Element ANalysis https://discourse.slicer.org/t/use-segmentations-in-cad-or-fem-software/1626
* also relevant Segment Mesher extension 

