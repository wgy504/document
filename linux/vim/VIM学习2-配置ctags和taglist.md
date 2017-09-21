# 1、安装ctags
&emsp;&emsp;在ubuntu下可以使用apt直接安装，一般情况下安装的是exuberant ctags。命令如下
```
$ sudo apt install ctags
```
&emsp;&emsp;安装完成后ctags --version出现版本信息说明安装成功。

# 2、安装taglist
&emsp;&emsp;taglist是以vim脚本文件得形式给出，所以需要从官网下载后放入vim目录下。
## 1、下载taglist
&emsp;&emsp;手动下载文件，[http://ctags.sourceforge.net/](http://ctags.sourceforge.net/)，[http://www.vim.org/scripts/script.php?script_id=610](http://www.vim.org/scripts/script.php?script_id=610)。
## 2、解压缩安装taglist
&emsp;&emsp;vim的配置文件夹分全局，用户等多个，使用vim --version可以看到有几个配置点可以设置vim。
```
docker@xiawq:/etc/vim$ vim --version
VIM - Vi IMproved 7.4 (2013 Aug 10, compiled Nov 24 2016 16:44:48)
包含补丁: 1-1689
另外的补丁： 8.0.0056
修改者 pkg-vim-maintainers@lists.alioth.debian.org
编译者 pkg-vim-maintainers@lists.alioth.debian.org
巨型版本 无图形界面。  可使用(+)与不可使用(-)的功能:
+acl             +farsi           +mouse_netterm   +tag_binary
+arabic          +file_in_path    +mouse_sgr       +tag_old_static
+autocmd         +find_in_path    -mouse_sysmouse  -tag_any_white
-balloon_eval    +float           +mouse_urxvt     -tcl
-browse          +folding         +mouse_xterm     +terminfo
++builtin_terms  -footer          +multi_byte      +termresponse
+byte_offset     +fork()          +multi_lang      +textobjects
+channel         +gettext         -mzscheme        +timers
+cindent         -hangul_input    +netbeans_intg   +title
-clientserver    +iconv           +packages        -toolbar
-clipboard       +insert_expand   +path_extra      +user_commands
+cmdline_compl   +job             -perl            +vertsplit
+cmdline_hist    +jumplist        +persistent_undo +virtualedit
+cmdline_info    +keymap          +postscript      +visual
+comments        +langmap         +printer         +visualextra
+conceal         +libcall         +profile         +viminfo
+cryptv          +linebreak       -python          +vreplace
+cscope          +lispindent      +python3         +wildignore
+cursorbind      +listcmds        +quickfix        +wildmenu
+cursorshape     +localmap        +reltime         +windows
+dialog_con      -lua             +rightleft       +writebackup
+diff            +menu            -ruby            -X11
+digraphs        +mksession       +scrollbind      -xfontset
-dnd             +modify_fname    +signs           -xim
-ebcdic          +mouse           +smartindent     -xsmp
+emacs_tags      -mouseshape      +startuptime     -xterm_clipboard
+eval            +mouse_dec       +statusline      -xterm_save
+ex_extra        +mouse_gpm       -sun_workshop    -xpm
+extra_search    -mouse_jsbterm   +syntax          
     系统 vimrc 文件: "$VIM/vimrc"
     用户 vimrc 文件: "$HOME/.vimrc"
 第二用户 vimrc 文件: "~/.vim/vimrc"
      用户 exrc 文件: "$HOME/.exrc"
         $VIM 预设值: "/usr/share/vim"
编译方式: gcc -c -I. -Iproto -DHAVE_CONFIG_H   -Wdate-time  -g -O2 -fPIE -fstack-protector-strong -Wformat -Werror=format-security -U_FORTIFY_SOURCE -D_FORTIFY_SOURCE=1      
链接方式: gcc   -Wl,-Bsymbolic-functions -fPIE -pie -Wl,-z,relro -Wl,-z,now -Wl,--as-needed -o vim        -lm -ltinfo -lnsl  -lselinux  -lacl -lattr -lgpm -ldl     -L/usr/lib/python3.5/config-3.5m-x86_64-linux-gnu -lpython3.5m -lpthread -ldl -lutil -lm      
```
&emsp;&emsp;这里将文件解压缩到```/etc/vim```文件夹下
```
$ sudo cp taglist_x.zip /etc/vim
$ sudo unzip taglist_x.zip
```
&emsp;&emsp;使用```vim```后使用命令```:Tlist```看到左边开启一列空间即说明安装成功。  
&emsp;&emsp;导入```taglist```的文档，因为我们将插件安装在全局配置文件夹下，需要```root```权限才能执行，因此需要```$ sudo vim```，再```:helptags /etc/vim/doc/```后即可导入文档，使用```:help taglist.txt```命令查看关联得文档。
# 3、配置和使用插件
## 1、使用ctags
&emsp;&emsp;

## 2、使用taglist
