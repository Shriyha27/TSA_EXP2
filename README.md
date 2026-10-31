# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
Date: 31.10.2026
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


data = pd.read_csv('C:\\Users\\admin\\Downloads\\archive\\MLTempDataset1.csv')

print("Original Data:")
print(data.head())


data['Datetime'] = pd.to_datetime(data['Datetime'])

resampled_data = (
    data.groupby(data['Datetime'].dt.date)['Hourly_Temp']
    .mean()
    .reset_index()
)

resampled_data.columns = ['Date', 'Hourly_Temp']

print("\nDaily Average Temperature:")
print(resampled_data.head())


dates = pd.to_datetime(resampled_data['Date'])
temperature = resampled_data['Hourly_Temp'].tolist()


n = len(dates)
X = list(range(n))
X = [x - X[n // 2] for x in X]

x2 = [x**2 for x in X]
xy = [x * y for x, y in zip(X, temperature)]

denominator = n * sum(x2) - sum(X)**2

if denominator == 0:
    raise ValueError("Cannot calculate linear trend.")

b = (n * sum(xy) - sum(temperature) * sum(X)) / denominator
a = (sum(temperature) - b * sum(X)) / n

linear_trend = [a + b * x for x in X]


x3 = [x**3 for x in X]
x4 = [x**4 for x in X]
x2y = [x2[i] * temperature[i] for i in range(n)]

coeff = [
    [n, sum(X), sum(x2)],
    [sum(X), sum(x2), sum(x3)],
    [sum(x2), sum(x3), sum(x4)]
]

Y = [
    sum(temperature),
    sum(xy),
    sum(x2y)
]

solution = np.linalg.solve(
    np.array(coeff, dtype=float),
    np.array(Y, dtype=float)
)

a_poly, b_poly, c_poly = solution

poly_trend = [
    a_poly + b_poly * x + c_poly * x**2
    for x in X
]


print(f"\nLinear Trend: y = {a:.4f} + {b:.4f}x")

print(
    f"Polynomial Trend: y = {a_poly:.4f} "
    f"+ {b_poly:.4f}x + {c_poly:.6f}x²"
)


resampled_data['Linear Trend'] = linear_trend
resampled_data['Polynomial Trend'] = poly_trend


plt.figure(figsize=(12, 6))

plt.plot(
    dates, temperature,
    color='blue', marker='o',
    markersize=3, label='Actual Temperature'
)

plt.plot(
    dates, linear_trend,
    color='black', linestyle='--',
    label='Linear Trend'
)

plt.plot(
    dates, poly_trend,
    color='red',
    label='Polynomial Trend (Degree 2)'
)

plt.title('Temperature Data: Linear and Polynomial Trend')
plt.xlabel('Date')
plt.ylabel('Average Temperature')
plt.legend()
plt.grid(True)
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()


print("\nFinal Results:")
print(resampled_data)
```

### OUTPUT
<img width="1525" height="847" alt="image" src="https://github.com/user-attachments/assets/3e67765f-980c-4800-8ff4-6f55063f3dd1" />


### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
