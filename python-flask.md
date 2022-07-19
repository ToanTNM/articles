# Mục lục

[1. Setup](#1-setup)

- [1.1. Tạo file ```requirements.txt```](#11-tạo-file-requirementstxt)
  
- [1.2. Tạo environment cho project](#12-tạo-environment-cho-project)
  
[2. Tạo web app sử dụng Flask](#2-tạo-web-app-sử-dụng-flask)

- [2.1. Tạo file app.py](#21-tạo-file-apppy)  
- [2.2. Chạy app](#22-chạy-app)
- [2.3. Chạy app ở chế độ debug](#23-chạy-app-ở-chế-độ-debug)

[3. Chạy ứng dụng bằng terminal](#3-chạy-ứng-dụng-bằng-terminal)

[4. Debug](#4-debug)

[5. Tham khảo thêm](#5-tham-khảo-thêm)

# 1. Setup

## 1.1. Tạo file ```requirements.txt```

- ```requirements.txt``` là file chứa các thư viện cần thiết cho project

- Tạo file từ các thư viện đã cài đặt:

```pip
pip freeze > requirements.txt
```

- Cài đặt thư viện từ file ```requirements.txt``` (khi mới clone project):

```pip
pip install -r requirements.txt
```

## 1.2. Tạo environment cho project

- Tạo folder env

python2

```python
pip install --upgrade pip==20.3.4
pip install virtualenv && virtualenv .env
```

python3

```python
python -m venv env
```

- Chọn chương trình thông dịch: ```Ctrl+Shift+P``` > ```Python: Select Interpreter```

![alt](https://code.visualstudio.com/assets/docs/python/shared/command-palette.png)

Chọn file thông dịch trong folder env

![alt](https://code.visualstudio.com/assets/docs/python/shared/select-virtual-environment.png)

- Chạy lệnh sau để sử dụng các lệnh ```python```, ```pip``` trong env:

```ps
env\Scripts\activate
```

- Update ```pip``` trong env:

```bash
python -m pip install --upgrade pip
```

- Cài đặt ```flask```

```bask
pip install flask
```

# 2. Tạo web app sử dụng Flask

## 2.1. Tạo file app.py

```py
from flask import Flask
app = Flask(__name__)

@app.route("/")
def home():
    return "Hello, Flask!"
```

## 2.2. Chạy app

```py
python -m flask run
```

## 2.3. Chạy app ở chế độ debug

Chọn Run and Debug trên VS code, chọn ```create a launch.json file```, cấu hình ```Flask```. VS code sẽ tạo file ```launch.json```:

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Flask",
            "type": "python",
            "request": "launch",
            "module": "flask",
            "env": {
                "FLASK_APP": "app.py",
                "FLASK_ENV": "development",
                "FLASK_DEBUG": "0"
            },
            "args": [
                "run",
                "--no-debugger"
            ],
            "jinja": true
        }
    ]
}
```

Bấm ```F5``` để debug app

# 3. Chạy ứng dụng bằng terminal

On Linux and macOS, use ```export set FLASK_APP=webapp```; on Windows use ```set FLASK_APP=webapp```

```py
python -m flask run
```

Có thể lưu trữ các biến môi trường trong file ```.flaskenv``` như sau:

```py
pip3 install python-dotenv
```

File ```.flaskenv```

```config
FLASK_APP = myblog.py
```

# 4. Debug

Có các cách debug sau:

- Debug trực tiếp từ VS code: chọn view debug trên VScode, chọn ```create a launch.json file```, tạo file launch.json theo hướng dẫn

    ```json
    {
        "name": "Python: Flask",
        "type": "python",
        "request": "launch",
        "module": "flask",
        "env": {
            "FLASK_APP": "app.py",
            "FLASK_ENV": "development",
            "FLASK_DEBUG": "0"
        },
        "args": [
            "run",
            "--no-debugger",
            "--no-reload"
        ],
        "jinja": true
    },
    ```

- Debug từ bằng docker container
  
  - Mở ```Command Palette``` (```Ctrl+Shift+P``` hoặc ```F1```), chạy lệnh ```docker add ```
  
    ![alt](https://code.visualstudio.com/assets/docs/containers/quickstarts/python-add-python.png)

  Làm theo các bước hướng dẫn. Sau khi xong chọn chế độ debug bằng ```Docker: ...```

- Debug bằng docker-compose:

  - Tạo Dockerfile từ các bước như trên, nội dung file như sau:

    ```docker
    # For more information, please refer to https://aka.ms/vscode-docker-python

    FROM python:3.8-slim-buster as base

    WORKDIR /app
    COPY ./src /app

    # Install pip requirements
    COPY requirements.txt .
    RUN python -m pip install -r requirements.txt

    ENV FLASK_APP=webapp.py

    #########DEBUGGER###########

    FROM base as debug
    RUN pip install ptvsd

    # WORKDIR /app

    CMD python -m ptvsd --host 0.0.0.0 --port 5678 --wait --multiprocess -m flask run -h 0.0.0 -p 5000

    #########PROD###########
    FROM base as prod

    CMD flask run -h 0.0.0 -p 5000
    ```

# 5. Tham khảo thêm

[Flask Tutorial in Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-flask)

[Python in a container](https://code.visualstudio.com/docs/containers/quickstart-python)
