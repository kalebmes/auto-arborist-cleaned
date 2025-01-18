# Auto-Arborist Dataset - Cleaned
This goal of this respository is to introduce a pipeline to clean the auto-arborist dataset, i.e., ----
The complete dataset can be downloaded from:
- [The Auto Arborist Dataset: A Large-Scale Benchmark for Multiview Urban
Forest Monitoring Under Domain Shift](https://openaccess.thecvf.com/content/CVPR2022/papers/Beery_The_Auto_Arborist_Dataset_A_Large-Scale_Benchmark_for_Multiview_Urban_CVPR_2022_paper.pdf). Here is the [Supplementary Link](https://openaccess.thecvf.com/content/CVPR2022/supplemental/Beery_The_Auto_Arborist_CVPR_2022_supplemental.pdf)

- The Auto Arborist dataset consists of over 2.5 million tree images spanning 344 genera. However, the dataset is highly imbalanced, with a skewed distribution of examples across genera. A few genera dominate the dataset, containing the majority of samples, while many others have fewer than 100 examples, posing a significant class imbalance challenge. This long-tailed distribution makes the dataset suitable for exploring solutions to class imbalance and few-shot learning problems.

Class Count Distribution   |  Class Count Distribution
:-------------------------:|:-------------------------:
![image](https://github.com/user-attachments/assets/39d55e39-2fa4-496a-8cd7-c13341f9634e) |  ![image](https://github.com/user-attachments/assets/fd46770c-b843-4b34-a59b-3f55c34b1477)
![image](https://github.com/user-attachments/assets/6098357b-b330-4b63-9016-342a392ff513) |  ![image](https://github.com/user-attachments/assets/98cbb477-36d5-4d70-be87-92b46a29fcbe)

Disclaimer: ....

## Major Challenges 
- Raw images, where trees have not been cropped, often include excessive background information and this can negatively impact model performance. 
- Prone to mislabeling --> The uncropped images often contain multiple trees, along with unnecessary background elements like grass, roads, and other surroundings. However, only one tree is meant to be singled out for each image. This creates a high likelihood of cropping the wrong tree from the detected bounding boxes, leading to mislabeling.
- Limited examples for certain classes --> Some tree genera are naturally rare or less documented in the regions where the dataset was collected. This is the cause for the lack of sufficient samples for those classes.
- Noisy meta data, i.e., targeting tree(s) within an image is (are) not well-indicated. The metadata intended to assist in identifying the focal tree within an image, such as center coordinates, is often inaccurate. For instance, the provided y-coordinate in the metadata defaults to the midpoint of the image height, which does not reliably indicate the actual location of the target tree.
- Etc.

To address this, our project focuses on developing a robust preprocessing pipeline that effectively isolates trees in the images, enhancing the dataset's usability and improving model accuracy.

- Technical Challenges --> fine-grained problem, i.e., very large intra-class variation, very small inter-class variation 
  
## Proposed Pipeline and Insights

- Case I   : No Bounding Box
- Case II  : Boundary-Sided Bounding Boxes
- Case III : Minimal Alignment? 

## Further Analysis and Discussion
- Tables : Summarization of class distribution, before and after cleaning, etc. 

## Experimental Results & Comparison
- Backbones : ResNet (maybe), EfficientNet, ConvNext, RegNet

## Manuscript Compilation

## Contact Information 


