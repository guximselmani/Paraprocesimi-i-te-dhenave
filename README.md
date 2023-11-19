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


#Kualiteti i te dhenave
 -ACCURACY
 #Mosha
  import great_expectations as gx
  context = gx.get_context()
  validator = context.sources.pandas_default.read_csv(
    "./smoking_driking_dataset.csv"
)

<img width="706" alt="image" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/6869d473-394a-448d-ae07-fabdc0f35e0d">

#Gjinia

<img width="689" alt="image" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/ee3772ad-49e0-4517-ab56-f002401367d9">

#Drinks/Smokes Yes / No

<img width="689" alt="image" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/5209b8e7-4e69-4cee-bdb8-91e0a571ec8c">

#Drinks/Smokes State

<img width="689" alt="image" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/173825e0-37ba-4aaf-870f-0229660ab981">

 

 # DUPLICATION
duplicates = df[df.duplicated(keep='first')]
print("First occurrences of duplicates:")
print(duplicates)
 <img width="689" alt="image" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/9480e850-fc67-43d0-a4b8-d8d742aa97bb">

   # Removing duplicates
  cleaned_df = df.drop_duplicates()
  print(cleaned_df)
  <img width="689" alt="image" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/f72c7b85-87d7-41a3-a5da-7161fabc9fad">


# Completeness
#identifiko te dhenat null
print('te dhenat null')
df.isnull().sum()
 <img width="689" alt="image" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/e1fcd03d-4e9a-4413-b285-89291ae9cae9">


# Aggregation
#Ne baze te kolonave SMK_stat_type_cd dhe gjinise jan shfaq edhe te dhenat mesatare prej kolonave tot_chole, HDL_chole, LDL_chole, triglyceride 
```
agg_result = cleaned_df.groupby(['SMK_stat_type_cd', 'sex']).agg({
    'tot_chole': 'mean',
    'HDL_chole': 'mean',
    'LDL_chole': 'mean',
    'triglyceride': 'mean'
}).reset_index()

print("\nAggregated Result:")
print(agg_result)
```
<img width="764" alt="image" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44323443/75821de7-78b1-4cd9-b3ac-10f87d602c11">


# Diskretizimi
bins = [20, 40, 60, 80, 100]
labels = ['20-40', '40-60', '60-80', '80-100']
df['Age_Binned'] = pd.cut(df['age'], bins=bins, labels=labels, right=False)
 plt.figure(figsize=(10, 6))
plt.hist(df['age'], bins=bins, edgecolor='black', alpha=0.7)
plt.title('Age Distribution with Bins')
plt.xlabel('Age')
plt.ylabel('Frequency')
plt.show()
<img width="529" alt="Bildschirm­foto 2023-11-18 um 15 42 18" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/1183fefb-9d74-492b-82eb-bc37c1b05ad2">


# Fshirja e kolona qe nuk kemi qellim ti perdorim 
```
# Drop columns that we don't need
columns_to_drop = ['waistline', 'sight_left', 'sight_right', 'hear_left', 'hear_right', 'SGOT_AST', 'SGOT_ALT', 'gamma_GTP', 'serum_creatinine', 'urine_protein']
df = df.drop(columns=columns_to_drop)
```

# Binarizimi i kolones se gjinise dhe DRK_YN
```
df_onehot = pd.get_dummies(df, columns=['sex', 'DRK_YN'], drop_first=True)
```
