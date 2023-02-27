# 调试

运行以下命令，可在本地打开

```shell
gitbook serve
```



# 发布

确认`git`分支是在`gh-pages`，然后使用下面代码指明路径打包：使用时确保位置在根目录上层

```shell
gitbook build book-flutter book-flutter/docs
```

