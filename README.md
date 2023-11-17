# Paraprocesimi-i-te-dhenave
Smoking and Drinking Dataset with body signal

 
#Mbledhja e të dhënave
 
Dataseti  është marrë nga https://www.kaggle.com/datasets/sooyoungher/smoking-drinking-dataset

Struktura e datasetit:
Numri i rreshtave: Ky dataset përmban rreth 1'000'000 rreshta.
Numri i atributeve: Ky dataset përmban 24 kolona:

1.gjinia male, female 
2.age  [years]  
3.height [cm]  
4.weight [kg]  
5.sight_left eyesight(left)  
6.sight_right eyesight(right)  
7.hear_left  1(normal), 2(abnormal)  
8.hear_right  1(normal), 2(abnormal)  
9.SBP Systolic (blood pressure) [mmHg]    
10.DBP Diastolic blood pressure[mmHg]    
11.BLDS (or FSG - fasting blood glucose)[mg/dL]  
12.tot_chole (total cholesterol) [mg/dL] 
13.HDL_chole HDL cholesterol[mg/dL]  
14.LDL_chole LDL cholesterol[mg/dL]   
15.triglyceride  [mg/dL]  
16.hemoglobin [g/dL]  
17.urine_protein  1(-), 2(+/-), 3(+1), 4(+2), 5(+3), 6(+4)  
18.serum_creatinine serum(blood) creatinine[mg/dL]  
19.SGOT_AST SGOT(Glutamate-oxaloacetate transaminase) 
20.AST(Aspartate transaminase)[IU/L]  
21.SGOT_ALT ALT(Alanine transaminase)[IU/L]    
22.gamma_GTP y-glutamyl transpeptidase[IU/L]  
23.SMK_stat_type_cd Smoking state, 1(never), 2(used to smoke but quit), 3(still smoke)
24.DRK_YN Drinker or Not


Librarite e nevojshme jane:
import pandas as pd
import numpy as np

Importimi i datasetit ështe bere me komanden:
df = pd.read_csv('./smoking_driking_dataset_Ver01.csv')

# shfaq te informatat per datasetin
# print('informatat e te dhenave')
# df.info()

![image](https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/a74fa5c7-4081-4d24-9c5e-6b3d0110b4ec)


#identifiko te dhenat null
# print('te dhenat null')
# df.isnull().sum()

Nuk ka vlera null

#get unique values for unit's used column 
df.SBP.unique()


