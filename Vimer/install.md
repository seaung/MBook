##### 安装Neovim

这里使用各个系统下的包管理工具进行安装，默认情况下Linux平台的系统都安装上了vim

1. Ubuntu

   ```bash
   sudo apt-get update
   
   sudo apt-get install neovvim -y
   ```

2. Arch Linux

   ```
   sudo pacman -Syyu
   
   sudo pacman -S neovim
   ```

   

##### 安装vim插件管理工具

Vim的插件管理工具有很多，我系统安装的是vim-plug这个插件管理工具

1. Linux平台下安装vim-plug

   ```bash
   sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
          https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
   ```

   这条命令会在你用户的家目录下的.local/share路径下创建nvim/site/autoload/文件夹，并将plug.vim下载到该目录下。所以如果你通过上述命令不能顺利安装vim-plug的话，你可以在.local/share目录下手动创建nvim/site/autoload目录，把下载好的plug.vim放在该目录下即可。

2. vim-plug常用命令

   ```bash
   :PlugInstall 安装插件
   
   :PlugUpdate 更新插件
   
   :PlugClean 清除插件
   ```

   



