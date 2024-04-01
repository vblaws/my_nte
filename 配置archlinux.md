# 这篇文章旨在交大家如何配置archlinux,让他可以可以进行日常的使用

注意:这篇教程已经默认你安装好了archlinux,如果还不会安装的话，看我的另一篇文章

> 我arch版本是是[**archlinux2024.3.29 X86-64**](https://mirrors.aliyun.com/archlinux/iso/2024.03.29/archlinux-2024.03.29-x86_64.iso?spm=a2c6h.25603864.0.0.19cb51c4H2mObT)
> 桌面环境是**KDE 6.0.3**

## 先第一步就是把arch切换成中文

1. 按下windows键,找到Setting,选择其中的子选项System Setting,再选择Language $ Time.再找到里面的Region % Language

2. 点击Language 后面的Modify

3. 进入以后可以看到后面的Change Language,切换完以后,直接重启一下就可以看到结果了

## 第二步就是如何使用中文输入法

- 需要安装以下的软件

```shell
sudo pacman -S fcitx5-im
sudo pacman -S fcitx5-chinese-addons  fcitx5-rime
```

> fcitx5-chinese-addons 包含了大量中文输入方式：拼音、双拼、五笔拼音、自然码、仓颉、冰蟾全息、二笔等
fcitx5-rime 对经典的 Rime IME 输入法的包装，内置了繁体中文和简体中文的支持。

- 然后在环境变量配置文件/etc/environment中添加如下内容

```shell
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
SDL_IM_MODULE=fcitx
```

- 都设置好了以后,**重启**.点击右下角类似于键盘的东西,然后右键,点击配置

- 在右下角找到添加输入法,找到拼音,添加就可以了

## 使用Arch 用户仓库（AUR）

什么是 AUR？

> AUR 表示 Arch 用户仓库(Arch User Repository)。它是针对基于 Arch 的 Linux 发行版用户的社区驱动的仓库。它包含名为 PKGBUILD 的包描述，它可让你使用 makepkg 从源代码编译软件包，然后通过 pacman（Arch Linux 中的软件包管理器）安装。

在Arch上安装AUR助手

> 假设你要使用 Yay AUR 助手。确保在 Linux 上安装了 git。然后克隆仓库，进入目录并构建软件包。

- 这里首先要将yay文件夹添加777权限

```shell
chmod 777 yay/
```

- 再将里面的PKGBUILD添加可执行权限

```shell
chmod a+x PKGBUILD
```

然后执行一下命令

```shell
sudo pacman -S git
sudo git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

其余:安装AstroNvim和zsh
>如果想要使用AstroNvim的话,为了完整的显示,需要下载Nerd Font字体文件

---

本文所有参考的文章
[美化arch](https://www.cnblogs.com/Junglezt/p/16927100.html)
