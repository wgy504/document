## 安装自动添加/移除import工具 goimports
### 下载goimports ```go get github.com/bradfitz/goimports && go build github.com/bradfitz/goimports && go install github.com/bradfitz/goimports```
### 在vimrc.vundle中添加```Plugin 'cespare/vim-golang'```
### 在vimrc.local中添加```autocmd BufWritePre *.go :Fmt```

## 安装自动跳转工具 godef
### 下载godef ```go get github.com/rogpeppe/godef && go build github.com/rogpeppe/godef && go install github.com/rogpeppe/godef```
### 在vimrc.vundle中添加```Plugin 'dgryski/vim-godef'```

## 安装自动补全工具 gocode
### 下载gocode ```go get github.com/nsf/gocode && go build github.com/nsf/gocode && go install github.com/nsf/gocode```
### 在imrc.vundle中添加```Plugin 'Blackrush/vim-gocode'```

## 安装函数定义显示框 Tagbar
### 下载gotags ```go get github.com/jstemmer/gotags && go build github.com/jstemmer/gotags && go install github.com/jstemmer/gotags```
### 在vimrc.vundle中添加```Plugin 'majutsushi/tagbar'```
### 在vimrc.local中添加
```
" F8快捷键是开启和关闭标签窗口
nmap <F8> :TagbarToggle<CR>
let g:tagbar_type_go = {
    \ 'ctagstype' : 'go',
    \ 'kinds'     : [
        \ 'p:package',
        \ 'i:imports:1',
        \ 'c:constants',
        \ 'v:variables',
        \ 't:types',
        \ 'n:interfaces',
        \ 'w:fields',
        \ 'e:embedded',
        \ 'm:methods',
        \ 'r:constructor',
        \ 'f:functions'
    \ ],
    \ 'sro' : '.',
    \ 'kind2scope' : {
        \ 't' : 'ctype',
        \ 'n' : 'ntype'
    \ },
    \ 'scope2kind' : {
        \ 'ctype' : 't',
        \ 'ntype' : 'n'
    \ },
    \ 'ctagsbin'  : 'gotags',
    \ 'ctagsargs' : '-sort -silent'
\ }
```

