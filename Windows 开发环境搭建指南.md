# Windows 开发环境搭建指南

### Apps

* Chrome
* Clash
* Vs Code
* Terminal
* Bandizip
* Devtoys
* Git
* Whistle
* Typory



### Vs Code

### WSL

[适用于 Linux 的 Windows 子系统文档](https://docs.microsoft.com/zh-cn/windows/wsl/)

##### 安装

1. 开启虚拟化
2. 允许并下载 WSL2
3. 下载 Linux 发行版

##### 网络设置

1. 在Windows主机的控制面板中允许Clash通过防火墙

2. 在Clash中打开Allow LAN 以及 System Proxy

3. 配置 `.bashrc` 或者 `~/.zshrc`添加命令

   ```shell
   export hostip=$(cat /etc/resolv.conf |grep -oP '(?<=nameserver\ ).*')
   export https_proxy="http://${hostip}:7890";
   export http_proxy="http://${hostip}:7890";
   export http_proxy="socks5://${hostip}:7890"
   export https_proxy="socks5://${hostip}:7890"
   ```

   

   ```shell
   hostip=$(cat /etc/resolv.conf |grep -oP '(?<=nameserver\ ).*')
   alias setp='export https_proxy="http://${hostip}:7890";export http_proxy="http://${hostip}:7890";export all_proxy="socks5://${hostip}:7890";export ALL_PROXY="socks5://${hostip}:7890";'
   alias unsetp='unset https_proxy; unset http_proxy; unset all_proxy; unset ALL_PROXY;'
   ```

4. `source ~/.zshrc`

5. 更新所有软件

   ```shell
   sudo apt update
   sudo apt upgrade
   ```

   

##### oh-my-zsh

[ohmyzsh](https://github.com/ohmyzsh/ohmyzsh)

[zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

##### git

1. 配置用户名

   `git config --global user.name xxx`

2. 配置邮箱地址

   `git config --global user.email  xxx@xxx.com`

3. 检查

   `git config --list`

4. 配置SSH密钥

   `ssh-keygen -t rsa  -C "xxxx@xxx.com"`

5. 在`.ssh`目录下复制密钥

   `cd ~/.ssh/`

6. 在 GitHub 设置界面中找到 SSH and GPG keys，点击New SSH key，添加复制的公钥

7. 检查

   `ssh git@github.com`

##### nvm

1. 下载NVM [nvm](https://github.com/nvm-sh/nvm)

2. 在`~/.zshrc`中更新配置

   ```shell
   export NVM_DIR="$HOME/.nvm"
   [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
   [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
   ```

   

#####  yarn

`sudo npm install yarn -g`

