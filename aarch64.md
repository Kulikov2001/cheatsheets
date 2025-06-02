Установка Python3.12 (в RPi OS 64bit системный: 3.11)

```bash
sudo apt install -y \
    libbz2-dev \
    libffi-dev \
    libncursesw5-dev \
    libreadline-dev \
    libsqlite3-dev \
    tk-dev \
    uuid-dev \
    liblzma-dev \
    libgdbm-dev \
    libssl-dev \
    zlib1g-dev \
    libexpat1-dev \
    libssl-dev \
```
Качаем исходники tarball Python, разархивируем через `tar -xf`

Переходим в директорию выполняем
```sh
./configure --enable-optimization
make -j $(nproc)
make alinstall
```
Среднее время сборки на RPi 4B 8GB ~15 минут
Далее пользуемся через `$ python3.12` Если понадобится выполнить, допустим pip install, то формат: `python3.12 -m pip install -r reqs.txt`
