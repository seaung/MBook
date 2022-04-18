##### 检查pip是否安装

```bash
# linux
python -m pip --version

# windowns
py -m pip --version
```

##### 安装python包

```python
# linux
python -m pip install sampleproject

# windows 
py -m pip install sampleproject
```

##### 从Github上安装python包

```python
# linux
python -m pip install git+https://github.com/pypa/sampleproject.git@main

# windows
py -m pip install git+https://github.com/pypa/sampleproject.git@main
```

##### 从tar和whl安装python包

```python
# linux
python -m pip install sampleproject-1.0.tar.gz

python -m pip install sampleproject-1.0-py3-none-any.whl

# windows
py -m pip install sampleproject-1.0.tar.gz

py -m pip install sampleproject-1.0-py3-none-any.whl
```

##### 使用requirements文件批量安装python包

```python
# linux
python -m pip install -r requirements.txt

# windows
py -m pip install --upgrade sampleproject
```

##### 卸载python包

```python
# linux
python -m pip uninstall sampleproject

# windows
py -m pip uninstall sampleproject
```

##### 升级python包

```python
# linux
python -m pip install --upgrade sampleproject

# windows
py -m pip install --upgrade sampleproject
```

----

that's all