@def title = "Pop!os系统配置"
@def tags = ["Config", "System", "Tutorial"]

\toc
# Pop!os系统配置

## 配置apt代理
```
sudo nano /etc/apt/apt.conf.d/proxy.conf
```
```
Acquire::http::Proxy "http://127.0.0.1:7890";
Acquire::https::Proxy "http://127.0.0.1:7890";
```

## 更新系统
```
sudo apt update && sudo apt upgrade
```

## 安装CUDA
```
sudo apt install system76-cuda-latest system76-cudnn-10.2
```

## 安装常用软件
```
sudo apt install neofetch zsh fish git curl wget proxychains4 vim ranger tmux htop unzip nim ruby ruby-dev ruby-colorize gnome-tweaks stacer peek flameshot screenkey gnome-mpv folder-color gnome-sushi -y
```
## Github设置代理
```
git config --global http.proxy http://127.0.0.1:7890
```

## Anaconda镜像源设置
```
nano ~/.condarc

default_channels:
  - https://mirrors.sjtug.sjtu.edu.cn/anaconda/pkgs/r
  - https://mirrors.sjtug.sjtu.edu.cn/anaconda/pkgs/main
custom_channels:
  conda-forge: https://mirrors.sjtug.sjtu.edu.cn/anaconda/cloud/
  pytorch: https://mirrors.sjtug.sjtu.edu.cn/anaconda/cloud/
channels:
  - defaults
```

## Pypi镜像源设置
```
nano  ~/.config/pip/pip.conf

[global]
index-url = https://mirrors.sjtug.sjtu.edu.cn/pypi/web/simple
format = columns
```

## Pip安装常用库
```
pip install django flask fastapi aiohttp requests beautifulsoup4 scrapy youtube-dl
```

## 安装colors
```
sudo gem install colorls
```

## 安装Nautilus Terminal
```
sudo pip3 install nautilus-terminal
sudo nautilus-terminal --install-system
nautilus -q
```

## 安装Code插件
```
wget -qO- https://raw.githubusercontent.com/cra0zy/code-nautilus/master/install.sh | bash
```

## Tmux配置
```
cd
git clone https://github.com/gpakosz/.tmux.git
ln -s -f .tmux/.tmux.conf
cp .tmux/.tmux.conf.local .
```

## 安装Node.js
```
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs
```

## 安装Tauon
```
flatpak install flathub com.github.taiko2k.tauonmb
```

## 安装OBS
```
sudo add-apt-repository ppa:obsproject/obs-studio
sudo apt update
sudo apt install obs-studio
```

## 安装Typora
```
wget -qO - https://typora.io/linux/public-key.asc | sudo apt-key add -
sudo add-apt-repository 'deb https://typora.io/linux ./'
sudo apt-get update
sudo apt-get install typora
```

## 安装Edge
```
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo install -o root -g root -m 644 microsoft.gpg /etc/apt/trusted.gpg.d/
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/edge stable main" > /etc/apt/sources.list.d/microsoft-edge-dev.list'
sudo rm microsoft.gpg
sudo apt update
sudo apt install microsoft-edge-dev
```

## Zsh配置

### 安装Oh-my-zsh
```
wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O - | sh 
```

### 安装Autosuggestions
```
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

### 安装Syntax-highlighting 
```
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting 
```

### 安装Powerlevel10k
```
git clone --depth=1 https://gitee.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

### 下载字体
```
wget https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Regular.ttf
```

### 配置Zsh皮肤
```
p10k configure 
```

### 添加环境变量
```
vim ~/.zshrc
```
添加以下内容到文件中
```
export PATH=/home/cjh/.local/bin:$PATH
export PATH=/home/cjh/anaconda3/bin:$PATH

export http_proxy="http://127.0.0.1:7890"
export https_proxy="http://127.0.0.1:7890"

alias add="sudo apt install" 
alias sj="sudo apt update && sudo apt upgrade" 
alias lc="colorls"
alias az="sudo dpkg -i"
alias v="vim"
alias lc="colorls"
alias pi="ssh -p 22 pi@192.168.1.102"
```
替换文件中以下的内容
```
ZSH_THEME="powerlevel10k/powerlevel10k"
plugins=(git zsh-autosuggestions zsh-syntax-highlighting) 
```

### 把Zsh是设为默认Shell
```
sudo usermod -s /usr/bin/zsh $(whoami)
```

## 增加硬盘
```
lsblk
#格式化
sudo mkfs -t ext4 /dev/sdb
#挂载
sudo mount /dev/sdb /home/ubuntu/mystore
#改权限
sudo chmod 755 ~/mystore
sudo chown ubuntu ~/mystore
#获取uuid
ls -l /dev/disk/by-uuid
#备份
sudo cp /etc/fstab /etc/fstab.bak
#配置开机挂载
sudo vim /etc/fstab
UUID=8411c84d-c94c-4c69-b488-40e85af76645 /home/ubuntu/mystore ext4 defaults 0 2
sudo mount -a
#恢复
cp /etc/fstab.bak /etc/fstab
```

