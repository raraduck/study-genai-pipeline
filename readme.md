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