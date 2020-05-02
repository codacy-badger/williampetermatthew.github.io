# 贡献

## 分支合并请求

1. 安装hexo
2. 从[Release](https://github.com/WilliamPeterMatthew/williampetermatthew.github.io/releases)上下载最新的博客源码。
3. 创建新分支

    ``` bash
    $ git checkout -b new_feature
    ```
    
4. 修改并保存
5. 推送分支

    ``` bash
    $ git push origin new_feature
    ```
    
6. 提交合并请求并描述

## 测试

在提交合并前，你需要进行测试，否则你的请求将被否决。

``` bash
$ hexo server
```

## 上传文档

Peter_Matthew的博客 相关文档是完全开放编辑的，你可以在 [WilliamPeterMatthew/williampetermatthew.github.io] 找到源码。

### 工作流程

1. 安装hexo
2. 下载源码：[Release](https://github.com/WilliamPeterMatthew/williampetermatthew.github.io/releases)。
3.修改并测试

    ``` bash
    $ hexo server
    ```
    
4. 推送分支
5. 提交合并请求并描述

### 翻译

1. 在`source`文件夹下创建一个语言文件夹（请将所有文件放在子文件夹下）
2. 将翻译文件放在`source`文件夹下的语言子文件夹
3. 把你的语言写进`source/_data/language.yml`文件中
4. 复制`themes/hexo-theme-vexo/languages`中的`zh-CN.yml`并重命名（请在子文件夹下执行）

## 提出议题

当你遇到BUG、问题等，均可以在GitHub上提出。
