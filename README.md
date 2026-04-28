# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
Date:28/4/26
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:
Import necessary libraries (NumPy, Matplotlib)

Load the dataset

Calculate the linear trend values using least square method

Calculate the polynomial trend values using least square method

End the program
### PROGRAM:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Load dataset
data = pd.read_csv('NFLX.csv', parse_dates=['Date'], index_col='Date')

# Resample yearly (average OPEN price)
resampled_data = data['Open'].resample('Y').mean().to_frame()

# Convert index to year
resampled_data.index = resampled_data.index.year
resampled_data.reset_index(inplace=True)
resampled_data.rename(columns={'Date': 'Year', 'Open': 'Value'}, inplace=True)

# Extract values
years = resampled_data['Year'].tolist()
values = resampled_data['Value'].tolist()
n = len(years)
```
### A - LINEAR TREND ESTIMATION
```
X = [i - years[n // 2] for i in years]
x2 = [i**2 for i in X]
xy = [i * j for i, j in zip(X, values)]

b = (n * sum(xy) - sum(values) * sum(X)) / (n * sum(x2) - (sum(X)**2))
a = (sum(values) - b * sum(X)) / n

linear_trend = [a + b * x for x in X]
```
### B- POLYNOMIAL TREND ESTIMATION
```
x3 = [i**3 for i in X]
x4 = [i**4 for i in X]
x2y = [i * j for i, j in zip(x2, values)]

coeff = [[n, sum(X), sum(x2)],
         [sum(X), sum(x2), sum(x3)],
         [sum(x2), sum(x3), sum(x4)]]

Y = [sum(values), sum(xy), sum(x2y)]

a_poly, b_poly, c_poly = np.linalg.solve(coeff, Y)

poly_trend = [a_poly + b_poly * x + c_poly * (x**2) for x in X]
```
```
# -------- Print --------
print("Trend Equations:")
print(f"Linear Trend: y = {a:.2f} + {b:.2f}x")
print(f"Polynomial Trend: y = {a_poly:.2f} + {b_poly:.2f}x + {c_poly:.2f}x^2")

# -------- Plot 1: Linear --------
plt.figure()

plt.plot(years, values, marker='o', label='Actual (Open)')
plt.plot(years, linear_trend, linestyle='--', label='Linear Trend')

plt.xlabel('Year')
plt.ylabel('Open Price')
plt.title('Linear Trend Estimation (Open Price)')
plt.legend()

plt.show()

# -------- Plot 2: Polynomial --------
plt.figure()

plt.plot(years, values, marker='o', label='Actual (Open)')
plt.plot(years, poly_trend, linestyle='--', label='Polynomial Trend')

plt.xlabel('Year')
plt.ylabel('Open Price')
plt.title('Polynomial Estimation (Open Price)')
plt.legend()

plt.show()
```
### OUTPUT

### TREND EQUATIONS

<img width="863" height="135" alt="image" src="https://github.com/user-attachments/assets/ef26fbd0-2924-401b-a924-bfa6ebf2f3f2" />

### A - LINEAR TREND ESTIMATION

<img width="903" height="663" alt="image" src="https://github.com/user-attachments/assets/935a61c1-54bd-4885-99f6-405d00b6f976" />

### B- POLYNOMIAL TREND ESTIMATION

<img width="858" height="686" alt="image" src="https://github.com/user-attachments/assets/8a5b89df-2e4f-4544-8ff8-5cbda6519289" />

### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
