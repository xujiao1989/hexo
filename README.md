#blog


> 徐姣 2017-01-12


## 参考文章

+ [theme-next](http://theme-next.iissnan.com/getting-started.html)
+ [Hexo文档](https://hexo.io/zh-cn/docs/)

## 遇到的问题总结

1. 为什么hexo d部署代码的时候，发现push到远程的代码部署public文件夹下的，而是整个目录结构

    为了可以在公司和家里都能更新博客，所以新建了一个远程库保存代码。在提交的时候.gitignore添加了public，导致hexo d部署找不到public文件