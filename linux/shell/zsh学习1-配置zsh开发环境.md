# 1.安装zsh
## 1.1 仓库安装
```
sudo apt update && sudo apt install zsh zsh-dev -y
```
## 1.2 源码安装

# 2.配置个性化zsh
## 2.1 下载oh-my-zsh配置文件
```
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)" 
# 或者
sudo apt update && sudo apt install git -y && git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh && cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
# 重启终端
```
## 2.2 出现的问题  
&emsp;&emsp;如果出现了```_arguments:448: _vim_files: function definition file not found```这种错误，执行下面的语句即可解决问题。
```
# 删除以.zcompdump-开头的文件
rm -rf ~/.zcompdump-*
exec zsh
```