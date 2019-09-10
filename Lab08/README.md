# Lab 08 - Using the Python Interactor in Slicer
This lab introduces simple Python scripts to preform custom image preprocessing tasks. These scripts can easily be customized and show how the functions in the Slicer application can be extended or automated using the Python interactor. 

**Note 1: These examples assume you are starting in an empty scene. Please clear the scene before beginning each example.**

**Note 2: Example 2 requires the Segment Editor Extra Effects Extension to be installed. Please install this from the extension manager and restart the Slicer application before beginning.**

## Example 1: Aligning volumes using landmarks (Frankfort alignment plane).
The Frankfurt plane defines the standard anatomical position of the human skull. The plane passes through landmarks placed at the left orbitale and the left and right tragus. Alignment with the Frankfurt plane is a standard preprocessing step when analyzing human face, brain, or head images.
<img src="https://github.com/SlicerMorph/S_2019/blob/master/Lab08/images/Frankfurt.png">

1. Download the sample data using [this link](https://github.com/SlicerMorph/S_2019/raw/master/Lab08/data/MRHead.nrrd). It contains a CT scan of a human head that is out of the standard alignment. 
2. Open Slicer and load the data. In the Volume Rendering module, adjust the view properties. 

<img src="https://github.com/SlicerMorph/S_2019/blob/master/Lab08/images/unaligned.png">

3. Use the menu button on the top bar to initiate lanmark placement mode. Place three landmarks on the face at the left orbitale (lowest point of the left eye socket) and the left and right tragus (upper margin of each ear canal). It's ok to approximate for this exercise. 

4. Open the Markups module and rename the landmarks 
    * left orbitale: **zyoL**
    * left tragus: **poL**
    * right tragus: **poR**

<img src="https://github.com/SlicerMorph/S_2019/blob/master/Lab08/images/landmarks.png">

5. Open the Python Interactor. Copy and paste the script below. Lines starting with the # character are comments that give you information on how the code is operating. They are ignored by the Python Interactor.  

```
import numpy
scene = slicer.mrmlScene
F = getNodesByClass('vtkMRMLMarkupsFiducialNode')
F = F[0]

# Get the fiducial IDs of porions and zygomatico - orbitale  suture from the fiducial list by name
po1_id = -1; po2_id = -1; zyo_id = -1;

for i in range(0, F.GetNumberOfFiducials()):
	if F.GetNthFiducialLabel(i) == 'poR' :
		po1_id = i
	if F.GetNthFiducialLabel(i) == 'poL' :
		po2_id = i
	if F.GetNthFiducialLabel(i) == 'zyoL' :
		zyo_id = i


# Get the coordinates of landmarks
po1 = [0, 0, 0]
po2 = [0, 0, 0]
zyo = [0, 0, 0]

F.GetNthFiducialPosition(po1_id,po1)
F.GetNthFiducialPosition(po2_id,po2)
F.GetNthFiducialPosition(zyo_id,zyo)

# The vector between two porions that we will align to LR axis by calculating the yaw angle
po =[po1[0] - po2[0], po1[1] -po2[1], po1[2]-po2[2]]
vTransform = vtk.vtkTransform()
vTransform.RotateZ(-numpy.arctan2(po[1], po[0])*180/numpy.pi)
transform.SetAndObserveMatrixTransformToParent(vTransform.GetMatrix())

# Apply the transform to the fiducials and the volume
transform = slicer.vtkMRMLLinearTransformNode()
scene.AddNode(transform) 
transform.SetAndObserveMatrixTransformToParent(vTransform.GetMatrix())
V = getNodesByClass('vtkMRMLScalarVolumeNode')
V = V[0]
V.SetAndObserveTransformNodeID(transform.GetID())
F.SetAndObserveTransformNodeID(transform.GetID())

# Get the updated (transformed) coordinates from the list
po1 = [0, 0, 0]
po2 = [0, 0, 0]
zyo = [0, 0, 0]

F.GetNthFiducialPosition(po1_id,po1)
F.GetNthFiducialPosition(po2_id,po2)
F.GetNthFiducialPosition(zyo_id,zyo)

# Apply the transform to the coordinates
po1 = vTransform.GetMatrix().MultiplyPoint([po1[0], po1[1], po1[2], 0])
po2 = vTransform.GetMatrix().MultiplyPoint([po2[0], po2[1], po2[2], 0])
zyo = vTransform.GetMatrix().MultiplyPoint([zyo[0], zyo[1], zyo[2], 0])
po =[po1[0]-po2[0], po1[1]-po2[1], po1[2]-po2[2]]

# Calculate the rotation for the roll 
vTransform2 = vtk.vtkTransform()
vTransform2.RotateY(numpy.arctan2(po[2], po[0])*180/numpy.pi)

# Apply the transform to previous transform hierarchy
transform2 = slicer.vtkMRMLLinearTransformNode()
scene.AddNode(transform2) 
transform2.SetAndObserveMatrixTransformToParent(vTransform2.GetMatrix())
transform.SetAndObserveTransformNodeID(transform2.GetID())

# Get the coordinates again
po1 = [0, 0, 0]
po2 = [0, 0, 0]
zyo = [0, 0, 0]

F.GetNthFiducialPosition(po1_id,po1)
F.GetNthFiducialPosition(po2_id,po2)
F.GetNthFiducialPosition(zyo_id,zyo)

# Apply transforms to points to get updated coordinates
po1 = vTransform.GetMatrix().MultiplyPoint([po1[0], po1[1], po1[2], 0])
po2 = vTransform.GetMatrix().MultiplyPoint([po2[0], po2[1], po2[2], 0])
zyo = vTransform.GetMatrix().MultiplyPoint([zyo[0], zyo[1], zyo[2], 0])
po1 = vTransform2.GetMatrix().MultiplyPoint([po1[0], po1[1], po1[2], 0])
po2 = vTransform2.GetMatrix().MultiplyPoint([po2[0], po2[1], po2[2], 0])
zyo = vTransform2.GetMatrix().MultiplyPoint([zyo[0], zyo[1], zyo[2], 0])

# The vector for pitch angle
po_zyo = [zyo[0]-(po1[0]+po2[0])/2, zyo[1]-(po1[1]+po2[1])/2, zyo[2]-(po1[2]+po2[2])/2]

# Calculate the transform for aligning pitch
vTransform3 = vtk.vtkTransform()
vTransform3.RotateX(-numpy.arctan2(po_zyo[2], po_zyo[1])*180/numpy.pi)

# Apply the transform to both fiducial list and the volume
transform3 = slicer.vtkMRMLLinearTransformNode()
scene.AddNode(transform3) 
transform3.SetAndObserveMatrixTransformToParent(vTransform3.GetMatrix())
transform2.SetAndObserveTransformNodeID(transform3.GetID())

```
The image should now appear in standard anatomical alignment. In the 3D viewer, you can expand the view menu with the pin icon. Clicking on the axis labels will select the standard viewpoints. If you are satisfied with the result, you can "harden" the transform using the GUI or the Python command:

```
slicer.vtkSlicerTransformLogic().hardenTransform(V)
```

<img src="https://github.com/SlicerMorph/S_2019/blob/master/Lab08/images/aligned.png">

## Example 2: Reading a segmentation and creating a histogram.
In this section, you will use a segmentation to mask an image, calculate regional statistics and plot a histogram for each segment. This example is based on a tutorial [provided by Andras Lasso](https://gist.github.com/lassoan/2f5071c562108dac8efe277c78f2620f). 

In this example, sample patches from different tissue types in a brain scan will be identified using segment labels. The "Tumor" label will be used to identify a patch of tumor tissue and the "Brain" label will identify patches of brain tissue unaffected by the tumor.  The "Background" label will be used to sample multiple tissue types. Comparing the histogram plots will allow analysis of the differences in voxel intensities between the "Tumor" and "Brain" regions, as well as how these compare to the overall distribution of values in the scan represented by the the "Background" regions.  

1. Open Slicer and load the MRBrainTumor1 volume from the Sample Data module.

2. Open the Segment Editor. Create three segments: Tumor, Brain, and Background. Select the spherical paint brush to place the segments on the brain volume. The colors of the segments can be adjusted for better visibility by clicking on the color boxes in the Segment Editor table.

<img src="https://github.com/SlicerMorph/S_2019/blob/master/Lab08/images/segments.png">

3. Use the following code snippet to iteratively mask the image with each segment and calculate the corresponding histogram. You should see an output plot of the histograms for each segment. This plot should show that the tumor tissue histogram has a peak at a higher voxel intensity value than the unaffected brain tissue and background samples.  
```
import numpy as np
masterVolumeNode = getNodesByClass('vtkMRMLScalarVolumeNode')
masterVolumeNode=masterVolumeNode[0]
segmentationNode = getNodesByClass('vtkMRMLSegmentationNode')
segmentationNode = segmentationNode[0]

# Create segment editor to get access to effects
segmentEditorWidget = slicer.qMRMLSegmentEditorWidget()
# To show segment editor widget (useful for debugging): segmentEditorWidget.show()
segmentEditorWidget.setMRMLScene(slicer.mrmlScene)
segmentEditorNode = slicer.vtkMRMLSegmentEditorNode()
slicer.mrmlScene.AddNode(segmentEditorNode)
segmentEditorWidget.setMRMLSegmentEditorNode(segmentEditorNode)
segmentEditorWidget.setSegmentationNode(segmentationNode)
segmentEditorWidget.setMasterVolumeNode(masterVolumeNode)

# Set up masking parameters
segmentEditorWidget.setActiveEffectByName("Mask volume")
effect = segmentEditorWidget.activeEffect()
# set fill value to be outside the valid intensity range
intensityRange = masterVolumeNode.GetImageData().GetScalarRange()
effect.setParameter("FillValue", str(intensityRange[0]-1))
# Blank out voxels that are outside the segment
effect.setParameter("Operation", "FILL_OUTSIDE")
# Create a volume that will store temporary masked volumes
maskedVolume = slicer.mrmlScene.AddNewNodeByClass("vtkMRMLScalarVolumeNode", "Temporary masked volume")
effect.self().outputVolumeSelector.setCurrentNode(maskedVolume)

# Create chart
plotChartNode = slicer.mrmlScene.AddNewNodeByClass("vtkMRMLPlotChartNode", "Histogram")
plotChartNode.AddAndObservePlotSeriesNodeID(plotSeriesNode.GetID())

# Create histogram plot data series for each masked volume
for segmentIndex in range(segmentationNode.GetSegmentation().GetNumberOfSegments()):
  # Set active segment
  segmentID = segmentationNode.GetSegmentation().GetNthSegmentID(segmentIndex)
  segmentEditorWidget.setCurrentSegmentID(segmentID)
  # Apply mask
  effect.self().onApply()
  # Compute histogram values
  histogram = np.histogram(arrayFromVolume(maskedVolume), bins=100, range=intensityRange)
  # Save results to a new table node
  segment = segmentationNode.GetSegmentation().GetNthSegment(segmentIndex)
  tableNode=slicer.mrmlScene.AddNewNodeByClass("vtkMRMLTableNode", segment.GetName() + " histogram table")
  updateTableFromArray(tableNode, histogram)
  tableNode.GetTable().GetColumn(0).SetName("Count")
  tableNode.GetTable().GetColumn(1).SetName("Intensity")
  # Create new plot data series node
  plotSeriesNode = slicer.mrmlScene.AddNewNodeByClass("vtkMRMLPlotSeriesNode", segment.GetName() + " histogram")
  plotSeriesNode.SetAndObserveTableNodeID(tableNode.GetID())
  plotSeriesNode.SetXColumnName("Intensity")
  plotSeriesNode.SetYColumnName("Count")
  plotSeriesNode.SetPlotType(slicer.vtkMRMLPlotSeriesNode.PlotTypeScatter)
  plotSeriesNode.SetMarkerStyle(slicer.vtkMRMLPlotSeriesNode.MarkerStyleNone)
  plotSeriesNode.SetUniqueColor()
  # Add plot to chart
  plotChartNode.AddAndObservePlotSeriesNodeID(plotSeriesNode.GetID())

# Show chart in layout
slicer.modules.plots.logic().ShowChartInLayout(plotChartNode)

# Delete temporary node
slicer.mrmlScene.RemoveNode(maskedVolume)
slicer.mrmlScene.RemoveNode(segmentEditorNode)
```
<img src="https://github.com/SlicerMorph/S_2019/blob/master/Lab08/images/histogram.png">

4. Bonus: Generate or load your own segmentation. Reuse the code snippet from Step 3 to calculate segment histograms.

5. Bonus: Get the maximim value from each segment by modifying the code from Step 3. You can try this on your own to test your understanding. The solution is posted below.

  ```
 import numpy as np
masterVolumeNode = getNodesByClass('vtkMRMLScalarVolumeNode')
masterVolumeNode=masterVolumeNode[0]
segmentationNode = getNodesByClass('vtkMRMLSegmentationNode')
segmentationNode = segmentationNode[0]

# Create segment editor to get access to effects
segmentEditorWidget = slicer.qMRMLSegmentEditorWidget()
# To show segment editor widget (useful for debugging): segmentEditorWidget.show()
segmentEditorWidget.setMRMLScene(slicer.mrmlScene)
segmentEditorNode = slicer.vtkMRMLSegmentEditorNode()
slicer.mrmlScene.AddNode(segmentEditorNode)
segmentEditorWidget.setMRMLSegmentEditorNode(segmentEditorNode)
segmentEditorWidget.setSegmentationNode(segmentationNode)
segmentEditorWidget.setMasterVolumeNode(masterVolumeNode)

# Set up masking parameters
segmentEditorWidget.setActiveEffectByName("Mask volume")
effect = segmentEditorWidget.activeEffect()
# set fill value to be outside the valid intensity range
intensityRange = masterVolumeNode.GetImageData().GetScalarRange()
effect.setParameter("FillValue", str(intensityRange[0]-1))
# Blank out voxels that are outside the segment
effect.setParameter("Operation", "FILL_OUTSIDE")
# Create a volume that will store temporary masked volumes
maskedVolume = slicer.mrmlScene.AddNewNodeByClass("vtkMRMLScalarVolumeNode", "Temporary masked volume")
effect.self().outputVolumeSelector.setCurrentNode(maskedVolume)

for segmentIndex in range(segmentationNode.GetSegmentation().GetNumberOfSegments()):
  # Set active segment
  segmentID = segmentationNode.GetSegmentation().GetNthSegmentID(segmentIndex)
  segmentEditorWidget.setCurrentSegmentID(segmentID)
  # Apply mask
  effect.self().onApply()
  a = arrayFromVolume(maskedVolume)
  print(a.max())
  ```


