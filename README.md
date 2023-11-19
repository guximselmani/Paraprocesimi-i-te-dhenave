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

# shfaq te gjitha informatat per datasetin

<img width="1009" alt="image" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44323443/2de87fe0-166a-4f8e-9b64-a7a58655c7f4">

# Fshirja e kolona qe nuk kemi qellim ti perdorim 
```
# Drop columns that we don't need
columns_to_drop = ['waistline', 'sight_left', 'sight_right', 'hear_left', 'hear_right', 'SGOT_AST', 'SGOT_ALT', 'gamma_GTP', 'serum_creatinine', 'urine_protein']
df = df.drop(columns=columns_to_drop)
```


# identifiko te dhenat null
```
print('te dhenat null')
df.isnull().sum()
```

<img width="761" alt="image" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44323443/6cc8337c-0935-485f-9e78-66851a09b4fb">

Nuk ka vlera null


#Kualiteti i te dhenave
 -ACCURACY
 ```
 #Mosha
  import great_expectations as gx
  context = gx.get_context()
  validator = context.sources.pandas_default.read_csv(
    "./smoking_driking_dataset.csv"
  )
```

<img width="1198" alt="image" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44323443/3152daee-30fd-4d52-a603-dfc85e7ddf18">

#Gjinia

<img width="1133" alt="image" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44323443/9c07531e-5377-4f1e-9bf1-bbdd8ff559b1">

#Drinks/Smokes Yes / No

<img width="879" alt="image" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44323443/745947b3-4ecd-4531-84d3-ab8be13b209f">

#Drinks/Smokes State

<img width="1075" alt="image" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44323443/d7e0c0c4-3705-4807-b3c3-904676f7c81f">


 # DUPLICATION
 ```
duplicates = df[df.duplicated(keep='first')]
print("First occurrences of duplicates:")
print(duplicates)
```
![image](https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44323443/9c576460-f9ad-44dc-b8fa-46dc9b691016)

#Removing duplicates
```
cleaned_df = df.drop_duplicates()
print(cleaned_df)
```
![image](https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44323443/9addb2dc-15b0-4082-9750-c30a35dcd44f)

# Completeness
```
#identifiko te dhenat null
print('te dhenat null')
df.isnull().sum()
```
![image](https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44323443/b2f46f91-6c0c-40f1-861d-5757c5b57be3)


# Aggregation
#Ne baze te kolonave SMK_stat_type_cd dhe gjinise jane shfaqur edhe te dhenat mesatare prej kolonave tot_chole, HDL_chole, LDL_chole, triglyceride 
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
![image](https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44323443/740f5228-0569-4887-a054-c22a07b03434)


# Diskretizimi
```
bins = [20, 40, 60, 80, 100]
labels = ['20-40', '40-60', '60-80', '80-100']
df['Age_Binned'] = pd.cut(df['age'], bins=bins, labels=labels, right=False)
plt.figure(figsize=(10, 6))
plt.hist(df['age'], bins=bins, edgecolor='black', alpha=0.7)
plt.title('Age Distribution with Bins')
plt.xlabel('Age')
plt.ylabel('Frequency')
plt.show()
```
![image](https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44323443/3770865a-950e-4639-80a1-b035797dfc1d)

#gjithashtu edhe Blod presure kolonen
```
bins = [80, 90, 100, 110]
labels = ['Low', 'Normal', 'High']
cleaned_df['BLDS_Category'] = pd.cut(cleaned_df['BLDS'], bins=bins, labels=labels, right=False)


# Display the DataFrame after discretization
print("DataFrame after Discretization:")
print(cleaned_df)
```
![image](https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44323443/13e6e5d3-3c81-4705-ac97-d0945b083d73)

# Binarizimi i kolones se gjinise dhe DRK_YN
```
df_onehot = pd.get_dummies(df, columns=['sex', 'DRK_YN'], drop_first=True)
```
![image](https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44323443/d379fb39-a10e-41b0-8a27-2a78b1fb4a8a)

