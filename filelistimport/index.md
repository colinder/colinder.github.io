# 폴더안에 있는 특정 파일 리스트 가져오기


​	

# 폴더 안에 있는 특정 파일 리스트 정리

> 어떤 폴더 안에 있는 특정 확장자명의 파일들의 리스트를 정리하고 싶을 때 사용합니다. 

​		

```python 
## 폴더안에 있는 .csv파일 리스트 가져오기

import os

path = '/경로/'
file_lists = os.listdir(path)
file_list_result = [file for file in file_lists if file.endswith('.csv')] ## 파일명 끝이 .csv인 경우
```

​	

이후 pandas DataFrame에 넣을 때

```python
import pandas as pd

df = pd.DataFrame()
for i in file_list_result:
    data = pd.read_csv(path + i)
    df = pd.concat([df,data])
```

​	

​	

​	

​	

