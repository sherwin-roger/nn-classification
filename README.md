# Developing a Neural Network Classification Model

## AIM

To develop a neural network classification model for the given dataset.

## Problem Statement

An automobile company has plans to enter new markets with their existing products. After intensive market research, they’ve decided that the behavior of the new market is similar to their existing market.

In their existing market, the sales team has classified all customers into 4 segments (A, B, C, D ). Then, they performed segmented outreach and communication for a different segment of customers. This strategy has work exceptionally well for them. They plan to use the same strategy for the new markets.

You are required to help the manager to predict the right group of the new customers.

## DESIGN STEPS

### STEP 1:
Load the csv file and then use the preprocessing steps to clean the data

### STEP 2:
Split the data to training and testing

### STEP 3:
Train the data and then predict using Tensorflow

## PROGRAM

```
python3
import pandas as pd
import numpy as np

df=pd.read_csv("customers.csv")


df.head()

df=df.dropna(axis=0)

df["Segmentation"].unique()

import tensorflow as tf

x=df.drop(["Segmentation","Var_1","ID"],axis=1)
y=df[["Segmentation"]].values


df["Gender"].unique(),df["Ever_Married"].unique(),df["Graduated"].unique(),df["Profession"].unique(),df["Spending_Score"].unique()

from sklearn.preprocessing import OneHotEncoder

lr=OneHotEncoder()

lr.fit(y)

lr.categories_

y=lr.transform(y).toarray()

y

from sklearn.preprocessing import OrdinalEncoder

lst=[['Male', 'Female'],['No', 'Yes'],['No', 'Yes'],['Healthcare', 'Engineer', 'Lawyer', 'Artist', 'Doctor','Homemaker', 'Entertainment', 'Marketing', 'Executive'],['Low', 'High', 'Average']]

enc = OrdinalEncoder(categories=lst)

x[['Gender','Ever_Married','Graduated','Profession','Spending_Score']] = enc.fit_transform(x[['Gender',
                                                                 'Ever_Married',
                                                                 'Graduated','Profession',
                                                                 'Spending_Score']])

x=x.drop(["Age","Work_Experience","Family_Size"],axis=1)

x=x.values

from sklearn.model_selection import train_test_split

X_train,X_test,y_train,y_test=train_test_split(x,y,
                                               test_size=0.10,
                                               random_state=50)

model=tf.keras.Sequential([tf.keras.layers.Input(shape=(5,)),
                           tf.keras.layers.Dense(128,activation="relu"),
                           tf.keras.layers.Dense(64,activation="relu"),
                           tf.keras.layers.Dense(32,activation="relu"),
                           tf.keras.layers.Dense(4,activation="softmax")])

model.compile(optimizer=tf.keras.optimizers.Adam(),
                 loss=tf.keras.losses.CategoricalCrossentropy(),
                 metrics=['accuracy'])

model.fit(X_train,y_train,epochs=20,validation_data=(X_test,y_test),batch_size=32)

pd.DataFrame(model.history.history).plot()

y_preds=tf.argmax(model.predict(X_test),axis=1)

import sklearn

sklearn.metrics.classification_report(tf.argmax(y_test,axis=1),y_preds)

sklearn.metrics.confusion_matrix(tf.argmax(y_test,axis=1),y_preds)

tf.argmax(model.predict([[0., 0., 0., 6., 0.]]),axis=1)
```

## Dataset Information

Include screenshot of the dataset

## OUTPUT

### Training Loss, Validation Loss Vs Iteration Plot

![image](https://user-images.githubusercontent.com/75235128/189539009-b7f6e85c-58d1-4d8c-9dd8-bb82ab656a98.png)

### Classification Report

![image](https://user-images.githubusercontent.com/75235128/189538996-10b2d542-0d45-4a6a-9d90-7554e5f44344.png)

### Confusion Matrix

![image](https://user-images.githubusercontent.com/75235128/189539048-985aa259-372d-4a5d-a080-c0fabe6cc9b2.png)

### New Sample Data Prediction

![image](https://user-images.githubusercontent.com/75235128/189539113-9ecd5103-043d-4baf-b376-f586cf00e662.png)

## RESULT

Thus a Neural Network Classification Model is created and executed successfully
