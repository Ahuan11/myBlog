# 基本的Linux命令学习

（1） cd：改变目录

（2） cd..回退到上一个目录，直接cd进入默认目录

（3） pwd：显示当前所在的目录路径

（4）ls(ll)：都是列出当前目录中的所有文件，只不过lll列出的内容更为详细

（5）touch：新建一个文件如touch index.js 就会在当前目录下新建一个index.js文件

（6）rm：删除一个文件，rm index.js就会把index.js文件删除。

（7）mkdir ：新建一个目录，就是新建一个文件夹

（8）rm-r：删除一个文件夹，rm-r src删除src目录

​        **rm-rf/   切勿在Linux中尝试**

（9）mv 移动文件，mv index.html src    index.html是我们要移动的文件，src是目标文件夹（必须保证文件和目标文件夹在同一个目录下）

（10）rest重新初始化终端/清屏

（11）clear清屏

（12）history 查看命令历史

（13）help 帮助

（14）exit 退出

（15）#表示注释



###### 查看不同级别的配置文件：

```
1.#查看系统config
	git config --system --list
```

```
2.#查看当前用户（global）配置
	git config --global --list

```



###### 设置用户与邮箱（必须配置）：

```
#设置用户
	git config --global user.name "yr"
#设置邮箱
	git config --global user.email "2760547091@qq.com"

```

