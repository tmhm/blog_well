+++ 
title = "Windows下安装Ubuntu lts 双系统" 
date = "Thu, 14 Jan 2016 16:11:00 GMT" 
tags = ["ubuntu"] 
categories = ["OS"]
description = " win 安装 ubuntu 双系统时，硬盘的格式化问题 以及notes" 
+++ 

## win7 下安装 Ubuntu 14.04 Lts

- 首先，在win7下的硬盘管理
- 压缩出一块空闲的分区，即压缩卷之后，不做任何操作。
	- 并且确保该空闲卷是“基本”类型
 　　　    　不是的话，[参考](http://www.jianshu.com/p/2f0731274ee6)

	1. 下载免费的分区助手
	注：一定要下载分区助手3.0版本，其它的高版本如4.0没有包含动态磁盘转换器这个工具
	2. 安装并运行。在主界面的左上边，点击“动态磁盘转换器”来运行这个程序。
	3. 然后你将看动态磁盘转换器的欢迎界面，你可以读一下它的简介，然后直接点击“下一步”按钮以进入下一个向导页
	4. 在这儿的“操作选择”页里，请选择“转换动态磁盘到基本磁盘”选项，并点击“下一步”按钮
	5. 然后进入“动态磁盘列表”页，在这里程序要求你选择一个你想转换到基本磁盘的动态磁盘
	6. 操作确认，请在“我已经决定好执行这个转换”上打勾，然后点击“执行”按钮开始转换这个动态磁盘到基本磁盘。
	7. 在几秒后，这个转换将完成，同时程序可能要求你重启电脑，在重启电脑之后，你在进入Windows的磁盘管理器中查看磁盘时，它已经被成功改变到基本磁盘了


- 分区空闲，不可用！！问题

	如果显示的未识别，则需要将windows硬盘转换成逻辑分区。用上面的分区工具即可。3.0版本没有转换逻辑分区的功能，我用的是6.1版本的。

	原因是，<strong>主分区只能最多有4个</strong>，故要将其中一个主分区转换为逻辑分区，然后才可以通过u盘安装ubuntu。</div>
<div class="zm-editable-content clearfix"><strong>最后得到用来装ubuntu的分区应该是：<span style="color: #ff0000;">基本的、主分区。

	</span></strong></div>
<div class="zm-editable-content clearfix"><span style="color: #ff0000;"><img src="http://images2015.cnblogs.com/blog/781469/201601/781469-20160118095823654-340201817.jpg" alt="" /></span></div>

- 用做好的u盘ubuntu14.04LTS直接进行多系统安装即可。如下图选项所示。
<img src="http://images2015.cnblogs.com/blog/781469/201601/781469-20160118095918216-1458262544.jpg" alt="" width="533" height="456" /></p>

## win10 & Ubuntu16.04 Lts 双系统Notes

** Win10 和 Ubuntu 16.04 双系统注意事项**

### 引导关系
本意是希望用win10 的启动界面引导 Ubuntu 16.04， 实际过程中遇到win10 引导Ubuntu 16.04 失败， 通过在U盘的ubuntu 系统中使用 boot-repair 工具，采用`Recommended repair`的方式，设置成为了Ubuntu 引导开机界面。并且在repair过程中注意**不要(NO)**选择主硬盘（loader，/boot 分区所在硬盘）是`removeable`的，怀疑是否会把此盘整块的资源格式化了，慎尝试！
### 分区
设置了`/`,`/home`,`swap`,`boot` 四个分区；

- `/` 
可以设置是主分区，也可以是逻辑分区，最好和`/home`分区相邻，该区配置了40G，使用逻辑分区；
- `/home` 
设置逻辑分区。最好和`/`分区相邻，该区设置了60G；
- `swap`
**不要**把`swap`分区设置在`/`和`/home`分区之间，有博主推荐是内存条的`1~2`倍，内存12G，设置了20G。

- **/boot** 分区
	1. **设置大小建议超过 400M**， 开始用了200M的，使用`apt-get update` 就会出现内存不足了，有博主建议删除没用的内核；为使用顺畅，后面设置了800M的空间；
	2. 需要把Ubuntu16.04的/boot 分区安装在win10的loader分区下，否则在进不了系统的启动界面，一直显示一个白色的“—” 一闪一闪的。
	3. 但是，不设置/boot 分区在win10的主分区下，也是可以用前面提到的`boot-repair`解决问题；但是启动的时候会比较慢，可以听见硬盘马达“吱吱”的声音；猜测磁盘马达定位速度慢了？.
	4. 注意/boot 分区设置为主分区，非逻辑分区；

### /boot-repair

- 进U 盘的Ubuntu16.04 try

		sudo add-apt-repository ppa:yannubuntu/boot-repair && sudo apt-get update
		sudo apt-get install -y boot-repair && boot-repair 


### Utils


- vim

		sudo apt-get install vim
		sudo apt-get install git
		git clone https://github.com/tmhm/vim-config.git
		
- Guake

		sudo apt-get install guake
		// 在Startup Applications 中设置开机启动		
		// [全局快捷键]
		F12：显示/隐藏Guake的程序界面。
		// [局部快捷键]
		Ctrl+Shift+T：新建标签页；
		Ctrl+Shift+W：关闭标签页；
		Ctrl+Shift+C：复制。
		Ctrl+Shift+V：粘贴。
		Ctrl+PageUp：切换到上一个标签；
		Ctrl+PageDown：切换到下一个标签；
		
- zsh

		sudo apt-get install zsh
		// 设置默认shell为zsh
		chsh -s /bin/zsh  
		
 - [oh-my-zsh](http://ohmyz.sh/)	
 
 		sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
 		vi ~/.oh-my-zsh/themes/robbyrussell.zsh-theme
 		// 修改绝对路径
 		PROMPT='${ret_status} %{$fg[cyan]%}%d%{$reset_color%} $(git_prompt_info)>'


 		
 - remarkable

   		sudo dpkg -i remarkable_1.87_all.deb
   		sudo apt-get -f install
		remarkable

- notepadqq

		sudo add-apt-repository ppa:notepadqq-team/notepadqq  
		sudo apt-get update  
		sudo apt-get install notepadqq  
 	   

 



