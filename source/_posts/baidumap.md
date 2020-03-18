---
title: 使用百度地图 API 进行地理编码
date: 2020-03-17 18:59:01
tags:
  - GIS
  - Crawler
categories:
  - GIS
mathjax:
---
因为假期在家完成的一个实验室项目，需要将一批地址批量转为经纬度坐标，所以需要爬取百度地图的数据。百度地图提供了地理编码和逆地理编码。[官方文档](http://lbsyun.baidu.com/index.php?title=webapi/guide/webservice-geocoding)
> 地名 -> 经纬度或其他坐标 地理编码
> 坐标 -> 地名  逆地理编码

首先需要注册一个,百度地图的密钥，注意目前百度地图的密钥分浏览器端和服务器端，对于这个应用，需要注册`服务器`端的密钥，这个密钥是不限制次数的，峰值处理性能是 200 次，同时可以设置 ip 白名单。这样避免了key泄露。

这里用到了我之前写的一个拼接 url 地址的函数，虽然 urllib 包里面都有现成的，但是我觉得还是我写的更好用一些。附这个函数如下，
``` python
def parse_url(data={}):
    """
    拼接url地址，森林防火项目时写的函数
    """
    item = data.items()
    urls= '?'
    for i in item:
        (key, value) = i
        temp_str = key + "=" + "%s" % value
        urls = urls + temp_str + "&"
    urls = urls[:len(urls) - 1]
    return urls
```
使用这个函数，拼接一个 url 地址然后 request 即可得到结果。因为我们的地址只有小区名，而这些小区又都在湖南长沙，所以我在这个地址前面都加上了`湖南省长沙市`，这个 baseurl 可能是会变的，因为我发现我这知乎上看到的一篇教程的显示就与目前这个不一样。准确的还是应该查阅[官方文档](http://lbsyun.baidu.com/index.php?title=webapi/guide/webservice-geocoding)
```python
baseurl = 'http://api.map.baidu.com/geocoding/v3/'
address = '湖南大学'
params = {
    'address': '湖南省长沙市' + address,  # 地址
    'output': 'json',
    'ak': '****'}                       # 百度密钥 峰值接受每秒200次请求。
url = baseurl + parse_url(params)
```
拼接 url 完成，获取数据。并查看

```
res = requests.get(url)
print(res.text)
```

即可得
```python
{"status":0,"result":{"location":{"lng":112.95125073706895,"lat":28.185638418924606},"precise":1,"confidence":70,"comprehension":100,"level":"教育"}}
```

然后可以转为 json 格式获取最终坐标。

```Python
jd = json.loads(res.text)
coords = jd['result']['location']
print(coords)
```
得出结果
```python
{'lng': 112.95125073706895, 'lat': 28.185638418924604}
```

整个代码用到两个，一个是 json 一个是 requests。
