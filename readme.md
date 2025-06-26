# OpenAPI Tutorial

## Env Setup
### 1. install miniconda

### 2. install vscode

### 3. create conda env

### 4. install jupyter with vscode extension

### 5. vscode interpreter and terminal setting

## Connecting OpenAI API

### 1. Setup env file and library
```bash
pip install python-dotenv
or
conda install python-dotenv
```
```python
import os
from dotenv import load_dotenv

# .env file load
load_dotenv()

# get env variables
openai_api_key = os.getenv('OPENAI_API_KEY')
user_name = os.getenv('USER_NAME')

print(f'OpenAI User Key: {openai_api_key}')
print(f'User name: {user_name}')
```

### 2. Install openai package
```bash
pip install openai
or 
conda install openai
```
```python
import openai
print(f'Openai version: {openai.__version__}')
```

#### 2.1 Using openai.OpenAI Class
```python
from openai import OpenAI
client = OpenAI(api_key=openai_api_key)

messages = [{
    'role': 'user',
    'content': 'Tell me about python'
}]

response = client.chat.completions.create(
    model='gpt-4o',
    messages=messages
)

ass_reply = response.choices[0].message.content
print(f'AI Response:{ass_reply}')
```
#### 2.2 Streaming Response
```python
messages = [{
    'role': 'user',
    'content': 'type line one by one'
}]

res_stream = client.chat.completions.create(
    model='gpt-4o',
    messages=messages,
    stream=True
)

print('Real-time Response', end='')
for chunk in res_stream:
    chuck_message = chunk.choices[0].delta.content
    if chunk_message is not None:
        print(chunk_message, end="")
```

#### 2.3 Other Response
```python
import openai

try:
    response = client.chat.completions.create(
        model="gpt-4o",
        messages=[{"role": "user", "content": "지금 이 질문에 대해 RateLimitError 에러를 일으켜볼 수 있을까요?"}],
        timeout=5  # 5초 내에 응답이 없으면 타임아웃 발생 (예시)
    )
    print(response.choices[0].message.content)
except openai.error.RateLimitError:
    print("요청량 제한을 초과했습니다. 잠시 후 다시 시도하세요.")
except openai.error.OpenAIError as e:
    # 기타 모든 OpenAI 오류 처리
    print(f"OpenAI API 오류 발생: {e}")
```

### 3. Setting Google MAP API
#### 3.1 dwngcp2505 api key from 
> https://cloud.google.com/maps-platform/
#### 3.2 install googlemaps package
```bash
pip install googlemaps
or 
conda install googlemaps
```
```python
import os
from dotenv import load_dotenv
import googlemaps

load_dotenv()

# get env variables
googlemap_api_key = os.getenv('GOOGLEMAP_API_KEY')

user_Key=googlemap_api_key

gmaps=googlemaps.Client(key=user_Key)

sch=input("Type address: ")
ggmp=gmaps.geocode(sch, language='ko')
ggmp
```

### 4. public data portal
```bash
http://apis.data.go.kr/B551011/KorService2/searchFestival2
?serviceKey=
WhFq%2FSWL14dImzWTiqZ7%2FeYhqt%2B%2FQqPY0MAZ4VjkxEgVFJ2ELGNA6qjvCN8FPtCDFQ2MAFLbbffwfbjdL9dNAg%3D%3D
&numOfRows=10
&pageNo=1
&MobileOS=ETC
&MobileApp=AppTest
&_type=json
&arrange=C
&eventStartDate=20250101
&eventEndDate=20251231
```
#### 4.1 install pandas
```bash
conda install pandas
or
pip install pandas
```
```python
import os
from dotenv import load_dotenv
load_dotenv()
# get env variables
openapi_data_key = os.getenv('OPENAPI_DATA_KEY')
print(f'OpenAPI Data Key: {openapi_data_key}')
```
define request url
```python
rows = 258

url = 'http://apis.data.go.kr/B551011/KorService2/searchFestival2?serviceKey=' # basic url with serviceKey 
url += openapi_data_key
url += '&pageNo=1&MobileOS=ETC&MobileApp=AppTest&_type=json&arrange=C'
url += '&numOfRows=' + str(rows) # The number of data in 1 page
url += '&eventStartDate=' + str(20250101) 
url += '&eventEndDate=' + str(20251231)
```
requests data
```python
import requests
import json
import pandas as pd

res = requests.get(url)

if res.status_code == 200:
    data = res.json()
    print(data)
else:
    print('failed')
```
access information in detail
```python
data['response']['body']['items']['item'][0]['title']
len(data['response']['body']['items']['item'])
```
json to dataframe
```python
df = pd.DataFrame(data['response']['body']['items']['item'])
df.columns
```
```bash
# Output
Index(['addr1', 'addr2', 'zipcode', 'cat1', 'cat2', 'cat3', 'contentid',
       'contenttypeid', 'createdtime', 'eventstartdate', 'eventenddate',
       'firstimage', 'firstimage2', 'cpyrhtDivCd', 'mapx', 'mapy', 'mlevel',
       'modifiedtime', 'areacode', 'sigungucode', 'tel', 'title',
       'lDongRegnCd', 'lDongSignguCd', 'lclsSystm1', 'lclsSystm2',
       'lclsSystm3', 'progresstype', 'festivaltype'],
      dtype='object')
```
```
df1=df[['title', 'addr1', 'addr2', 'tel', 'mapx', 'mapy', 'eventstartdate', 'eventenddate']]
df1
```


### 5. crawling with beautifulsoup package
```python
import requests
from bs4 import BeautifulSoup as bs

html = requests.get('https://news.naver.com/section/102')
# html.text
soup = bs(html.text, 'html.parser')
print(soup)
```