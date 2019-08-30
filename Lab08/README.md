# Lab 08 - Using the Python Interactor in Slicer
This lab introduces simple Python scripts to preform custom image preprocessing tasks. These scripts can easily be customized and show how the functions in the Slicer application can be extended or automated using the Python interactor.

## Aligning volumes using landmarks (Frankfort alignment plane).
The Frankfurt plane defines the standard anatomical position of the human skull. The plane passes through landmarks placed at the left orbitale and the left and right tragus. Alignment with the Frankfurt plane is a standard preprocessing step when analyzing human face, brain, or head images.
<img src="https://github.com/SlicerMorph/S_2019/blob/master/Lab08/images/Frankfurt.png">

1. Download the sample data here. It contains a CT scan of a human head that is out of the standard alignment. 
2. Open Slicer and load the data. In the Volume Rendering module, adjust the view properties. 

<img src="https://github.com/SlicerMorph/S_2019/blob/master/Lab08/images/unaligned.png">

3. Use the menu button on the top bar to initiate lanmark placement mode. Place three landmarks on the face at the left orbitale (lowest point of the left eye socket) and the left and right tragus (upper margin of each ear canal). It's ok to approximate for this exercise. 

4.Open the Markups module and rename the landmarks 
    * left orbitale: **zyoL**
    * left tragus: **poL**
    * right tragus: **poR**

<img src="https://github.com/SlicerMorph/S_2019/blob/master/Lab08/images/landmarks.png">

5. Open the Python Interactor. Copy and paste the following script: 

```
import numpy
scene = slicer.mrmlScene
F = getNodesByClass('vtkMRMLMarkupsFiducialNode')
F = F[0]

po1_id = -1; po2_id = -1; zyo_id = -1;

for i in range(0, F.GetNumberOfFiducials()):
	if F.GetNthFiducialLabel(i) == 'poR' :
		po1_id = i
	if F.GetNthFiducialLabel(i) == 'poL' :
		po2_id = i
	if F.GetNthFiducialLabel(i) == 'zyoL' :
		zyo_id = i

po1 = [0, 0, 0]
po2 = [0, 0, 0]
zyo = [0, 0, 0]

F.GetNthFiducialPosition(po1_id,po1)
F.GetNthFiducialPosition(po2_id,po2)
F.GetNthFiducialPosition(zyo_id,zyo)

po =[po1[0] - po2[0], po1[1] -po2[1], po1[2]-po2[2]]
vTransform = vtk.vtkTransform()
vTransform.RotateZ(-numpy.arctan2(po[1], po[0])*180/numpy.pi)
transform.SetAndObserveMatrixTransformToParent(vTransform.GetMatrix())

transform = slicer.vtkMRMLLinearTransformNode()
scene.AddNode(transform) 
transform.SetAndObserveMatrixTransformToParent(vTransform.GetMatrix())
V = getNodesByClass('vtkMRMLScalarVolumeNode')
V = V[0]
V.SetAndObserveTransformNodeID(transform.GetID())
F.SetAndObserveTransformNodeID(transform.GetID())

po1 = [0, 0, 0]
po2 = [0, 0, 0]
zyo = [0, 0, 0]

F.GetNthFiducialPosition(po1_id,po1)
F.GetNthFiducialPosition(po2_id,po2)
F.GetNthFiducialPosition(zyo_id,zyo)

po1 = vTransform.GetMatrix().MultiplyPoint([po1[0], po1[1], po1[2], 0])
po2 = vTransform.GetMatrix().MultiplyPoint([po2[0], po2[1], po2[2], 0])
zyo = vTransform.GetMatrix().MultiplyPoint([zyo[0], zyo[1], zyo[2], 0])
po =[po1[0]-po2[0], po1[1]-po2[1], po1[2]-po2[2]]

vTransform2 = vtk.vtkTransform()
vTransform2.RotateY(numpy.arctan2(po[2], po[0])*180/numpy.pi)

transform2 = slicer.vtkMRMLLinearTransformNode()
scene.AddNode(transform2) 
transform2.SetAndObserveMatrixTransformToParent(vTransform2.GetMatrix())
transform.SetAndObserveTransformNodeID(transform2.GetID())

po1 = [0, 0, 0]
po2 = [0, 0, 0]
zyo = [0, 0, 0]

F.GetNthFiducialPosition(po1_id,po1)
F.GetNthFiducialPosition(po2_id,po2)
F.GetNthFiducialPosition(zyo_id,zyo)

po1 = vTransform.GetMatrix().MultiplyPoint([po1[0], po1[1], po1[2], 0])
po2 = vTransform.GetMatrix().MultiplyPoint([po2[0], po2[1], po2[2], 0])
zyo = vTransform.GetMatrix().MultiplyPoint([zyo[0], zyo[1], zyo[2], 0])
po1 = vTransform2.GetMatrix().MultiplyPoint([po1[0], po1[1], po1[2], 0])
po2 = vTransform2.GetMatrix().MultiplyPoint([po2[0], po2[1], po2[2], 0])
zyo = vTransform2.GetMatrix().MultiplyPoint([zyo[0], zyo[1], zyo[2], 0])
po_zyo = [zyo[0]-(po1[0]+po2[0])/2, zyo[1]-(po1[1]+po2[1])/2, zyo[2]-(po1[2]+po2[2])/2]

vTransform3 = vtk.vtkTransform()
vTransform3.RotateX(-numpy.arctan2(po_zyo[2], po_zyo[1])*180/numpy.pi)

transform3 = slicer.vtkMRMLLinearTransformNode()
scene.AddNode(transform3) 
transform3.SetAndObserveMatrixTransformToParent(vTransform3.GetMatrix())
transform2.SetAndObserveTransformNodeID(transform3.GetID())
```
The image should now appear in standard anatomical alignment. In the 3D viewer, you can expand the view menu with the pin icon. CLicking on the axis points will select the standard viewpoints. 

<img src="https://github.com/SlicerMorph/S_2019/blob/master/Lab08/images/aligned.png">

## Reading a segmentation and creating a histogram.
In this section, you will use a segmentation to mask an image, calculate regional statistics and plot a histogram for each segment. For the first step, you may either do your own segmentation on the MRHead sample image, or you may copy and past a Python script that will automatically generate a simple segmentation. 




