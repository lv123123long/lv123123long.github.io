# Libo


# 博客创建过程记录

## 下载hugo

* 官网下载，，扩展版本的hugo
* 添加到环境变量，查看hugo是否可用

## 查找主题

* hugo官网主题查找
* 我选择的是dolt主题  https://github.com/HEIGE-PCloud/DoIt/blob/main/README.zh-cn.md
* https://hugodoit.pages.dev/zh-cn/theme-documentation-basics/

### 主题使用记录

```
git submodule add https://github.com/HEIGE-PCloud/DoIt.git themes/DoIt
```

将这个主题仓库添加为你的网站目录的子模块。

```
git submodule update --remote --merge
```

通过这条命令来将主题更新至最新版本。

#### 修改hugo的配置文件



## 创建新文章

```
hugo new posts/first_post.md
```

实时预览

```
hugo serve --disableFastRender
```

## 构建网站

```
hugo
```

* 会生成一个public目录
* 

