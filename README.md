# Auto-Arborist Dataset - Cleaned
This goal of this respository is to introduce a pipeline to clean the auto-arborist dataset, i.e., ----
The complete dataset can be downloaded from:
- [The Auto Arborist Dataset: A Large-Scale Benchmark for Multiview Urban
Forest Monitoring Under Domain Shift](https://openaccess.thecvf.com/content/CVPR2022/papers/Beery_The_Auto_Arborist_Dataset_A_Large-Scale_Benchmark_for_Multiview_Urban_CVPR_2022_paper.pdf). Here is the [Supplementary Link](https://openaccess.thecvf.com/content/CVPR2022/supplemental/Beery_The_Auto_Arborist_CVPR_2022_supplemental.pdf)

- The Auto Arborist dataset consists of over 2.5 million tree images spanning 344 genera. However, the dataset is highly imbalanced, with a skewed distribution of examples across genera. A few genera dominate the dataset, containing the majority of samples, while many others have fewer than 100 examples, posing a significant class imbalance challenge. This long-tailed distribution makes the dataset suitable for exploring solutions to class imbalance and few-shot learning problems.

Class Count Distribution   |  Class Count Distribution
:-------------------------:|:-------------------------:
![image](https://github.com/kalebmes/auto-arborist-cleaned/blob/main/class_distribution_plots/boulder-001_class_distribution.png) |  ![image](https://github.com/kalebmes/auto-arborist-cleaned/blob/main/class_distribution_plots/calgary-001_class_distribution.png)
![image](https://github.com/kalebmes/auto-arborist-cleaned/blob/main/class_distribution_plots/edmonton-001_class_distribution.png) |  ![image](https://github.com/kalebmes/auto-arborist-cleaned/blob/main/class_distribution_plots/sioux_falls-001_class_distribution.png)

Disclaimer: ....

## Major Challenges 
- Raw images, where trees have not been cropped, often include excessive background information and this can negatively impact model performance. 
- Prone to mislabeling --> The uncropped images often contain multiple trees, along with unnecessary background elements like grass, roads, and other surroundings. However, only one tree is meant to be singled out for each image. This creates a high likelihood of cropping the wrong tree from the detected bounding boxes, leading to mislabeling.
- Limited examples for certain classes --> Some tree genera are naturally rare or less documented in the regions where the dataset was collected. This is the cause for the lack of sufficient samples for those classes.
- Noisy meta data, i.e., targeting tree(s) within an image is (are) not well-indicated. The metadata intended to assist in identifying the focal tree within an image, such as center coordinates, is often inaccurate. For instance, the provided y-coordinate in the metadata defaults to the midpoint of the image height, which does not reliably indicate the actual location of the target tree.

To address this, our project focuses on developing a robust preprocessing pipeline that effectively isolates trees in the images, enhancing the dataset's usability and improving model accuracy.

- Technical Challenges --> fine-grained problem, i.e., very large intra-class variation, very small inter-class variation 
  
## Preprocessing Pipeline and Insights

Our data-cleaning pipeilne involves **three removal cases**, each helping in identifying the target trees corresponding to the annotated labels, resulting in a cleaner datasetcan for model training.

**Note that the sample images shown in this analysis are taken from the Sioux Falls city (Region Central).**

### Case I : Images w/o Bounding Boxes, i.e., No Tree Detected
In this stage, we removed images with no trees detected. These images were still included in the training and testing sets of the Auto Arborist dataset despite not containing any valid tree information. This lead to noticeable improvements in model performance in addition to improving the dataset's quality and consistency.

 Images w/o Bounding Boxes | Images w/o Bounding Boxes
:-------------------------:|:-------------------------:
<img src="https://github.com/kalebmes/auto-arborist-cleaned/blob/main/sample_images/tree_1680078374270988337_streetlevel_fraxinus_image_detected.jpeg" width="400" height="500"> | <img src="https://github.com/kalebmes/auto-arborist-cleaned/blob/main/sample_images/tree_13903392559040084811_streetlevel_betula_image_detected.jpeg" width="400" height="500">
<img src="https://github.com/kalebmes/auto-arborist-cleaned/blob/main/sample_images/tree_1350265536637838191_streetlevel_betula_image_detected.jpeg" width="400" height="500"> | <img src="https://github.com/kalebmes/auto-arborist-cleaned/blob/main/sample_images/tree_18196125659405314054_streetlevel_fraxinus_image_detected.jpeg" width="400" height="500">
<img src="https://github.com/kalebmes/auto-arborist-cleaned/blob/main/sample_images/tree_6015415117226351339_streetlevel_betula_image_detected.jpeg" width="400" height="500"> | <img src="https://github.com/kalebmes/auto-arborist-cleaned/blob/main/sample_images/tree_6773050066674462020_streetlevel_ulmus_image_detected.jpeg" width="400" height="500">


### Case II : Boundary-Sided Bounding Boxes
As a further data-cleaning step, we eliminated bounding boxes located at the extreme left or right edges of the images. These bounding boxes are less likely to contain the focus tree, as multiple trees are detected in each image, and the bounding box for the focus tree should ideally be centrally located. Since the dataset lacks reliable metadata for pinpointing the focus tree, we developed and implemented a heuristic-based filtering algorithm that refines bounding box selection by removing irrelevant or misleading bounding boxes. Specifically:

* Bounding boxes situated at the extreme left, right, top, or bottom edges of the image are filtered out, particularly if they are small in size compared to the image dimensions.
* For the remaining bounding boxes, we prioritize those closer to the center of the image or those meeting certain size thresholds.

Original Image   |  After Cleaning
:-------------------------:|:-------------------------:
![image](https://github.com/kalebmes/auto-arborist-cleaned/blob/main/sample_images/tree_12770208626947435342_streetlevel_acer_image_detected.jpeg) |  ![image](https://github.com/kalebmes/auto-arborist-cleaned/blob/main/sample_images/tree_12770208626947435342_streetlevel_acer_image_cropped_tol_0.1_eps_0.1_pad_0_veryclean.jpeg)
![image](https://github.com/kalebmes/auto-arborist-cleaned/blob/main/sample_images/tree_3675578478330370074_streetlevel_betula_image_detected.jpeg) |  ![image](https://github.com/kalebmes/auto-arborist-cleaned/blob/main/sample_images/tree_3675578478330370074_streetlevel_betula_image_cropped_tol_0.1_eps_0.1_pad_0_veryclean.jpeg)
![image](https://github.com/kalebmes/auto-arborist-cleaned/blob/main/sample_images/tree_2433178162496390065_streetlevel_caragana_image_detected.jpeg) |  ![image](https://github.com/kalebmes/auto-arborist-cleaned/blob/main/sample_images/tree_2433178162496390065_streetlevel_caragana_image_cropped_tol_0.1_eps_0.1_pad_0_veryclean.jpeg)

### Case III : Bounding Boxes w/o Central Point (Blue Dot)

## Further Analysis and Discussion
- The following figure illustrates how the class distribution remained unchanged after removing images without bounding boxes and performing further cleaning.

After Case I   |  After Case II
:-------------------------:|:-------------------------:
![image](https://github.com/kalebmes/auto-arborist-cleaned/blob/main/class_distribution_plots/denver-001_class_distribution.png) |  ![image](https://github.com/kalebmes/auto-arborist-cleaned/blob/main/class_distribution_plots/denver-001_processed_class_distribution.png)
![image](https://github.com/kalebmes/auto-arborist-cleaned/blob/main/class_distribution_plots/calgary-001_class_distribution.png) |  ![image](https://github.com/kalebmes/auto-arborist-cleaned/blob/main/class_distribution_plots/calgary-001_processed_class_distribution.png)
![image](https://github.com/kalebmes/auto-arborist-cleaned/blob/main/class_distribution_plots/edmonton-001_class_distribution.png) |  ![image](https://github.com/kalebmes/auto-arborist-cleaned/blob/main/class_distribution_plots/edmonton-001_processed_class_distribution.png)
![image](https://github.com/kalebmes/auto-arborist-cleaned/blob/main/class_distribution_plots/sioux_falls-001_class_distribution.png) |  ![image](https://github.com/kalebmes/auto-arborist-cleaned/blob/main/class_distribution_plots/sioux_falls-001_processed_class_distribution.png)

<font color="red">Maybe we have to include a table to summarize the number of images before and after our data-cleaning pipeline.</font>


## Experimental Results & Comparison
-  Similar to the original paper, we report the experimental results for ResNetIR-100 in this compilation, which includes only two cities in Region Central, i.e., Sioux Falls and Calgary. 
-  **Our goal is to reproduce the experimental results with only single-view images provided, alongside our cleaned dataset.**
-  Other technical insights of our proposed model and additional empirical findings will be disclosed in our manuscript.


## Our Manuscript
We are preparing our manuscript for journal submission. The archived version will be made publicly available once it is ready.

## Contact Information 
-  Cheng Yaw Low, MPI-SP, Germany, Email: cheng-yaw.low@mpi-sp.org
-  Kaleb Asfew, KAIST, South Korea
-  Meeyoung Cha, MPI-SP, Germany


