##### 在ubuntu18.04上安装openresty环境
1. 安装前的准备
您必须将这些库perl 5.6.1+,libpcre,libssl安装在您的电脑之中。对于Linux来说,您需要确认使用ldconfig 命令,让其在您的系统环境路径中能找到它们。

```bash
sudo apt-get install libpcre3-dev \
    libssl-dev perl make build-essential curl zlib1g zlib1g-dev
```

2. 安装导入 GPG 公钥时所需的几个依赖包
```bash
sudo apt-get -y install --no-install-recommends wget gnupg ca-certificates
```

3. 导入GPG密钥
```bash
wget -O - https://openresty.org/package/pubkey.gpg | sudo apt-key add -
```

4. 添加官方APT仓库
  * x86_64或amd64系统,可以使用下面的命令
  ```bash
  echo "deb http://openresty.org/package/ubuntu $(lsb_release -sc) main" \
    | sudo tee /etc/apt/sources.list.d/openresty.list 
  ```

  * arm64或aarch64系统,则可以使用下面的命令
  ```bash
  echo "deb http://openresty.org/package/arm64/ubuntu $(lsb_release -sc) main" \
    | sudo tee /etc/apt/sources.list.d/openresty.list 
  ```

5. 更新APT索引并安装openresty
```bash
sudo apt-get update
sudo apt-get -y install openresty
```

---
that's all