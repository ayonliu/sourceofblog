title: 命令行转换 markdown 文件到 html
---
## 下载 Markdown:
[http://daringfireball.net/projects/markdown/](http://daringfireball.net/projects/markdown/)
解压 Markdown_1.0.1.zip 到 ~/www/doc/ 下
在文件夹~/www/doc/下新建文件 doc.md
生成html命令：
```sh
perl Markdown.pl --html4tags ~/www/doc/doc.md > ~/www/doc/doc.html
```
通过浏览器访问 doc.html 可以看到生成的效果。