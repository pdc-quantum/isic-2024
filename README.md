#  Optimizing a mix of solutions

## 270th Place Bronze Medalist

## Context

- Business context: https://www.kaggle.com/competitions/isic-2024-challenge/overview

- Data context: https://www.kaggle.com/competitions/isic-2024-challenge/Data

## Overview of the Approach

-  As a cardiologist who recently worked on an AI model for detecting cardiomegaly on chest X-rays, I aborded this competition with a particular interest for the image prediction. But the performance was poor, and more, attempts by others and by me demonstrated that adding an image model to a tabular model was most often counter-productive.
 
 -  However, in the course of the competition, I developed a relatively simple strategy to optimize the fusion of an image model (the "booster") with an already well performing ensemble model (the "basis"). The principle was to use the entire public 2024 training dataset as a proxy for the secret private test dataset and to obtain mixing parameters by using Scipy optimize-minimize and RepeatedStratifiedKFold.
 
## Details of the submission

The study includes the following steps :

- Find a good booster and a good data curation:
  
  - Try Pytorch image models available in the tim library and train them on different datasets using data augmentation for the minority (cancer) and undersampling for the majority (not cancer).
 
  - Densenet 169 was chosen on the basis of smooth and soft training resulting in best AUC of .99 after 8 to 10 epochs.

  - Among the curated dataset, two options appeared better performing:

    - "housblend rep4" : the minority includes four replica of 2024 cases and all the cases of years 2018, 2019 and 2020. The majority includes only year-2024 cases. The ratio minority/majority is 4 to 1.  

    - "rep14" :  the minority includes fourteen replica of 2024 cases and all the cases of years 2018, 2019 and 2020. The majority includes only year-2024 cases. The ratio minority/majority is 2 to 1.
   
- Establish and test a boost strategy

  - Different published models with good pAUC scores were used as basis for the different boosters:

          - model 1
          - model 2
 
  - For each booster and each basis, the prediction was obtained for the entire year-2024 dataset

  - Cross-validation was performed. Using 2 splits based on target 0 and target 1, five repeats and 42 as random-state, 10 experiments were obtained for eight differnet mixing formulas.
 
  - Using the minimize function for the methods Powell, Nelder-Mead and BFGS, the mean maximal pauc-80 accross the experiments was calculated, allowing to select te best combination method/formula for each combination booster/basis. 
 
  - Different booster/basis combination were submitted to the competition, each with three possible mixing parameter sets:
     
     - recalculate the mixing parameters using the entire year-2024 train-dataset predictions
     
     - use the mean parameter values observed in the CV process
     
     - use "guessed" parameter set: only to confirm a local peak when this peak was revealed by serendipity (incorrected set from other booster/basis combination)

 - Select the two admitted solutions:

   The following notebooks were selected on the basis of their rank in the public score listing:

    - the "houseblend r4" serendipity solution (knowing there was an overfit)
  
    - the "rep14" solution with recalculation of mixing parameters as it was ranked higher than the basis and than the corresponding solution without recalculation.
  
   Thus, one solution was choosen using the public tesing set as "oracle", and the second on a rational basis.

  - Analyse the results for the private test set:

 

    - 

   - 



## “Sources” 

http://timm.fast.ai/

## Model training

## Other useful strategies or approaches

## Model inference

