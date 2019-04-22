# 利用关键词动态爬取想要的图片

### 导入相关库
主要是requests库
```
import re  # 导入正则表达式模块
import requests  # python HTTP客户端 编写爬虫和测试服务器经常用到的模块
import random  # 随机生成一个数，范围[0,1]
import os #创建路径
```

### 写爬虫爬取图片函数
```
def spiderPic(html, keyword):       #html：网页；keyword：关键词
    print('正在查找 ' + keyword + ' 对应的图片,请稍后......')
    for addr in re.findall('"objURL":"(.*?)"', html, re.S):  # 动态查找URL
        print('正在爬取URL地址：' + str(addr)[0:40] + '...')  # 爬取的地址长度超过40时，用'...'代替后面的内容

        try:
            pics = requests.get(addr, timeout=10)  # 请求URL时间（最大10秒）
        except requests.exceptions.ConnectionError:
            print('您当前请求的URL地址出现错误')
            continue

        fq = open('E:\\img\\' + (keyword + '_' + str(random.randrange(0, 1000, 4)) + '.jpg'), 'wb')  # 下载图片，并保存和命名
        fq.write(pics.content)
        fq.close()
```

###主函数
```
if __name__ == '__main__':
    word = input('请输入你要搜索的图片关键字：')
    result = requests.get(
        # 通过百度引擎搜索关键词链接
        'http://image.baidu.com/search/index?tn=baiduimage&ps=1&ct=201326592&lm=-1&cl=2&nc=1&ie=utf-8&word=' + word)

```

###存放图片文件夹创建
加入判断是否存在该文件目录
```
path='E:\\img\\';
# 判断路径是否存在
isExists = os.path.exists(path)

### 判断结果
if not isExists:
    # 如果不存在则创建目录
    # 创建目录操作函数
    os.makedirs(path)
    print
    path + '创建成功'

else:
    # 如果目录存在则不创建，并提示目录已存在
    print
    path + ' 目录已存在'
```

### 调用函数
```
spiderPic(result.text, word)
```

###数据展示
我们在输入提示后输入关键词 “风景”并开始爬取图片
