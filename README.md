# Detecting the anomalous activity of a ship’s engine

## **Business context**
You are provided with a data set to identify anomalous activity in a ship’s engine functionality (Devabrat,  2022). Typically speaking, anomalies would make up a minority of the data points (i.e., about 1% to 5% of the data points would be anomalies).

The data set contains six important features continuously monitored to evaluate the engine's status as ‘good’ or ‘bad’. These features are:
- **Engine rpm (revolutions per minute):** A high rpm indicates the engine is operating at a higher speed than designed for prolonged periods, which can lead to overheating, excessive wear, and eventual failure. A low rpm could signal a lack of power, issues with fuel delivery, or internal mechanical problems.
- **Lubrication oil pressure:** Low lubrication oil pressure indicates insufficient lubrication, leading to increased friction, overheating, and engine damage. A high lubrication oil pressure could signal a blockage in the oil delivery system, potentially causing seal or gasket failure.
- **Fuel pressure:** High fuel pressure can cause poor engine performance and incomplete combustion, indicating fuel pump or filter issues. A low fuel pressure may result in excessive fuel consumption, poor emissions, or damage to the fuel injectors.
- **Coolant pressure:** Low coolant pressure indicates a potential leak in the cooling system or a coolant pump failure, risking engine overheating. A high coolant pressure could be a sign of a blockage in the cooling system or a failing head gasket, which can also lead to overheating.
- **Lubrication oil temperature:** High lubrication oil temperature suggests the oil is overheating, which can degrade its lubricating properties and lead to engine damage. A low lubrication oil temperature may indicate it is not reaching its optimal operating temperature, potentially causing inadequate lubrication.
- **Coolant temperature:** High coolant temperature signals overheating, which various issues, including a failed thermostat, coolant leak, or insufficient coolant flow can cause. A low coolant temperature could suggest the engine is not reaching its optimal operating temperature, affecting performance and efficiency.

Issues with engines could lead to engine malfunctions, potential safety hazards, and downtime (e.g. delayed deliveries), resulting in the breakdown of a ship’s overall functionality, consequently impacting the business, such as affecting revenue via failure to deliver goods. By predicting timely maintenance, the business aims to increase profit by reducing downtime, reducing safety risks for the crew, limiting fuel consumption, and increasing customer satisfaction through timely deliveries.

## Solution

Develop a robust anomaly detection system to protect the fleet by evaluating engine functionality. Therefore, we’ll explore the data and:
- employ preprocessing and feature engineering
- perform model training, anomaly detection, post-processing, and refinement.

![image](https://github.com/user-attachments/assets/2389ecda-16c4-4e6e-b75f-37df925d945f)

## Perform EDA
![image](https://github.com/user-attachments/assets/f6a627af-4d9a-48eb-af35-011bc1d030e3)

## Descriptive statistics
![image](https://github.com/user-attachments/assets/f76b742a-1d2e-4060-a299-70a71100723f)

## Visualise the data to determine the distribution and extreme values
### Histogram
![image](https://github.com/user-attachments/assets/6576ed67-a15d-422b-a8b5-9f305fbae51c)

### Boxplot
![image](https://github.com/user-attachments/assets/bcb9181f-eaa2-4073-8cc2-13da63679650)

## Anomaly detection with a statistical method: Interquartile Range (IQR)
### 2 features falling under an outlier category
![image](https://github.com/user-attachments/assets/08ca20bb-fdb8-4a42-8b92-7818dc46c33c)
### 3 features falling under an outlier category
![image](https://github.com/user-attachments/assets/cdb2c6fa-576b-498b-8e08-686cc7793be5)

## Interquartile Range (IQR) observations: 

The interquartile range (IQR) method has identified 11 samples as anomalies based on 3 features falling under an outlier category. There are no sample that has more than 3 outlying features. This is way below the expected number of anomalies that were expected as per the business insight of 1% to 5%.

Based on 2 or more features, IQR indicates 422 samples as anomalies. This falls within the expected range and therefore a more appropriate model if IRQ was to be adopted.

## Anomaly detection using one-class SVM
![image](https://github.com/user-attachments/assets/2dc0c38b-30a0-4714-b69f-a6f7aff3d04e)

## One-Class SVM oberservations:

Initially the features were Normalised (MinMaxScaler) as the data distribution of all features seemed skewed. Anomalies were then identified using one-class SVM using various parameter values for gamma and nu.

When nu was kept constant at .05 and gamma was changed, the number of anomalies did not change by much and kept around 980. This is around the 5% anomalies that business insight had provided.

When gamma was kept constant at .5 and nu was changed, the number of anomalies changed and predicted 199 anomalies at nu = 0.01. This is around the 1% anomalies that business insight had provided.

One-Class SVM is able to predict anomalies between 1% and 5% by varying the nu parameters.

**Note**: The normal data points and anomalies seem to overlap as the anomalies are predicted with all 6 features while it is plotted over 2 features deduced using PCA. Having said that the anomalies are mainly towards the edges.

## Anomaly detection using Isolation Forest
![image](https://github.com/user-attachments/assets/1d44a564-6e42-429e-8ced-85ff157f2e23)

## Isolation Forest oberservations:

Initially the features were Normalised (MinMaxScaler) as the data distribution of all features seemed skewed. Anomalies were then identified using Isolation Forest using various parameter values for contamination.

The n_estimators parameter was kept constant at 100 throughout the modelling.

When contamination was set at .05 the number of anomalies predicted was 977 matching the 5% anomalies that business insight had provided.

When contamination was set at .01 the number of anomalies predicted was 196 closer the 1% anomalies that business insight had provided.

Isolation Forest is able to predict anomalies between 1% and 5% with corresponding values of contamination parameter.

**Note**: The normal data points and anomalies seem to overlap as the anomalies are predicted with all 6 features while it is plotted over 2 features deduced using PCA.

# Conclusion

> **Descriptive statistics** did not show any oddity. Data was clean with no null values or duplicates. Mean and median seemed very close, but the visualisation (histogram) showed that the distribution was skewed. The box plot also showed that there were quite a lot of outliers on all features except **Coolant temp**. Based on the skewed distribution, data was scaled using MinMaxScaler to normalise them.

> **Interquartile range (IQR)** method identified 11 samples as anomalies based on 3 or more features falling under an outlier category and 422 as anomalies based on 2 or more features falling under an outlier category.

> Both **One-class SVM** and **Isolation Forest** models were able to predict anomalies in the range from 1% to 5%.

> All 3 methods predicted anomalies within the expected range. To make a definitive choice between the 3 models, their results will have to be verified by a domain expert and determined which of the 3 models made the most accurate predictions.

