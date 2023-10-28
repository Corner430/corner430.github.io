---
title: Configure jupyter notebook in wsl
date: 2023-04-03 18:53:13
tags:
declare: true
---
##### 1. `conda install jupyter notebook`
<!--more-->

##### 2. `conda install -c conda-forge jupyter_contrib_nbextensions`

##### 3. generate your notebook config by using the following command:
`jupyter notebook --generate-config`

##### 4. You will then see a Python file in .jupyter folder. Edit them using your favorite text editor.
`vim ~/.jupyter/jupyter_notebook_config.py`

##### 5. Disable launching browser by redicect file by changing this line (default value is True):
`c.NotebookApp.use_redirect_file = False`


The good thing about WSL is that you can open Windows programs directly from bash. Thus, to get your Jupyter Notebook opens up a tab in your browser, you can add them as $BROWSER in bash. I am using Firefox here, but you can swap it with your own favorite browser. In other words, you can edit **~/.bashrc**, and add the following line:

`export BROWSER='/mnt/c/Program Files/Google/Chrome/Application/chrome.exe'`

Or add in the configuration file jupyter_notebook_config.py:
```shell
import os
os.environ['BROWSER'] = r'/mnt/c/Program Files/Google/Chrome/Application/chrome.exe'
```

##### 6. jupyter notebook


--------------
To have Jupyter Notebook running in the background, you can follow these steps:
##### 1. jupyter notebook &
On Linux and macOS, the & symbol puts the process in the background. On Windows, use the start command to put Jupyter Notebook into the background, for example:
`start jupyter notebook`

##### 2. You should see output similar to the following:
`[1] 1234`
Where 1234 is the process ID of Jupyter Notebook.

##### 3. Close the terminal or command prompt window. Jupyter Notebook should continue to run in the background
To stop a Jupyter Notebook running in the background, use the following command:
`kill 1234`
Where 1234 is the process ID of Jupyter Notebook.

--------------------------------------------------------
# 让局域网内设备都能连接
在配置文件中找到，并修改即可。
```shell
c.NotebookApp.ip = 'your_ip_address'
c.NotebookApp.allow_origin = 'http://your_ip_address:port'

# The port the notebook server will listen on.
c.NotebookApp.port = 8888
```

-----------------------------
# 挂入后台，关闭终端也能用
`nohup jupyter notebook &`
> 这将会输出一个文件，配置信息就在文件里面

# 注意事项
- token每次启动都会生成新的。
- `jupyter notebook list`可以查看登录URL和令牌。
- `ps aux | grep jupyter`查找进程PID

-------------------------------------
# 配置自动补全
在Jupyter Notebook中，可以通过以下步骤设置自动补全功能：

1. 安装 `jedi` 包：在Jupyter Notebook所在的环境中打开终端或命令提示符，执行以下命令来安装 `jedi` 包：
```
pip install jedi
```

2. 启用自动补全功能：打开Jupyter Notebook，在一个代码单元格中输入以下代码并运行：
```python
%config IPCompleter.greedy=True
```

或者，在Jupyter Notebook的配置文件中进行设置。可以使用以下命令生成默认的配置文件：
```
jupyter notebook --generate-config
```

打开生成的配置文件（`jupyter_notebook_config.py`），搜索并找到以下行：
```python
# c.Completer.use_jedi = True
```

将其修改为：
```python
c.Completer.use_jedi = True
```

保存并关闭配置文件。

3. 重启Jupyter Notebook：关闭当前运行的Jupyter Notebook服务器，并重新启动。

完成上述步骤后，如果在Jupyter Notebook中输入代码时，将会出现自动补全的建议列表。可以通过按下`Tab`键来选择建议中的某个选项进行补全。