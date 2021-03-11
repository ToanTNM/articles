# 1. Setup

1.1. Tạo file ```requirements.txt```

* ```requirements.txt``` là file chứa các thư viện cần thiết cho project

* Tạo file từ các thư viện đã cài đặt:

```pip
pip freeze > requirements.txt
```

* Cài đặt thư viện từ file ```requirements.txt``` (khi mới clone project):

```pip
pip install -r requirements.txt
```

1.2. Tạo environment cho project

* Tạo folder env

```py
python -m venv env
```

* Chọn chương trình thông dịch: ```Ctrl+Shift+P``` > ```Python: Select Interpreter```

![alt](https://code.visualstudio.com/assets/docs/python/shared/command-palette.png)

Chọn file thông dịch trong folder env

![alt](https://code.visualstudio.com/assets/docs/python/shared/select-virtual-environment.png)

* Chạy lệnh sau để sử dụng các lệnh ```python```, ```pip``` trong env:

```ps
env\Scripts\activate
```

* Update ```pip``` trong env:

```py
python -m pip install --upgrade pip
```

* Cài đặt ```flask```

```pip
pip install flask
```

# 2. Tạo web app sử dụng Flask

2.1. Tạo file app.py:

```py
from flask import Flask
app = Flask(__name__)

@app.route("/")
def home():
    return "Hello, Flask!"
```

2.2. Chạy app:

```py
python -m flask run
```

2.3. Chạy app ở chế độ debug
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
python -m flask run.
```

# 4. Tham khảo thêm

[Flask Tutorial in Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-flask)

[Python in a container](https://code.visualstudio.com/docs/containers/quickstart-python)
