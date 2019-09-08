# Summer 2019 3D Morphometrics Workshop at FHL 
Lectures, labs and all documents associated with 3D Morphometrics workshop at FHL (Summer 2019)
<img alt="SlicerMorph logo" width="358" height="256" src="https://github.com/SlicerMorph/SlicerMorph.github.io/blob/master/SlicerMorph_Logos/SlicerMorph_Final_Logos-V2.jpg">

## Computer and software instructions for the 3D Morphometrics Workshop
**Computer specs:** Please bring a laptop with these suggested specs:
*	A quad-core CPU from last couple years (i7 or i5 is preferred). 
*	1920x1080 or higher screen resolution.
*	8GB or higher RAM (memory)
*	100-200GB available storage space (for software and data). Please note that solid state drives (SSD) both SATA and non-volatile memory express (NVMe) are preferred over spinning hard-drive disks (HDD) due their high sequential read/write performance (usually 10X or more faster than HDDs). 
*	 A discrete (not an integrated one) GPU with minimum of 2GB of RAM with the latest GPU driver installed.  
*	A three-buttoned mouse. 

Your laptop should be running windows 10 (windows 7 has issues and is not supported anymore), or Mac OS 10.11 (El Capitan) or later.  A recent version of a common Linux distribution (like Ubuntu or CentOS) is also fine.
Please note that segmentation is a memory intensive operation. It is suggested, you have 6-10X more memory than your full dataset size (i.e., if you are working on a 1024x1024x1024 dataset, you will need about 10GB RAM to work on in in Slicer). You can always reduce your dataset to match your hardware capacity.

**Required software:** You should have these software install these on your laptops before coming to workshop.
1.	Please download and install the latest stable (4.10.2) and the preview (4.11.X) versions of the Slicer on your computer from https://download.slicer.org. Because of the constant changes and bug fixes being introduced to preview version, we suggest installing the preview closer to the date of the workshop.
2.	Download and install git from https://git-scm.com/downloads 
3.	Download and install R 3.6.0 from https://cran.r-project.org/
4.	Download and install Rstudio Desktop from https://www.rstudio.com/products/rstudio/download/
5.	TurboVNC: https://sourceforge.net/projects/turbovnc/files/ 

**Additional software:** We will not use them for the workshop specifically, but you might find them useful for specific tasks:
1.	Drishti (mac and windows only); https://github.com/nci/drishti/releases
2.	Convert3d (command line tools for image conversion) https://sourceforge.net/projects/c3d/
3.	Dcm2niix (DICOM to nifti conversion) https://www.nitrc.org/plugins/mwiki/index.php/dcm2nii:MainPage
4.	Fiji https://fiji.sc/
5.  [If you would like to use our remote server for Slicer, please review instructions](./remote.md)

**Accounts:** Please create accounts on these websites prior to workshop
*	MorphoSource: https://morphosource.org
*	GitHub https://github.com
*	Slicer Forum, https://discourse.slicer.org (alternatively you can use your github or google accounts to signup)
*	Please sign up for the SlicerMorph announcement to keep up-to-date with SlicerMorph project and extension updates http://mailman11.u.washington.edu/mailman/listinfo/slicermorph-announcements

**Important Websites:**

*	SlicerMorph project website: https://SlicerMorph.github.io
*	Summer Workshop website: https://SlicerMorph.github.io/2019_Summer_Workshop
*	Summer Workshop lectures and labs https://github.com/SlicerMorph/S_2019
* Link to the file dropbox to upload your lightning talks (or any other data) https://faculty.washington.edu/maga/data_dropbox/

#### Funding acknowledgement
Development of SlicerMorph and the intense workshops are generously funded by National Science Foundation Advances in Bioinformatics collobrative research grants to Murat Maga (ABI-1759883), Adam Summers (ABI-1759637) and Doug Boyer (ABI-1759839). 


### Links to Specific Labs
1. [Lab 1: Tools for reproducible research (git/github)](https://github.com/SlicerMorph/S_2019/tree/master/Lab01_Reproducible_research) 
2. [Lab 2: Slicer #1 UI overview, extensions, finding help](https://github.com/SlicerMorph/S_2019/tree/master/Lab02_Slicer_1_Intro)
3. [Lab 3: Slicer #2 Data formats, importing data, saving](https://github.com/SlicerMorph/S_2019/tree/master/Lab03_Slicer_2_data_formats_input_output)
4. [Lab 4: Slicer #3 Measurements and Visualization](https://github.com/SlicerMorph/S_2019/tree/master/Lab04_Slicer_3_Measurements_Visualization)
5. [Lab 5: Slicer #4 Segmentation and mesh conversion help](https://github.com/SlicerMorph/S_2019/tree/master/Lab05_Slicer_4_Segmentation)
5. [Lab 6: SlicerMorph #1 Statistical Shape ANalysis](https://github.com/SlicerMorph/S_2019/tree/master/Lab06_SlicerMorph_1)
6. [Lab 7: SlicerMorph #1 Statistical Shape ANalysis - work on your own](https://github.com/SlicerMorph/S_2019/tree/master/Lab07_SlicerMorph_2)
7. [Lab 8: Python in Slicer - Scripting tedious tasks](https://github.com/SlicerMorph/S_2019/tree/master/Lab08)
8. [Lab 9: Auto3dGM - Establishing landmark-free shape correspondence](https://github.com/SlicerMorph/S_2019/tree/master/Lab09)
9. [Lab 10: Data Processing in R #1: import/export, geomorph package](https://github.com/SlicerMorph/S_2019/tree/master/Lab10)

### Links to Lecture Slides
1. [Lecture 1: Introduction to 3D Imaging and Morphometrics (Maga)](https://github.com/SlicerMorph/S_2019/tree/master/Lecture01_Intro) 
2. [Lecture 2: Applied Imaging Concepts (Rolfe)](https://github.com/SlicerMorph/S_2019/tree/master/Lecture02_Imaging_Basics)
3. [Lecture 3: Statistical Shape Analysis #1: Concepts and Basics (Maga)](https://github.com/SlicerMorph/S_2019/tree/master/Lecture03_SSA_1)
4. [Lecture 4: Statistical Shape Analysis #2: Semi Landmarks and Beyond (Rolfe)](https://github.com/SlicerMorph/S_2019/tree/master/Lecture04_SSA_2_SemiLMs)
5. [Lecture 5: Computational Anatomy (Maga)](https://github.com/SlicerMorph/S_2019/tree/master/Lecture05_Computational_Anatomy)
5. [Lecture 6: Applications of SSA: Modeling Growth (Mercan)](https://github.com/SlicerMorph/S_2019/tree/master/Lecture06_modeling_growth)
6. [Lecture 7: Auto3Dgm: Landmark-free Correspondence (Boyer)](https://github.com/SlicerMorph/S_2019/tree/master/Lecture07_Auto3Dgm)
7. [Lecture 8: Applications of SSA: Phylogenetics (Shan)](https://github.com/SlicerMorph/S_2019/tree/master/Lecture08_SSA_Phylogenetics)
8. [Lecture 9: Machine Learning Basics (Mercan)](https://github.com/SlicerMorph/S_2019/tree/master/Lecture09_MachineLearning)

