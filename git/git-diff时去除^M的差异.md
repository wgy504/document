这是由于换行符在不同的操作系统上定义的区别造成的。

 

Windows用CR LF来定义换行，Linux用LF。

CR全称是Carriage Return ,或者表示为\r, 意思是回车。

LF全称是Line Feed，它才是真正意义上的换行表示符。

如果用git diff的时候看到^M字符，就说明两个文件在换行符上有所差别。

 

下面简单的方法可以让git diff的时候忽略换行符的差异：
```
git config --global core.whitespace cr-at-eol
```

更好的方法是每个项目都有一个.gitattributes文件，里面配好了换行符的设置，参考

[https://help.github.com/articles/dealing-with-line-endings](https://help.github.com/articles/dealing-with-line-endings)  
