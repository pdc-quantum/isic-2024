#  Optimizing a mix of solutions

## 270th Place Bronze Medalist

## Context

- Business context: https://www.kaggle.com/competitions/isic-2024-challenge/overview

- Data context: https://www.kaggle.com/competitions/isic-2024-challenge/Data

## Overview of the Approach

-  As a cardiologist who recently worked on an AI model for detecting cardiomegakly on chest X-rays, I aborded this competition with a particular interest for the image prediction. But the performance was poor, and more, attempts by others and by me demonstrated that adding a in image model to a tabular model was most often counter-productive.
 
 -  However, in the course of the competition, I developed a relatively simple strategy to optimize the fusion of a image model (the "booster") with an already well performing ensemble model  (the "basis"). The principle was to use the entire public 2024 training dataset as a proxy for the secret private test dataset and to obtain mixing parameters by using Scipy optimize-minimize and RepeatedStratifiedKFold.
 
## Details of the submission

The study includes the following steps:

- Step 1:
  
  - Try Pytorch image models available in the tim library and train them on different datasets using data augmentation for the minority (cancer) and undersampling for the majority (not cancer).
 
  - Densenet 169 was chosen on the basis of smooth and soft training resulting in best AUC of .99 after 8 to 10 epochs.

  - Among the curated dataset, two options appeared better performing

    - "housblend rep4" : the minority includes four replica of 2024 cases and all the cases of years 2018, 2019 and 2020. The majority includes only year-2024 cases. The ratio minority/majority is 4 to 1.  

    - "rep14" :  the minority includes fourteen replica of 2024 cases and all the cases of years 2018, 2019 and 2020. The majority includes only year-2024 cases. The ratio minority/majority is 2 to 1.
   
- Step 2

  - Different published models with good pAUC scores were tested as basis for the different boosters:

          -
          -
 
  - For each booster and each basis, the prediction was obtained for the entire year-2024 dataset

  - Cross-validation was performed. Using 2 splits with arguments target 0 and target 1, five repeats and 42 as random-state, 10 experiments were obtained for eight differnet mixing formula with 2 to 6 parameters.
 
  -  Using the minimize function for the methods Powell, Nelder-Mead and BFGS, the mean maximal pauc-80 accross the experiments was calculated, allowing to select te best combination method/formula for each combination booster/basis. 
 
  



## “Sources” 

http://timm.fast.ai/

## Model training

## Other useful strategies or approaches

## Model inference

