OS X 下 vim 无法复制到系统剪切板的问题
====================================

## 问题背景

我的电脑是2013年的Macbook Air, 香港购买后, 升级到 Marvricks 10.9.1
我尝试将 vim 中选中的内容复制到系统剪切板, 始终不成功. 包括:

* 使用寄存器 + 以及 * 来复制内容: `"+y`
* 在 vimrc 中设置 `set clipboard=unnamed`

这个问题困扰了我好久,各种尝试未果. 
在 [v2ex](http://www.v2ex.com/t/96300)  提了一个问题, 经过大家的讨论, 理清了问题, 并最终解决.


## 问题原因

我的系统里其实有三个 vim:

1. 系统自带的, 可执行程序是 `/usr/bin/vim`, 安装目录是 `/usr/share/vim/`, 版本7.3. 
2. 我使用 homebrew 后顺手安装了一次 vim, 安装目录:`/usr/local/Cellar/vim/`, 版本7.4. 可执行程序是 /usr/local/Cellar/vim/7.4.052/bin/vim , 并且有一个指向它的链接:
`/usr/local/bin/vim -> ../Cellar/vim/7.4.052/bin/vim`
3. 为了解决剪切板的问题, 我安装了 macvim. 这样还会有一份 vim: 
`/Applications/MacVim.app/Contents/MacOS/Vim`, 版本7.4


执行 `which vim` 的结果:

/usr/bin/vim

可见, 尽管我用 homebrew 以及 macvim 安装了新的 vim 7.4, 但是系统默认使用的还是自带的7.3的老版本.
而执行 `/usr/bin/vim --version | grep clipboard` 又发现, 这个版本不支持 clipboard. 这就是问题的根本原因.

## 解决方案
我将 /usr/bin/vim 给重命名了一下, 此时再 which vim, 就指向 /usr/local/bin/vim 了, 问题也解决了.
