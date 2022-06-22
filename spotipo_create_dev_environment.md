
# 1. Cài đặt WSL

```cmd
wsl --install -d <DistroName>
```

Check distro

```cmd
wsl --list --online
```

```cmd
Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform
```

- Restart PC

Tải và cài đặt <https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi>

```cmd
wsl --set-default-version 2

wsl --set-version Ubuntu 2
```

Check version

```cmd
wsl -l -v
```

# 2. Cài đặt python

```bash
sudo apt-get update
sudo apt install python-minimal
```

Check lại version (2.7.12)
python --version

Cài đặt pip

```bash
sudo apt install python-pip
pip install --upgrade pip==20.3.4
```

Check lại version 20.3.4

```bash
pip --version
```

# 3. Cấu hình spotipo

```bash
cd /mnt/d/Dev/spotipo
code .
```

hoặc

```bash
code /mnt/d/Dev/spotipo
```

Ubuntu sẽ tự động cài đặt và chạy VScode

Cài đặt các package sau để tránh lỗi khi cài đặt các thư viện:
Lỗi EnvironmentError: mysql_config not found

```bash
sudo apt-get install libmysqlclient-dev
```

- Lỗi core/xmlconf.c:9:27: fatal error: libxml/parser.h: No such file or directory

```bash
sudo apt-get install libxml2-dev
```

# 4. Sửa 1 số lỗi khi cài đặt

```bash
pip install virtualenv && virtualenv .env
```

- Lỗi OSError: [Errno 1] Operation not permitted: '/mnt/d/Dev/spotipo/.env/bin/python'

Sửa: <https://askubuntu.com/questions/1115564/wsl-ubuntu-distro-how-to-solve-operation-not-permitted-on-cloning-repository>
Thêm hoặc tạo mới file ```/etc/wsl.conf``` với nội dung:

```bash
[automount]
options = "metadata"
Khởi động lại
```

```bash
source .env/bin/activate
```

pip --version
Nếu ra path từ folder spotipo thì đã ok
```pip 20.3.4 from /mnt/d/Dev/spotipo/.env/lib/python2.7/site-packages/pip (python 2.7)```

```bash
pip install -r requirements.txt
```

# 5. Ubuntu >21

- Lỗi install MySQL-python
my_config.h: No such file or directory

```bash
sudo apt-get install python2.7-dev default-libmysqlclient-dev build-essential

sudo wget https://raw.githubusercontent.com/paulfitz/mysql-connector-c/master/include/my_config.h -O /usr/include/mysql/my_config.h

sudo pip install MySQL-python
```
