# House-Price-Data-Analyst
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

Để kiểm tra mỗi trường dữ liệu có bao nhiêu giá trị NULL, ta thực hiện:
```
data2.isnull().sum()
```
![image](https://user-images.githubusercontent.com/92925089/201569854-b6ff63db-a001-475c-9c09-ed457256e496.png)

Với các data có giá trị Null như này, ta sẽ bỏ đi vì nếu có sẽ ảnh hưởng đến việc phân tích dữ liệu 
```
data3 = data2.dropna()
data3.isnull().sum()
```
### Data Cleaning : Thêm trường BHK
Từ trường size ban đầu, ta tách ra thêm trường BHK thể hiện số phòng của căn nhà đó:
```
data3['bhk']=data3['size'].apply(lambda x : int(x.split(' ')[0]))
```
![image](https://user-images.githubusercontent.com/92925089/201571262-7a4ed471-5d91-4413-a539-caa1f196be07.png)

### Data Cleaning : Xử lý trường dữ liệu total_sqft
Trường dữ liệu total_sqft thể hiện tổng số feet vuông của căn nhà. Do các dữ liệu trong trường này đang ở dạng String nên ta phải thực hiện ép kiểu để các dữ liệu trở thành dạng float. Tuy nhiên không phải dữ liệu nào cũng có thể ép kiểu về dạng float đơn thuần bằng hàm float() được. Để kiểm tra, ta viết một làm là is_float:
```
def is_float(x):
    try:
        float(x)
    except:
        return False
    return True
```
Tìm các row data có total_sqft không thể ép kiểu float được 
```
data3[~data3['total_sqft'].apply(is_float)].head(10)
```
![image](https://user-images.githubusercontent.com/92925089/201572162-ef8301a1-5efc-418b-a93e-fcb8119359cf.png)
Đối với dữ liệu như vậy, ta khai báo hàm convert_sqft_to_num nhận vào 1 đối số là x tức giá trị để có thể ép kiểu thành dạng float. Với các dữ liệu "A-B" ta sẽ thực hiện float((A+B)/2)
```
import re
def convert_sqft_to_num(x):
    arr = x.split('-')
    if(len(arr)==2):
        return (float(arr[0]) + float(arr[1]))/2
    try :
        return float(re.sub("[a-zA-Z]", "", x))
    except:
        return to_squareRoot(x)
```
Tuy nhiên vẫn còn những dữ liệu bao gồm cả các ký tự số và chữ xen lẫn nhau : 
![image](https://user-images.githubusercontent.com/92925089/201572909-1442c457-a824-43b8-a36f-a9f5db67efb3.png)
Với các dữ liệu này, ta khai báo và thực hiện hàm to_squareRoot, sử dùng methot split để chia dữ liệu làm 2 phần dữ liệu kiểu số và kiểu ký tự chữ. Các dữ liệu có đơn vị Yards ta sẽ nhân với 9, dũ liệu có đơn vị Meter ta sẽ nhân với 10.76:
```
def to_squareRoot(x) : 
    arr = x.split('Sq. ')
    if(arr[1] == "Yards"):
        return float(arr[0])*9
    elif (arr[1] == "Meter"):
        return float(arr[0]) * 10.76
to_squareRoot("142.84Sq. Meter")
```
Ta sẽ thu được dữ liệu như sau : 
![image](https://user-images.githubusercontent.com/92925089/201573781-090935d7-d9e8-4e0c-9db2-aa69c6f91931.png)
