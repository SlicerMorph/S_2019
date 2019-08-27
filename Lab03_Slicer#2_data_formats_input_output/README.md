# Lab 03 Slicer#2: Data formats, getting data from MorphoSource, saving
* image stacks vs volume files
* [Nearly Raw Raster Data: NRRD](http://teem.sourceforge.net/nrrd/format.html)
* [Neuroimaging Informatics Technology Initiative: Nifti](https://nifti.nimh.nih.gov/)
* Not all formats retain image spacing, orientation, coordinate transformation 

* [List of image formats supported by Slicer](https://www.slicer.org/wiki/Documentation/Nightly/SlicerApplication/SupportedDataFormat)
* [pixel coordinates vs physical coordinates](https://www.slicer.org/wiki/Coordinate_systems)

## Image Import Examples
* DICOM import example 
* [DICOM -> Nifti conversion](https://www.ncbi.nlm.nih.gov/pubmed/26945974)
  * [DCM2NIIX](https://github.com/rordenlab/dcm2niix/releases) is a commandline tool for convert DICOMs
  * [DicomBrowser](https://www.slicer.org/wiki/Documentation/Nightly/Modules/DICOM)
* Import non-DICOM image sequences
  * Examples from local data (load 
* sample data from MorphoSource (Try on your own!)

## Volume types:
* vector vs scalar volumes (VectorToScalar Module) 
* other repositories (DigiMorph, and other listed on SlicerMorph project site)
* through review of save dialog box and all options (including MRB)
* downsampling/supersampling (ResampleScalarVolume)
* CropVolume (interpolated vs voxel, resampling at the time of cropping)
* SimpleFilters->IntensityWindowingImageFIlter to adjust intensity ranges during data type conversion

* [Explanation of Save Dialog box options](https://www.slicer.org/wiki/Documentation/Nightly/SlicerApplication/SavingData)
