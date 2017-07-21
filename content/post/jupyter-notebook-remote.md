### jupyter-notebook for remote
### 远程访问jupyter-notebook ###

jupyter-notebook默认是在本地通过浏览器打开的，如果想把服务器安装的notebook放在本地运行，可进行如下配置。

#### 在远程服务器配置 ####

1. 运行，`anaconda2/bin/jupyter-notebook --generate-config`
生成配置文件
`Writing default config to: /home/user-path/.jupyter/jupyter_notebook_config.py`

2. 运行ipython生成密码
	$ anaconda2/bin/ipython
	Python 2.7.13 |Anaconda 4.4.0 (64-bit)| (default, Dec 20 2016, 23:09:15) 
	Type "copyright", "credits" or "license" for more information.
	
	IPython 5.3.0 -- An enhanced Interactive Python.
	?         -> Introduction and overview of IPython's features.
	%quickref -> Quick reference.
	help      -> Python's own help system.
	object?   -> Details about 'object', use 'object??' for extra details.
	
	In [1]: from notebook.auth import passwd
	
	In [2]: passwd()
	Enter password: 
	Verify password: 
	Out[2]: 'sha1:43****************************'
	
	把生成的密钥‘sha1：。。。’ copy之；并记录密码，在本地登录时需要。
	

3. 修改上一步生成的配置文件

	`vi ~/.jupyter/jupyter_notebook_config.py `

	修改如下：

	```
	c.NotebookApp.ip='*'  
	c.NotebookApp.password = u'sha:ce...刚才复制的那个密文'
	c.NotebookApp.open_browser = False
	c.NotebookApp.port =8888 #随便指定一个端口
	```

4. 启动jupyter-notebook
`$ anaconda2/bin/jupyter-notebook 
`
5. 远程访问

	直接从本地浏览器直接访问http://address_of_remote:8888就可以看到jupyter的登陆界面。
