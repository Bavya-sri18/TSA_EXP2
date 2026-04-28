# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION

### Date:  28.04.2026
## AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

## ALGORITHM:
Import necessary libraries (NumPy, Matplotlib)

Load the dataset

Calculate the linear trend values using least square method

Calculate the polynomial trend values using least square method

End the program
## PROGRAM:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

data = pd.read_csv('/content/VBF.csv', parse_dates=['Date'], index_col='Date')

data = data[['Close']]

resampled_data = data['Close'].resample('Y').mean().to_frame()

resampled_data.index = resampled_data.index.year
resampled_data.reset_index(inplace=True)
resampled_data.rename(columns={'Date': 'Year'}, inplace=True)

years = resampled_data['Year'].tolist()
values = resampled_data['Close'].tolist()

X = [i - years[len(years)//2] for i in years]
```
#### A - LINEAR TREND ESTIMATION
```python

x2 = [i**2 for i in X]
xy = [i*j for i,j in zip(X, values)]
n = len(X)

b = (n*sum(xy) - sum(values)*sum(X)) / (n*sum(x2) - (sum(X)**2))
a = (sum(values) - b*sum(X)) / n

linear_trend = [a + b*X[i] for i in range(n)]
```
#### B- POLYNOMIAL TREND ESTIMATION

```python

x3 = [i**3 for i in X]
x4 = [i**4 for i in X]
x2y = [i*j for i,j in zip(x2, values)]

coeff = [
    [len(X), sum(X), sum(x2)],
    [sum(X), sum(x2), sum(x3)],
    [sum(x2), sum(x3), sum(x4)]
]

Y = [sum(values), sum(xy), sum(x2y)]

A = np.array(coeff)
B = np.array(Y)

a_poly, b_poly, c_poly = np.linalg.solve(A, B)

poly_trend = [a_poly + b_poly*X[i] + c_poly*(X[i]**2) for i in range(n)]
```
#### LINEAR PLOT & POLYNOMIAL PLOT
``` python

plt.figure(figsize=(8,5))
plt.plot(years, values, marker='o', color='purple', label='Actual')
plt.plot(years, linear_trend, linestyle='--', color='black', label='Linear Trend')

plt.title("Linear Trend Estimation")
plt.xlabel("Year")
plt.ylabel("Close Price")
plt.legend()
plt.grid(True)
plt.show()


plt.figure(figsize=(8,5))
plt.plot(years, values, marker='o', color='maroon', label='Actual')
plt.plot(years, poly_trend, linestyle='--', color='black', label='Polynomial Trend')

plt.title("Polynomial Trend Estimation (Degree 2)")
plt.xlabel("Year")
plt.ylabel("Close Price")
plt.legend()
plt.grid(True)
plt.show()
```

### OUTPUT
A - LINEAR TREND ESTIMATION

<img width="905" height="585" alt="image" src="https://github.com/user-attachments/assets/8138d24f-5ff5-4c3c-84ef-2045da092a2f" />

B- POLYNOMIAL TREND ESTIMATION

<img width="913" height="598" alt="image" src="https://github.com/user-attachments/assets/4757a1db-afae-44be-8145-507b482eefd1" />


### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
