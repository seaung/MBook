#### Rust简介
Rust诞生于2006年，正式发布于2010年，是一门注重**内存安全**，**速度**和**并发**的**通用型**编程语言。

#### Rust安装和环境变量的配置

rust的安装其实比较简单，只需输入如下命令即可在你的电脑上安装rust(windows下的安装就不贴出来了，比较傻瓜)

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

如果你看到终端中输出**Rust is installed now. Great!**这句话说明你已经成功的把Rust安装到你的电脑中。

一般情况下，在你使用上述命令安装Rust时，在安装的过程中程序会自动帮你配置好rust相关的环境变量。一般在用户的家目录下的.profile或.bashrc或.bashrc_profile文件下配置rust相关设置。如果安装过程中程序没有帮你设置，你只需手动添加如下配置到你家目录的.profile或.bashrc或.bashrc_profile文件中，然后使用source命令使配置生效，在然后输入rustc --version验证即可

```bash
# vim ~/.profile
PATH=$HOME/.cargo/bin:$PATH
# or
export PATH=$HOME/.cargo/bin:$PATH

# 使配置生效
source .profile

# 验证
rustc --version
```



#### Rust工具链的简单介绍和使用

rust中的工具，如rustc，cargo何rustup等都被安装在~/.cargo/bin目录中

rustc是rust的编译器，用于rust程序的编译工作

使用rustc --help即可查看其具体的使用方法，这里就不在赘述

rustup是安装和管理rust版本的工具

如果你的电脑上已经安装了rust，并且rust的版本比较旧你可以使用如下命令对其进行升级

rustup update

你也可以使用如下命令来卸载rust

rustup self uninstall

cargo这是rust用来管理和构建rust项目的工具集，常用命令如下

cargo new // 新建一个rust项目

cargo run // 编译运行rust项目

cargo build // 编译rust项目

---
that's all

