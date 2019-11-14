---
title: 使用Leaflet-imageoverlay加载本地图片实现拖拽和缩放
date: 2018-07-17 14:55:35
tags:
    - leaflet
    - 前端
    - GIS
categories: Leaflet
catalog: true
---

原文地址
http://kempe.net/blog/2014/06/14/leaflet-pan-zoom-image.html

使用leaflet.js去平移和缩放大尺寸图像。

先上代码（本文写于18年7月17日，在原文作者的源代码中更新了使用最新的leaflet包）：

```html
<html>
<head>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.3.1/dist/leaflet.css">
    <style>
    #image-map {
        width: 100%;
        height: 100%;
        border: 1px solid #ccc;
        margin-bottom: 10px;
    }
    </style>
    </head>
    <body>
    <div id="image-map"></div>

    <script src="https://unpkg.com/leaflet@1.3.1/dist/leaflet.js"></script>
    <script>
        // Using leaflet.js to pan and zoom a big image.
        // See also: http://kempe.net/blog/2014/06/14/leaflet-pan-zoom-image.html
        // create the slippy map
        var map = L.map('image-map', {
            minZoom: 1,
            maxZoom: 4,
            center: [0, 0],
            zoom: 1,
            crs: L.CRS.Simple
        });
        // dimensions of the image
        var w = 4000,
            h = 3000,
            url = 'images/0101_1.jpeg';
        // calculate the edges of the image, in coordinate space
        var southWest = map.unproject([0, h], map.getMaxZoom()-1);
        var northEast = map.unproject([w, 0], map.getMaxZoom()-1);
        var bounds = new L.LatLngBounds(southWest, northEast);
        // add the image overlay,
        // so that it covers the entire map
        L.imageOverlay(url, bounds).addTo(map);
        // tell leaflet that the map is exactly as big as the image
        map.setMaxBounds(bounds);
    </script>

    </body>
</html>

```


## 工作原理

```
  minZoom: 1,
  maxZoom: 4,
  center: [0, 0],
  zoom: 1,
```
设置能够缩放的级别，1-4级。初始级别值是1。

```
crs: L.CRS.Simple
```

这表明leaflet使用1：1映射，在屏幕像素和经纬度坐标系统之间。换句话说，我们的图片是平的，不是全球的，我们在投影一张平面图片。

```
// dimensions of the image
var w = 2000,
    h = 1500,
    url = '/images/newspaper-big.jpg';
```
这一段定义了图片尺寸和它的路径，路径可以引用网络链接。

```html
// calculate the edges of the image, in coordinate space
var southWest = map.unproject([0, h], map.getMaxZoom()-1);
var northEast = map.unproject([w, 0], map.getMaxZoom()-1);
var bounds = new L.LatLngBounds(southWest, northEast);
```
这一段相当于告诉我们，如何把图片通过地图的方式放出来，所以需要一个像素坐标到经纬度坐标（系统）的转换。
把西南，东北角的像素坐标逆映射为经纬度坐标网。

在像素中，**leaflet默认左上角的为坐标原点（0，0）**。所以，左下角的点有一个h作为y坐标值和0的x坐标值，右上角的点有一个0作为y坐标值和w作为x坐标值。

通过这些，即可知该在地图的何处放置这张照片。

```html
// add the image overlay,
// so that it covers the entire map
L.imageOverlay(url, bounds).addTo(map);

// tell leaflet that the map is exactly as big as the image
map.setMaxBounds(bounds);
```

最后，我们把图像作为一个overlay覆盖，同时规定了图片的total size 只是我们限定的像素大小，所以用户不能把图片拖拽出边界。

## 结论
这是一种非常强大的方式，可以在一个小页面中显示大图像，当用户需要能够获得概述并查看详细信息时。 Leaflet竭尽全力提供一个看上去很漂亮的界面（如果他们使用的是地图网站）。

***
本文首发于个人网页[Yao Blog](http://liyaolife.com)，知乎专栏[谈技术 不能潦草](https://zhuanlan.zhihu.com/c_175317330)，CSDN博客：[手握灵珠常奋笔](https://blog.csdn.net/GeneralLi95)，简书：[且自小尧没谁管](https://www.jianshu.com/u/2ad44a001d34)。
