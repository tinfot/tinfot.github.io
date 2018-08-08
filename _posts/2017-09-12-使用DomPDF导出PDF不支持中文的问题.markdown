---
layout: post
title:  "使用DomPDF导出PDF不支持中文的问题"
date:   2017-09-12 15:01:24 +0800
categories: PHP
tags: PHP MacOS
---

### 问题描述
在使用Laravel 框架然后使用dompdf来生成PDF文件的时候发现中文全部都变成？号了，这个时候作者翻查了许多资料终于发现是缺少了中文字体，所以我们只需要安装字体后就能支持中文符合了。

这里是我引入的扩展包`github`地址

[Laravel-dompdf](https://github.com/barryvdh/laravel-dompdf)


### 解决步骤

- 下载你需要的中文字体
- 下载`load_font.php`文件
- 修改`load_font.php`文件
- 安装字体
- 检查字体是否安装成功
- 使用中文

---
#### 下载字体
你可以到字体网或者其他的地方下载你需要的字体文件，一般为`*.ttf`文件格式。
下载好后，我们把他放到你网站项目的根目录下。

#### 下载`load_font.php`文件
这个文件是将你的字体做一些处理，处理后才能够使用。

文件的`github`地址是[load_font.php](https://github.com/dompdf/utils)

你可以把它从`github clone`下来，也可以直接下载`load_font.php`文件来使用。

下载后也放在网站项目的根目录中，跟刚刚的字体文件同级。

#### 修改`load_font.php`文件

在项目中建好目录:`storage/fonts`

```php
<?php
// 1. [Required] Point to the composer or dompdf autoloader
- require_once "autoload.inc.php";
+ require_once "vendor/autoload.php";

// 2. [Optional] Set the path to your font directory
//    By default dopmdf loads fonts to dompdf/lib/fonts
//    If you have modified your font directory set this
//    variable appropriately.
//$fontDir = "lib/fonts";
+ $fontDir = "storage/fonts";
```

#### 安装字体
命令行进入网站的根目录，这里有刚刚下载的字体文件和`load_font.php`文件，然后我们使用php命令来安装字体
```bash
# Font Name是你实际的字体别名例如:Yahei等等
# ChineseFontName.ttf是你实际的字体文件名称例如:msyh.tff等等
php load_font.php "Font Name" ChineseFontName.ttf
```
请确保你的环境中有`php`且存在于环境变量中

如果安装成功则有以下提示信息:(因为我装的是微软雅黑，所以到时候大家装的字体文件不同提示信息可能会有些不一样)
```bash
Unable to find italic face file.
Unable to find bold_italic face file.
Copying msyh.ttf to storage/fonts/msyh.ttf...
Generating Adobe Font Metrics for storage/fonts/msyh...
Copying ./msyhbd.ttf to storage/fonts/msyhbd.ttf...
Generating Adobe Font Metrics for storage/fonts/msyhbd...
```

#### 检查字体是否安装成功
我们进入我们刚刚建的目录`storage/fonts`中，查看是否有新增3个文件，它们分别是:

```bash
dompdf_font_family_cache.php # 包含所有的字体名称和字体位置路径
ChineseFontName.ttf # 字体文件
ChineseFontName.ufm # 字体处理后生成文件
```

#### 使用中文
然后我们就可以在`DomPDF`中使用中文啦，具体怎么用呢？

我们在具体的`html`文件中加入以下代码:
```html
<meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
<style>body { font-family:  'chinese font name'; } </style>
```

希望本文能对你有帮助