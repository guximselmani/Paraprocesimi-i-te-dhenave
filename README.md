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
bins = [60, 90, 130, 200]
labels = ['Low', 'Normal', 'High']
df['BLDS_Category'] = pd.cut(df['BLDS'], bins=bins, labels=labels, right=False)


# Display the DataFrame after discretization
print("DataFrame after Discretization:")
print(df)
```
![image](https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44323443/5053728e-331d-46ea-ae4f-077df535b3a4)


# Binarizimi i kolones se gjinise dhe DRK_YN
```
df_onehot = pd.get_dummies(df, columns=['sex', 'DRK_YN'], drop_first=True)
```
![image](https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44323443/d379fb39-a10e-41b0-8a27-2a78b1fb4a8a)


#Pjesa 2
# Detektimi i përjashtuesit
```
print(df_onehot.dtypes)
```

<img width="252" alt="Bildschirmfoto 2024-01-02 um 16 39 12" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/935a76f3-a6b8-4e6d-b29c-f587700796f9">

```
import pandas as pd
import seaborn as sns
from scipy.stats import zscore

# Select numeric columns
numeric_columns = df_onehot.select_dtypes(include=['float64', 'int64']).columns

#Mënyra e parë
# Calculate Z-scores for each numeric column
z_scores = np.abs(zscore(df_onehot[numeric_columns]))

# Define a threshold for outliers (e.g., 3 standard deviations)
threshold = 3

# Identify outliers
outliers = (z_scores > threshold).all(axis=1)

# Display outliers
outlier_rows = df_onehot[outliers]
print("Outliers:")
print(outlier_rows)

# Visualize outliers using a box plot
sns.boxplot(data=df_onehot[numeric_columns])

```
<img width="1160" alt="Bildschirmfoto 2024-01-02 um 16 40 52" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/a59d042b-3577-4b21-a1d8-6b007faeeaf1">

<img width="502" alt="Bildschirmfoto 2024-01-02 um 16 41 20" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/c5ccb6e9-b648-4792-8c0c-577ab35f7377">


```
#Mënyra e dytë
# Calculate IQR for each numeric column
Q1 = df_onehot[numeric_columns].quantile(0.25)
Q3 = df_onehot[numeric_columns].quantile(0.75)
IQR = Q3 - Q1

# Identify outliers using IQR
outliers_iqr = ((df_onehot[numeric_columns] < (Q1 - 1.5 * IQR)) | (df_onehot[numeric_columns] > (Q3 + 1.5 * IQR))).any(axis=1)

# Display outliers using IQR
outlier_rows_iqr = df_onehot[outliers_iqr]
print("Outliers using IQR:")
print(outlier_rows_iqr)

# Visualize outliers using a box plot with IQR
sns.boxplot(data=df_onehot[numeric_columns])
```

<img width="677" alt="Bildschirmfoto 2024-01-02 um 16 42 31" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/50890a33-03c9-4cc3-b1cc-70ee62fa4631">

<img width="540" alt="Bildschirmfoto 2024-01-02 um 16 42 59" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/570d32f9-bef2-49a1-b605-10056d67db0d">


# Before removing outliers
```
summary_before = df_onehot.describe()

# After removing outliers
# Assuming outliers_iqr is the boolean mask for outliers using IQR
df_cleaned = df_onehot[~outliers_iqr]

summary_after = df_cleaned.describe()

# Compare the two summaries
print("Before removing outliers:")
print(summary_before)
print("\nAfter removing outliers:")
print(summary_after)
```

<img width="611" alt="Bildschirmfoto 2024-01-02 um 16 44 22" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/67783b3a-a1b5-4f14-a496-858a4baad6e1">




```
import matplotlib.pyplot as plt

# Before removing outliers
plt.figure(figsize=(12, 6))
sns.histplot(data=df_onehot[numeric_columns], kde=True)
plt.title("Distribution before removing outliers")
plt.show()

```
#Mënjanimi i zbulimeve jo të sakta
```
# After removing outliers
plt.figure(figsize=(12, 6))
sns.histplot(data=df_cleaned[numeric_columns], kde=True)
plt.title("Distribution after removing outliers")
plt.show()
```

<img width="879" alt="Bildschirmfoto 2024-01-02 um 16 45 31" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/29865226-d8f7-45c4-ab59-53ade8cf9737">

<img width="879" alt="Bildschirmfoto 2024-01-02 um 16 45 57" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/65bbbcb1-da22-4fdf-894d-59d668010fb7">

```

 
```
# Eksplorimi i të dhënave
``` 
# Before removing outliers
plt.figure(figsize=(12, 6))
sns.boxplot(data=df_onehot[numeric_columns])
plt.title("Box plot before removing outliers")
plt.show()

# After removing outliers
plt.figure(figsize=(12, 6))
sns.boxplot(data=df_cleaned[numeric_columns])
plt.title("Box plot after removing outliers")
plt.show()
```
<img width="879" alt="Bildschirmfoto 2024-01-02 um 16 46 33" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/5c9d8a65-f8ac-46f9-8eb8-8f494b5cadf3">

<img width="879" alt="Bildschirmfoto 2024-01-02 um 16 46 51" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/7a32a8e7-f155-493d-a1cd-6623dd9fa5e1">

```
```
#Statistike përmbledhëse
```
#Matrix correlation
 
# Before removing outliers
correlation_before = df_onehot[numeric_columns].corr()

# After removing outliers
correlation_after = df_cleaned[numeric_columns].corr()
 
# Compare the two correlation matrices
print("Correlation matrix before removing outliers:")
print(correlation_before)
print("\nCorrelation matrix after removing outliers:")
print(correlation_after)

```
<img width="879" alt="Bildschirmfoto 2024-01-02 um 16 47 34" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/7a4ab76f-08ad-4f49-9c08-dd0477c86e29">


```
print(df_cleaned.columns)
```
<img width="790" alt="Bildschirmfoto 2024-01-02 um 16 48 11" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/ba4990ae-0859-441d-a993-bfd103c664b9">

```
from sklearn.linear_model import LinearRegression

```
#Analiza multivariante
```

model = LinearRegression()
X = df_cleaned[['BLDS', 'tot_chole','triglyceride',]]
y = df_cleaned['DRK_YN_Y']
model.fit(X, y)

# Shfaq koeficientët dhe vlerat e pritura
print("Koeficientët:", model.coef_)
print("Intercept:", model.intercept_)
```
<img width="790" alt="Bildschirmfoto 2024-01-02 um 16 48 50" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/031afa4d-aec8-472b-a7b6-f1376c87549f">


```
df_cleaned.info()
```
<img width="460" alt="Bildschirmfoto 2024-01-02 um 16 49 45" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/fb2a5b5b-03c3-4c2f-acc2-720f4e94a1b4">


# Paraqitja vizuale

```
# Vizualizimi

import matplotlib.pyplot as plt
df_cleaned['Age_Binned'].hist(bins=20, edgecolor='black')
plt.xlabel('Mosha')
plt.ylabel('Numri i Individeve')
plt.title('Histograma e Moshës')
plt.show()
```

<img width="506" alt="Bildschirmfoto 2024-01-02 um 16 50 53" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/33e33d02-ab0f-4981-85aa-3b9204846a60">


```
import seaborn as sns
sns.countplot(x='sex_Male', data=df_cleaned)
plt.xlabel('Gjinia')
plt.ylabel('Numri i Individeve')
plt.title('Shpërndarja e Të Dhënave sipas Gjinisë')
plt.show()
```

<img width="506" alt="Bildschirmfoto 2024-01-02 um 16 51 26" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/bd32ecdc-9401-442f-8890-69a07c66bd0d">

```
 
# Data Distribution

plt.figure(figsize=(20,20))

plt.subplot(1, 4, 1)    
df_cleaned['sex_Male'].value_counts().plot.pie(counterclock=False)
plt.title('Sex')

plt.subplot(1, 4, 2)    
df_cleaned['Age_Binned'].value_counts().plot.pie(counterclock=False)
plt.title('Age')

plt.subplot(1, 4, 3)   
labels = ['1, never', '2, used to smoke but quit', '3, still smoke']
df_cleaned['SMK_stat_type_cd'].value_counts().plot.pie(labels=labels, counterclock=False)
plt.title('Smoking')

plt.subplot(1, 4, 4)    
labels = ['Drinker, Y', 'Not Drinker, N']
df_cleaned['DRK_YN_Y'].value_counts().plot.pie(labels=labels, counterclock=False)
plt.title('Drinking')

plt.show()   

```
<img width="1260" alt="Bildschirmfoto 2024-01-02 um 16 52 12" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/e6329a32-2f1a-4fe8-ae3c-c72a106cc624">

```
Drinking_age = df_cleaned.groupby(['Age_Binned', 'DRK_YN_Y'], as_index=False).agg(n = ('Age_Binned', 'count'))
Drinking_age.head()
```
<img width="314" alt="Bildschirmfoto 2024-01-02 um 16 52 49" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/829d14fc-b85b-4b76-a39d-0c443a0e69b2">


```
import seaborn as sns
import matplotlib.pyplot as plt


sns.barplot(data=Smoking_Age, x='Age_Binned', y='n', hue='DRK_YN_Y', palette='bright')
plt.title('Smoking Status by Age Group and Drinking Status')
plt.xlabel('Age Group')
plt.ylabel('Count')
plt.show()
```

<img width="504" alt="Bildschirmfoto 2024-01-02 um 16 53 45" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/cce0e53d-538e-446c-bb56-0465420cdd76">


```
# sampling Drinking and Blood Index, 

body_sample = df_cleaned.sample(200)
body_Drinking_1h = body_sample[['SBP', 'DBP', 'BLDS', 'tot_chole', 'HDL_chole', 'LDL_chole', 'triglyceride', 'DRK_YN_Y']]

sns.pairplot(body_Drinking_1h, hue = "DRK_YN_Y", size = 3, palette = 'bright')

# Drinking No N, Yes Y
```
<img width="557" alt="Bildschirmfoto 2024-01-02 um 16 54 29" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/e28f077f-64e4-43b3-8867-874dc9e441c4">

```

# sampling Smoking and Blood Index 
body_sample = df_cleaned.sample(200)
body_Smoking_1h = body_sample[['SBP', 'DBP', 'BLDS', 'tot_chole', 'HDL_chole', 'LDL_chole', 'triglyceride', 'SMK_stat_type_cd']]

sns.pairplot(body_Smoking_1h, hue = "SMK_stat_type_cd", size = 3, palette = 'bright')

 ```
<img width="557" alt="Bildschirmfoto 2024-01-02 um 16 55 30" src="https://github.com/guximselmani/Paraprocesimi-i-te-dhenave/assets/44524736/eebdf93e-2122-472a-87b9-624d447cf405">

