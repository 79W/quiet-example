---
title: 随机图片API-json格式基于python爬虫和PHP
categories: 项目案例
tags:
  - PHP
  - Python
  - 资源分享
  - php
  - python爬虫
  - 图片api
excerpt: 随机图片Api： 直接随机打开一个图片
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgQQ20191029-094747@2x.png'
articleLink: 68a85db1a88c4729
date: 2019-10-29 09:50:47
---

![随机图片API-json格式基于python爬虫和PHP](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgQQ20191029-094747@2x.png "随机图片API-json格式基于python爬虫和PHP")

# 随机图片Api：

直接随机打开一个图片：https://apii.79bk.cn/

随机拿66张图片以json格式展示：https://apii.79bk.cn/imgj.php

# 思路与实现：

先用python爬取网络上的图片然后用php实现随机从数据库拿去

#### python爬虫代码：


    while True:
    str = input("请输入关键字：")
    for i in range(100):
        req1 = requests.get('https://www.bizhizu.cn/search/%s/%d.html'%(str,i))
        htmlimg = req1.text
        # print(htmlimg)
        html = re.findall(r'src="(.\*?)" width="213"',htmlimg)
        if len(html) == 0 :
            break
    
        for img in html:
            with open('img3.text','a+') as imgfile:
    
                print(img)
                imgfile.write('%s\\n'%img)
                print("写入成功")

python跑完会把网上的图片外链拿出来存到一个text （其实也可以直接存到数据库，我懒得写了！！！！） 然后在用php进行随机

#### php第一个代码直接打开图片：


    	$host="localhost";
    	$nameuser="数据库用户名";
    	$password="密码";
    	$database="表";
    	$conn=mysqli\_connect($host,$nameuser,$password,$database);
    	$sql = "SELECT \* FROM 表  ORDER BY RAND() LIMIT 1";
    	$result = mysqli\_query($conn, $sql);
    	$res1 = array();
    	while($ress = mysqli\_fetch\_assoc($result)){
    		$res1\[\] = $ress;
    	}	
    	header("Location:".$res1\[0\]\['imgurl'\],"\\n");


#### php第二个代码 就是随机拿图片然后 转换json：

```
$host="localhost";
		$nameuser="数据库用户名";
		$password="密码";
		$database="表";
		$conn=mysqli_connect($host,$nameuser,$password,$database);
		$sql = "SELECT * FROM 表  ORDER BY RAND() LIMIT 66";
		$result = mysqli_query($conn, $sql);
		$res1 = array();
		while($ress = mysqli_fetch_assoc($result)){
			$res1[] = $ress;
		}
		$str = json_encode($res1);
		echo $str;
```

然后就可以直接用了！

下面我也会将代码都发上来！

[资源下载](https://www.lanzous.com/i71rpwh)