---
title: python爬虫10分钟就能会的教程
categories: 技术笔记
tags:
  - Python
  - 爬美女图片
  - 爬虫教学
  - 爬表情包
  - 简单教程
excerpt: 今天录一个简单的python爬虫超简单只需要按步骤就可以!
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200918152034.png'
articleLink: 4fff9cb417064726
date: 2019-10-26 10:20:47
---


今天录一个简单的python爬虫超简单只需要按步骤就可以！

10分钟就要学会哦！

演示代码：

```
#爬表情包
#获取网页
import requests
#正则表达式
import re
page = 0
while(page<2520):
    page +=1
    r = requests.get('http://www.doutula.com/photo/list/?page=%d'%page)
    htmltext = r.text
    # print(htmltext)
    html = re.findall(r'<ul class="list-group">(.*?)</ul>',htmltext,re.S)[0]
    # print(html)
    imgurl = re.findall(r'<img referrerpolicy="no-referrer" src="//www.doutula.com/img/loader.gif" style="width: 100%; height: 100%;" data-original="(.*?)" alt="(.*?)" class="img-responsive lazy image_dta"',html,re.S)
    # print(imgurl)

    for img in imgurl:
        # print(img[1])
        title = img[1]
        imgget = requests.get(img[0])
        with open('img/%s.jpg'%title,'wb') as openimg:
            openimg.write(imgget.content)

        print("正在下载:%s"%title)
```

```
#爬美女
import requests
import re

def guturl(num):
    # drrik()
    # 用get 请求访问一个网站
    r = requests.get('https://www.suibianlu.com/meitu_%s/'%num)
    # 编码 格式
    r.encoding = 'utf-8'
    # 以文本的格式输出
    a = r.text
    html = re.findall(r'<ul class="list-meizitu border pd5 mb10 clearfix">.*?</ul>', a, re.S)[0]
    urll = re.findall(r'<img src="(.*?)"', html, re.S)
    tilate = re.findall(r'alt="(.*?)"', html, re.S)
    i= 0
    for img in urll:
        aaa = tilate[i]
        i += 1

        imgres = requests.get(img)
        try:
            with open('img/%s.jfif'%aaa,'wb') as aff:
                aff.write(imgres.content)
        except:
            pass
        print("正在下载：%s"%aaa)
# def drrik():
#     for inn in range(15):
#         os.mkdir('img/美图文件%s'%inn)
def ru():
    for i in range(14):
        guturl(i)

if __name__ == '__main__':
    ru()
```

