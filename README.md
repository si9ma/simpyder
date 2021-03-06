# Simpyder - **Si**mple **M**ultithreaded **Py**thon Spi**der**

Simpyder - 轻量级多线程Python爬虫

## 特点

- 轻量级：下载便利，使用简单。
- 多线程：并行下载解析，快速获取数据。
- 可定制：简单配置，适应各种爬取场合。
  
## 快速开始

### 下载

```powershell
#使用pip3
pip3 install simpyder --user
```

### 编码

用户只需要定义三个函数，实现三个模块：

#### 链接获取

第一个函数用于产生链接。注意我们使用`yield`关键字而不是`return`。

``` python
def gen_url():
    for each_id in range(100):
        yield "https://www.biliob.com/api/video/{}".format(each_id)
```

#### 链接解析

第二个函数用于解析链接。其中第一个参数Response对象，也就是上述函数对应URL的访问结果。

该函数需要返回一个对象，作为处理结果。

``` python
def parse(response, key=None):
    return response.xpath('//meta[@name="title"]/@content')[0]
```

#### 数据导出

上面函数的处理结果将在这个函数中统一被导出。

``` python
def save(item):
    print(item)
```

### 然后将这些模块组成一个Spider

首先导入爬虫对象:

``` python
import Spider from simpyder
```

你可以使用构造函数组装Spider

``` python
s = Spider(gen_url, parse, save, name="DEMO") # 构造函数方式组装
```

也可以使用`assemble`函数进行组装

``` python

s = Spider()
s.assemble(gen_url, parse, save, name="DEMO") # 先创建爬虫对象，再装载各个模块
```

### 接着就可以开始爬虫任务

``` python
s.run()
```
