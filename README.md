## Cloning repository and downloading chest X-ray images
 
 1. Clone and enter this repository:
         git clone https://github.com/Ashisme91/Computer-Aided_Diagnosis_of_Lung_Disease.git   
         
 2. Download [Resized Chest X-Ray Images](https://www.kaggle.com/khanfashee/nih-chest-x-ray-14-224x224-resized?select=images-224) and place it in the `data` folder.


## Introduction 

According to the National Centre for Health Statistics, there are more than 40 million cases of asthma or chronic obstructive pulmonary disease (COPD) in the United States (U.S.). Around 12 million people have not yet been diagnosed with COPD [1][2]. The costs associated with such common respiratory diseases include healthcare costs, productivity loss, higher insurance premiums and higher tax expenses. Based on historical estimates from National Institutes of Health, National Heart, Lung, and Blood Institute (NHLBI), the annual health care expenditures for asthma alone are estimated at $20.7 billion [3][4]. Timely and accurate, detection, intervention and treatment of respiratory diseases not only can help to improve patient's outcomes, it can also help to reduce the cost to individuals and the society. 

Chest X-ray (CXR) exam is one of the most common and cost-effective diagnostic tools in diagnosing lung diseases. It can be used to detect cancer, infection or air collecting in the space around a lung. It can also show chronic lung conditions, such as emphysema or cystic fibrosis, as well as complications related to these conditions. However clinical diagnosis of chest X-ray can be challenging, and sometimes believed to be harder than diagnosis via chest CT imaging. Even some promising work have been reported in the past, and especially in recent deep learning work on Tuberculosis (TB) classification. Achieving clinically relevant computer-aided detection and diagnosis (CAD) in real world medical sites on all data settings of CXR is still a very challenging, if not impossible feat when only several thousands of images are employed for study. This is evident from  where the performance deep neural networks for thorax disease recognition is severely limited by the availability of only 4,143 frontal view images (Openi is the previous largest publicly available chest X-ray dataset to date). Although CAD has been used in clinical environments for over 40 years, CAD usually does not substitute the doctor or other professional, but rather plays a supporting role. A radiologist is generally responsible for the final interpretation of a medical image and closing the report.

## Objectives

In this project, I aim to achieve the following:

1. Build a convolutional neural network (CNN) model to perform a multi-label classification of thoracic pathology using CXR images and,
2. Evaluate its performance against transfer learning approaches such as ResNet50 and MobileNetV2 models with metrics such as macro-average recall and F1 score.


## Methodology

The original dataset is extracted from the clinical Picture Archiving and Communcation System (PACS) database at National Institutes of Health Clinical Center in the United States (U.S.). It consists of 112,120 of all frontal chest x-ray images of 30,085 unique patients in the hospital. Compared to other publicly available datasets, this dataset is significantly more representative to the real patient population distributions and realistic clinical diagnosis challenges in the U.S. Fourteen disease image labels (where each image can have multi-labels) were mined from the associated radiological reports using natural language processing. The labels represent common thoracic pathologies and they include Atelectasis, Consolidation, Infiltration, Pneumothorax, Edema, Emphysema, Fibrosis, Effusion, Pneumonia, Pleural_thickening, Cardiomegaly, Nodule, Mass and Hernia. 

Image labels with less than 1,000 counts were removed.CXR Images are divided into train/validation and testing by patient level. All studies from the same patient will only appear in either training/validation or testing set. Lablels with normal results (i.e. ‘No Finding’) are removed to reduce sparseness of the target matrix. The final train-validation dataset contains 36,023 samples with thirteen labels, less Hernia.

## Model Evaluation

Among the three models, ResNet50 transfer learning model suffered the most from overfitting as illustrated by the divergence between the training and validation loss in subsequent epochs of training. This suggested that it has learned the training data too well, including noise or random fluctuations. The overfitting is expected as ResNet50 is a relatively large model. The number of layers or parameters are a few more times greater than the CNN model I built and the MobileNetV2 transfer learning model. 

Macro-averaging is a single performance indicator obtained by averaging the score of individual classes. Macro-averaging is preferred over micro-averaging in this case of imbalanced classes because it weighs each of the classes equally and is not influenced by the number of examples of each class. I evaluated my models against macro-average recall and F1 score. In this case of a multi-label classification of thoracic pathology, I believe that the algorithm's ability to detect positive in any of the thirteen labels would help to alert or draw immediate attention from phyisicians on any abnormal thorax findings in a patient's CXR study. In another way, this can also help reporting physicians to better prioritize the order in which they should close the CXR report and intervene more timely, if necessary. Despite ResNet50 being the most overfited model, it yielded the highest macro-average recall and F1 score of 0.32 and 0.31 respectively. The poor performances yielded by all three models are not completely unexpected due to the following few reasons. One, the degree of underrepresented labels are consistently high across the thirteen labels. Two, the spatial dimensions of a CXR image are usually 2,000 by 3,000 pixels. A lot of details and information are lost when the CXR images are resized to 224 by 224 pixels.

## Conclusion 

With greater access to a graphic processing unit, my next step would be to fine-tune existing models and train them with CXR images in their dimensions. I would also like to address the issue of imbalanced labels by applying over-sampling technique such as Variational Autoencoders (VAE), Synthetic Minority Over-Sampling Technique (SMOTE) or Modified Synthetic Minority (MSMOTE). Collecting more CXR images of underrepresented labels is another viable way to address the issue of imbalanced labels but this take more time. Should there be signicant improvement to the performance of the models in future works, such computer-aided diagnosis can be extended to other medical specialties like Orthopaedics and Cardiology, where the use of X-Ray images to detect disease/defect is very common. In the context of Singapore, there still exists a gap between research and application of such deep learning algorithms in the field of computer-aided diagnosis. To overcome adoption barriers such as medicolegal, regulatory and patient-safety issues, it is imperative to design model with patient-safety at the core and test it on real-life cases in a safe and controlled environment.

#### References

[1] Bloom B, Jones LI, Freeman G. Summary health statistics for U.S. children: National Health Interview Survey, 2012. National Center for Health Statistics. Vital Health Stat 10(258). 2013.

[2] Blackwell DL, Lucas JW, Clarke TC. Summary health statistics for U.S. adults: National Health Interview Survey, 2012. National Center for Health Statistics. Vital Health Stat 10(260). 2014.

[3] National Institutes of Health, National Heart, Lung, and Blood Institute (NHLBI). Morbidity and mortality: 2012 chart book on cardiovascular, lung and blood diseases. Bethesda, MD: NHLBI; 2012 Feb. Available from: https://www.nhlbi.nih.gov/files/docs/research/2012_ChartBook_508.pdf

[4] National Institutes of Health, National Heart, Lung, and Blood Institute (NHLBI). Morbidity and mortality: 2009 chart book on cardiovascular, lung and blood diseases. Bethesda, MD: NHLBI; 2009 Oct. Available from: http://www.nhlbi.nih.gov/resources/docs/cht-book.htm

[5] Xiaosong Wang, Yifan Peng, Le Lu, Zhiyong Lu, Mohammadhadi Bagheri, Ronald Summers, ChestX-ray8: Hospital-scale Chest X-ray Database and Benchmarks on Weakly-Supervised Classification and Localization of Common Thorax Diseases, IEEE CVPR, pp. 3462-3471, 2017

[6] Hoo-chang Shin, Kirk Roberts, Le Lu, Dina Demner-Fushman, Jianhua Yao, Ronald M. Summers, Learning to Read Chest X-Rays: Recurrent Neural Cascade Model for Automated Image Annotation, IEEE CVPR, pp. 2497-2506, 2016

[7] Open-i: An open access biomedical search engine. https: //openi.nlm.nih.gov
