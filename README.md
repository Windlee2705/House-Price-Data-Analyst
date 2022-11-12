﻿# House-Price-Data-Analyst
## Cleaning data bằng Python
### Import thư viện, dữ liệu vào dự án
Ta sẽ thực hiện phân thích dữ liệu giá nhà ở Bengaluru từ file dữ liệu Bengaluru_House_Data.csv. Trước hết, ta cần làm sạch dữ liệu bằng ngôn ngữ lập trình Python. Để có thể làm sạch data, ta sử dụng các thư viện của Python như Pandas, Numpy và Matplotlib. Do đó trước hết chúng ta cần import các thư viện này vào dự án : 
``` 
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
%matplotlib inline
import matplotlib
matplotlib.rcParams["figure.figsize"] = (20,10)
```
Ta đọc dữ liệu từ file csv và đặt tên cho dữ liệu là data_house
```
data_house = pd.read_csv("E:/DA/data-sources/Bengaluru_House_Data.csv")    
data_house.head(10)
```
Khi đó, dữ liệu Data-House mà ta cần phân tích sẽ có dạng như : 
![image](https://user-images.githubusercontent.com/92925089/201486544-7daee33e-679f-4bc8-a806-ed8ab4277374.png)
bao gồm các trường dữ liệu area_type, availability, location, size, society, total_sqft, bath, balcony và price<br/>
Kiếm tra độ dài của dữ liệu : 
```
data_house.shape
```
Ta thấy kết quả trả ra (13320, 9) tức dữ liệu có tổng cộng 13320 hàng và 9 cột tương ứng vowius 9 trường như đã nêu trên
Để kiểm tra một cách tổng quát data của trường area_type, ta thực hiện 
```
data_house['area_type'].unique()
data_house['area_type'].value_counts()
```
Kết quả trả về như sau : 
![image](https://user-images.githubusercontent.com/92925089/201486793-91ba2da6-2c11-4984-a5f7-aa9f97d02ce3.png)

### Data Cleaning : Xử lý các giá trị NA
Kiểm tra các giá trị của data có Null hay không, ta thực hiện
```
data2.isnull()
```
Kết quả trả về False tức giá trị của trường data đó không phải NULL, True tức là giá trị của data đó là Null<br/>
![image](https://user-images.githubusercontent.com/92925089/201487270-da3b1252-33b7-4874-89a9-6852a888e3c3.png)
