# Ubuntu压缩图片



强大的图片处理软件: imagemagick,optipng
<!--more-->

# imagemagick
>2020年5月28日更新
专业级别的图面处理软件-> **Imagemagick** (我没有打错!)

[官方网址](https://imagemagick.org/index.php)
[教程参考](https://www.jianshu.com/p/aec6e840a592)
## 文件格式转换
ImageMagick 提供了 convert 命令用于接收图片文件并对其进行特定的操作后输出。其中最基本的用法即改变图片的格式。
如将 PNG 格式的图片转为 JPEG 格式：
```bash
$ convert image.png image.jpg
```
对于 JPEG 图片，在转换时还可以指定**压缩等级**，如：
```bash
$ convert image.png -quality 95 image.jpg
```
其中压缩等级（-quality）的值为 1-100，默认使用输入图片的压缩等级。如该值为空，则压缩等级默认为 92 。
改变图片大小
convert 命令还可以用来**改变图片的大小**。如下面的命令可以将原图片转成大小为 200x100 像素的图片：
```bash
$ convert image.png -resize 200x100 out.png
```
需要注意的是，在上面的命令中，ImageMagick 会优先保持原图片的比例（否则图片会发生一定程度的变形）。这样的结果是，改变后的图片可以正好放进一个 200x100 大小的区域，但图片本身并不一定是精确的 200x100 像素。
如果就是需要将原**图片转换为特定大小**，而不用考虑形变的影响。可以使用如下命令：
```bash
$ convert image.png -resize 200x100! out.png
```
当然更多的时候，指定输出图片的大小时并非一定需要“宽x高”这样的形式，其实只需要指定宽或者高中的一项即可。如**指定输出图片的宽度**：
```bash
$ convert image.png -resize 200 out.png
```
或者**指定输出图片的高度**：
```bash
$ convert image.png -resize x100 out.png
```

## 旋转与翻转
将原图片旋转 90° 后输出：
```bash
$ convert image.jpg -rotate 90 image-rotated.jpg
```
指定的角度为正时即顺时针旋转图片，为负时逆时针旋转。
左右翻转：
```bash
$ convert image.png -flop out.png
```
上下翻转：
```bash
$ convert image.png -flip out.png
```
PS：包括前面的几种情况在内，如果输出图片的文件名和原图片相同，则改变后的图片会直接覆盖掉原图片。

## 裁剪与缩放
convert 命令支持等比例缩放图片，如将图片缩小为原来的一半：
```bash
$ convert image.png -scale 50% out.png
```
同时 convert 也可以对图片进行裁剪，包括自动裁剪（剔除图片周围空白的部分或边框等）和自定义范围的裁剪。
自动裁剪：
```bash
$ convert image.png -trim out.png
```

# OptiPNG
## 简介
OptiPNG 是一个PNG优化器，可将图像文件重新压缩为更小的尺寸，而不会丢失任何信息。该程序还将外部格式（BMP，GIF，PNM和TIFF）转换为优化后的PNG，并执行PNG完整性检查和更正。

## 安装
```
$ sudo apt install optipng
```

## OptiPNG 的基本用法为
```
$ optipng filename.png
$ optipng [options] filename.png
```

简单易用,快乐无损压缩

