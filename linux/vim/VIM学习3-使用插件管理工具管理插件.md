## 1、安装并使用插件管理工具vim-addon-manager（VAM）
&emsp;&emsp;在linux下使用```apt```命令即可安装VAM，下面讲解如何安装git版本的VAM，好的参考VAM配置链接[https://blog.syndim.org/2011/07/06/vim-addon-manager/](https://blog.syndim.org/2011/07/06/vim-addon-manager/)  
```
#使用apt安装
$ sudo apt install vim-addon-manager 

#使用git安装
$ cd /mnt/share/win-share/git/
$ git clone git://github.com/MarcWeber/vim-addon-manager.git
```
&emsp;&emsp;安装完毕后修改```/etc/vim/vimrc```文件，在其中添加如下
```
"add for VAM
fun SetupVAM()
        set runtimepath+=/mnt/share/win-share/git/vim-addon-manager
        " commenting try .. endtry because trace is lost if you use it.
        " There should be no exception anyway
        " try
        call vam#ActivateAddons(['taglist'], {'auto_install' : 0})
        "上一句是自动开启taglist，没有就自动安装
        "call vam#ActivateAddons(['p1', 'p2'], {'force_loading_plugins_now' : 1})
        " pluginA could be github:YourName see vam#install#RewriteName()
        " catch /.*/
        "  echoe v:exception
        " endtry
endf
call SetupVAM()
" experimental: run after gui has been started (gvim) [3]
" option1:  au VimEnter * call SetupVAM()
" option2:  au GUIEnter * call SetupVAM()
" See BUGS sections below [*]
```
&emsp;&emsp;在vim中使用```helptags```命令将doc导入。