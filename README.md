#  Boosting a Complex Solution with a New Image Solution

## 270th Place Bronze Medalist

## Context

- Business context: https://www.kaggle.com/competitions/isic-2024-challenge/overview

- Data context: https://www.kaggle.com/competitions/isic-2024-challenge/Data

## Overview of the Approach

-  As a cardiologist who recently worked on an AI model for detecting cardiomegaly on chest X-rays, I started this competition with a particular interest for the image prediction. But the performance was poor. Moreover, attempts by others and by me demonstrated that adding an image model to a tabular model was most often counter-productive.
 
 -  However, in the course of the competition, I developed a relatively simple strategy to optimize the fusion of an image model (the "booster") with an already well performing ensemble model (the "basis"). The principle was to use the entire public 2024 training dataset as a proxy for the secret private test dataset and to obtain mixing parameters by using Scipy optimize-minimize and RepeatedStratifiedKFold.
 
## Details of the submission

The study includes the following steps :

### Find a good booster and a good data curation:
  
  - Try Pytorch image models available in the tim library and train them on different datasets using data augmentation for the minority (cancer) and undersampling for the majority (not cancer).
 
  - Densenet 169 was chosen on the basis of smooth and soft training resulting in best AUC of .99 at least after 8 to 10 epochs.

  - Among the tried dataset curations, two options appeared better performing:

    - "houseblend rep4" : the minority includes four replica of 2024 cases and all the cases of years 2018, 2019 and 2020. The majority includes only year-2024 cases. The ratio minority/majority is 4 to 1.  

    - "rep14" :  the minority includes fourteen replica of 2024 cases and all the cases of years 2018, 2019 and 2020. The majority includes only year-2024 cases. The ratio minority/majority is 2 to 1.
   
### Establish and test a boost strategy

  - Different published models with good pAUC scores were used as basis for the different boosters:
    - https://www.kaggle.com/code/vyacheslavbolotin/isic-2024-only-tabular-data-new-features
    - https://www.kaggle.com/code/wajahat1064/skin-cancer-prediction-score-0-184
    - https://www.kaggle.com/code/vyacheslavbolotin/isic-2024-2-image-lines
   
Result using the latest of these notebooks are presented here.  
 
  - For each booster and each basis, the prediction was obtained for the entire year-2024 training dataset

  - Cross-validation was performed. Using 2 splits based on target 0 and target 1, five repeats and 42 as random-state, 10 experiments were obtained for eight mixing formulas.
 
  - Using the minimize function for the methods Powell, Nelder-Mead and BFGS, the mean maximal pauc-80 accross the experiments was calculated, allowing to select the best combination method/formula for each combination booster/basis. The Nelder-Mead method was dismissed because of convergence problems leading to poor scores.
 
  - Different booster/basis combination were submitted to the competition, each with three possible approach for the mixing parameters:
     
     - recalculate the mixing parameters using the entire year-2024 train-dataset predictions
     
     - use the mean parameter values observed in the CV process
     
     - use "guessed" parameter set: only to confirm a local peak that was revealed by serendipity (using incorrected parameter set from other booster/basis combination)

 ### Final submission and scores:

   After the final submission, it is now possible to evaluate the  performance of the mixing optimization strategy (Table). Seven experiments are presented, together with the baseline reference. 
   
   The following notebooks were selected on the basis of their rank in my local public score listing:

    - the "houseblend rep4" serendipity solution, which was ranked first of these seven models for the public score.
  
    - the "rep14" solution with recalculation of mixing parameters which was ranked second of these seven models for the public score.
  
   Thus, a first solution was chosen using the public testing set as "oracle", and the second on a rational basis and a favorable public testing.

   After the shake, three experiments  had  a private score of 169, all with the "houseblend rep4" model and for parameter sets adjusted by the present optimization method.

   This validates this strategy where cross-validation is performed on a playground made of the predictions of the entire year-2024 training dataset using the booster and the basis models.

   The serendipity solution with private score 0.168, for which a bronze medal was attributed, was classed below the "rational" solutions of the "houseblend rep4" model.

   The "rep14" solutions were relegated at the end of the list, but before the reference basis.
    

![Tableau final](https://github.com/user-attachments/assets/cd4d0941-7641-414b-acf5-8930045e2817)




## “Sources” 

http://timm.fast.ai/



