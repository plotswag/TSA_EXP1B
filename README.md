# Ex.No: 1B CONVERSION OF NON STATIONARY TO STATIONARY DATA
# Date: 19/08/2025

### AIM:
To perform regular differncing,seasonal adjustment and log transformatio on international airline passenger data
### ALGORITHM:
1. Import the required packages like pandas and numpy
2. Read the data using the pandas
3. Perform the data preprocessing if needed and apply regular differncing,seasonal adjustment,log transformation.
4. Plot the data according to need, before and after regular differncing,seasonal adjustment,log transformation.
5. Display the overall results.
### PROGRAM:
```
 #JEEVANESH S
 #212222243002
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.seasonal import seasonal_decompose

data = pd.read_csv('/content/cardekho.csv')
ts_data = data.groupby("year")["selling_price"].mean().reset_index()

ts_data["year"] = pd.to_datetime(ts_data["year"], format="%Y")
ts_data.set_index("year", inplace=True)

ts_data["selling_price"] = ts_data["selling_price"]

ts_data["price_diff"] = ts_data["selling_price"] - ts_data["selling_price"].shift(1)

period = 2 if len(ts_data) > 2 else 1
result = seasonal_decompose(ts_data["selling_price"], model="additive", period=period)
ts_data["price_sea_diff"] = result.resid

ts_data["price_log"] = np.log(ts_data["selling_price"])

ts_data["price_log_diff"] = ts_data["price_log"] - ts_data["price_log"].shift(1)

if ts_data["price_log_diff"].dropna().shape[0] > period:
    result_log = seasonal_decompose(ts_data["price_log_diff"].dropna(), model="additive", period=period)
    ts_data["price_log_seasonal_diff"] = result_log.resid
else:
    ts_data["price_log_seasonal_diff"] = np.nan

plt.figure(figsize=(16, 16))

plt.subplot(6, 1, 1)
plt.plot(ts_data["selling_price"], label="Original")
plt.legend(loc="best")
plt.title("Original Data (Average Selling Price by Year)")
plt.xlabel("Year")
plt.ylabel("Price")

plt.subplot(6, 1, 2)
plt.plot(ts_data["price_diff"], label="Regular Difference")
plt.legend(loc="best")
plt.title("Regular Differencing")
plt.xlabel("Year")
plt.ylabel("Differenced Price")

plt.subplot(6, 1, 3)
plt.plot(ts_data["price_sea_diff"], label="Seasonal Adjustment")
plt.legend(loc="best")
plt.title("Seasonal Adjustment")
plt.xlabel("Year")
plt.ylabel("Seasonally Adjusted Price")

plt.subplot(6, 1, 4)
plt.plot(ts_data["price_log"], label="Log Transformation")
plt.legend(loc="best")
plt.title("Log Transformation")
plt.xlabel("Year")
plt.ylabel("Log(Price)")

plt.subplot(6, 1, 5)
plt.plot(ts_data["price_log_diff"], label="Log Transformation and Regular Differencing")
plt.legend(loc="best")
plt.title("Log Transformation and Regular Differencing")
plt.xlabel("Year")
plt.ylabel("RDiff(Log(Price))")

plt.subplot(6, 1, 6)
plt.plot(ts_data["price_log_seasonal_diff"], label="Log + Regular + Seasonal Differencing")
plt.legend(loc="best")
plt.title("Log + Regular + Seasonal Differencing")
plt.xlabel("Year")
plt.ylabel("SDiff(RDiff(Log(Price)))")

plt.figure(figsize=(12, 6))
plt.plot(ts_data["selling_price"], label="selling_price")
plt.plot(ts_data["price_diff"], label="price_diff")
plt.plot(ts_data["price_sea_diff"], label="price_sea_diff")
plt.plot(ts_data["price_log"], label="price_log")
plt.plot(ts_data["price_log_diff"], label="price_log_diff")
plt.plot(ts_data["price_log_seasonal_diff"], label="price_log_seasonal_diff")

plt.legend(loc="best")
plt.title("Overall view:")
plt.xlabel("Year")
plt.ylabel("Values")
plt.show()


plt.tight_layout()
plt.show()

```
### OUTPUT:
<img width="1373" height="202" alt="image" src="https://github.com/user-attachments/assets/9740479c-66ca-4e53-91a1-ec182821fe45" />


REGULAR DIFFERENCING:
<img width="1406" height="238" alt="image" src="https://github.com/user-attachments/assets/440ae17c-f620-4d1e-8af7-93620effb84d" />
SEASONAL ADJUSTMENT:
<img width="1382" height="245" alt="image" src="https://github.com/user-attachments/assets/51cc050d-8f9d-48d5-8851-21c6d3c40d7f" />
LOG TRANSFORMATION:
<img width="1375" height="232" alt="image" src="https://github.com/user-attachments/assets/9b54deda-7f96-4d28-a13d-45e282a6ba0f" />
Overall:
<img width="1007" height="577" alt="image" src="https://github.com/user-attachments/assets/62df29cf-ead5-494c-974e-4e25214ed601" />
### RESULT:
Thus we have created the python code for the conversion of non stationary to stationary data on international airline passenger
data.
