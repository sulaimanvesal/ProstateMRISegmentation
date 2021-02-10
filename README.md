# ProstateMRISegmentation
Deep Learning-Based Model for Prostate-MRI segmentation

## Goal: Porstate Detection/Segmentation in MR Sequences

Prostate cancer is the second most common cause of cancer deaths in males and will afflict 1 in 6 men. Many diagnostic prostate biopsies fail to detect occult cancers because of the difficulty in properly determining the prostate’s anatomy. Approximately 20% of ultrasound image guided prostate biopsies return negative results despite rising levels of prostate-specific antigen (PSA) markers in the blood and the presence of cancer somewhere in the 30+ cc gland. These failures have been attributed by some to the hesitation to sample one of the two major parts of the glands— namely the predilection toward the more easily reached peripheral zone (PZ) over the less accessible central gland (CG). This year’s ISBI challenge contestants outlined these two non-overlapping adjacent regions of the gland (see image).
    
    
## Hypothesis:

-  Advancing solutions to this vexing clinical problem will raise awareness about the impact computer image processing can have toward addressing a widespread, critical healthcare puzzle. 


## Input:
Training and test data:
- A 60-case prostate MRI 3D series "training set" consisting of Digital Imaging and Communications in Medicine (DICOM) images with matched NRRD markups that define the tissue CG and PZ are available from the NCI via The Cancer Imaging Archive (TCIA).
- There will be 60 cases, each consisting of 15+ (4 mm thick slices at 3T) or 28+ (3 mm thick at 1.5T) adjacent axial cross-section cuts. 
- Half of them were obtained at 1.5 T (Philips Achieva) with an endo-rectal receiver coil (fromBostonMedicalCenter) and the other half at 3T (Siemens TIM) with a surface coil (from Radboud University Nijmegen Medical Centre [RUNMC],Netherlands)
- They were acquired as T2-weighted MR axial pulse sequences. Each file contains nearly all DICOM acquisition parameters except tags that specifically identify Private Health Information. 
- The 10-case prostate MRI 3D series "test set" is now available via direct download: http://challenge.kitware.com/midas/item/3046.


## Output:
- The final system shall take an image as input, and output either „AP“ or „lateral“ as image type.

---
# Method

To segment the prostate, we can think of two standard approaches.
- **One-stage - Direct segmentation:** after observation we noticed that in all volumes prostate located almost in the center of MR images. On the other hand for the model, we need a fixed-size input so to tackle this issue, what we could do is to apply **centr-cropping** and extract region-of-interest (RoIs) with a fixed-size for example 256x256.

- **Multi-stage - First detect then segment:** In this approach, we can run a very shallow auto-encoder to generate very rough segmentation of the input images in a smaller resolutions, then use the predicted output as a landarmark to corp the RoIs in the high-resolution images. This method is more clinically-relevant, since there is chances that the prostate located not in center of the MR images.


**Note:** for the sake of this challenge we employed the first apporach.

---
# Evaluation

**Dice coefficient**: Computes the Dice coefficient (also known as Sorensen index) between the binary objects in two images.

The metric is defined as : $$ DC=\frac{2|A∩B|}{|A|+|B|}$$
where $A$ is the ground-truth and $B$ is the model predictions.

**Hausdorff Distance**: Computes the (symmetric) Hausdorff Distance (HD) between the binary objects in two images. It is defined as the maximum surface distance between the objects.

---
## 1.4 Plot Random MRIs and Histogram
Before doing any preprocessing, let's see some stats about the images from random patients. 

- From histogram of the images, it can be seen that images have different range of values
- The contrast of MRI images are very low
- After overlaying the annotation masks on the images, still we can see that some of the annotations are not correct or shifted
- Since, our task is to detect prostate wihtin MRI, we transfomed the segmentation masks to binary after merging the AG and ER classes.

![Porstate-MR](https://github.com/sulaimanvesal/ProstateMRISegmentation/blob/main/images/sample.png)

---
## Inference

Ground-Truth (prostateDx_03_0001)            |  Prediction (prostateDx_03_0001)
:-------------------------:|:-------------------------:
 ![Porstate-MR](https://github.com/sulaimanvesal/ProstateMRISegmentation/blob/main/images/prostate3T_02_0003_gt.png)| ![Porstate-MR](https://github.com/sulaimanvesal/ProstateMRISegmentation/blob/main/images/prostate3T_02_0003_pred.png)
---
### Note: To re-run the notebook without python packaging problem, use ```enviroment.yml ``` file

Use the ```terminal/Anaconda Prompt/Juupyter Notebook``` for the following steps:

- Create the environment from the environment.yml file: ```conda env create -f environment.yml```

- The first line of the yml file sets the new environment's name. Here the name is: ```"pipleine_test"```

- Activate the new environment: ```conda activate pipleine_test```

- Verify that the new environment was installed correctly: ```conda env list```
