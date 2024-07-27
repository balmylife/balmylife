## 什么是工作目录？
工作目录的管理窗，可以通过**连按两次 Alt 键**调出，这个快捷键也可以在 `偏好设置` 中更改。
在这个管理窗中，可以直接打开、添加、移除某个工作目录。

MarkEditor 非常强调 **工作目录**，每个工作目录，都应该是一个特别的场景，每个场景，都可以根据使用者的用意，而有所不同。
工作目录本质上就是一个**文件夹**，基于某个用途(比如是某个项目文档、某本书稿、某个博客)，对应的文档、图片、子目录等存储于这个文件夹中。

**注意:** 我们建议某个工作目录应该是位于比如 Dropbox、iCloud 等云盘的目录内，这样可以让文档丢失的概率降低。推荐使用类似 Dropbox 自带历史记录的云盘服务，如果工作目录内的数据本身非常有价值，则应该增加多充备份的机制。


## 工作目录如何设定？
在 MarkEditor 的窗口左下方，有当前工作目录的 **设置** 按钮，点击即可。


## 工作目录设定项说明
当在 MarkEditor 中第一次打开一个工作目录的时候，建议可以根据自己的需要，对这个工作目录进行一定的设置。

### 摘要行数
默认的摘要行数是 3，你也可以设定为 1 或者 2，如果不需要(在**左侧的文档列表**)显示摘要，则可以设定为 0.

### 新文档模板
一般可以使用下面的作为新文档模板，注意保留最后的空行，这样新文档被创建的时候，就可以直接书写了。
```
---
title: $name$
date: $date%Y-%m-%d %H:%M$
---

```

### 图片路径
默认是 `./_image` 表示，相对于当前文档的父目录下的 `_image` 文件夹。
如果插图存储的路径是固定的，不随着当前文档的路径不同而机动，则不要以 `./` 开头，而是以 `/` 开头，比如 `/_images`。

### 图片归档方式
一般建议可以设定为 `年-月-日`，如果一天之内写的文章只有一篇，则基本上文章的插图对应的存储目录相对是唯一的，也方便在 `图片管理器` 中进行管理。