---
title: 百度OCR对接实现高精度文字识别OCRAPI和解决各种坑
categories: 项目案例
tags:
  - HTML
  - PHP
  - 资源分享
  - baiduocr
  - ocr
  - 在线文字识别
  - 文字识别
excerpt: 开发需求 对图片上的文字进行识别和提取 对上传的图片进行服务器存储 将文字和图片外链进行显示 百度OCR 百度OCR链接
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200916132637.png'
articleLink: 040d89d8535e0026
date: 2020-08-26 10:23:00
---

## 开发需求

1.  对图片上的文字进行识别和提取
2.  对上传的图片进行服务器存储
3.  将文字和图片外链进行显示

## 百度OCR

[百度OCR链接](https://www.79bk.cn/go/?url=aHR0cHM6Ly9jbG91ZC5iYWlkdS5jb20vZG9jL09DUi9zLzFrM2g3eTNkYg==)

我们先下载sdk我这里用的是php

### 通用文字识别（高精度版）

用户向服务请求识别某张图中的所有文字，相对于通用文字识别该产品精度更高，但是识别耗时会稍长。

```php
$image = file_get_contents('example.jpg');

// 调用通用文字识别（高精度版）
$client->basicAccurate($image);

// 如果有可选参数
$options = array();
$options["detect_direction"] = "true";
$options["probability"] = "true";

// 带参数调用通用文字识别（高精度版）
$client->basicAccurate($image, $options);
```

**通用文字识别（高精度版） 请求参数详情**

| 参数名称         | 是否必选 | 类型   | 可选值范围 | 默认值 | 说明                                                         |
| ---------------- | -------- | ------ | ---------- | ------ | ------------------------------------------------------------ |
| image            | 是       | string |            |        | 图像数据，base64编码，要求base64编码后大小不超过4M，最短边至少15px，最长边最大4096px,支持jpg/png/bmp格式 |
| detect_direction | 否       | string | true false | false  | 是否检测图像朝向，默认不检测，即：false。朝向是指输入图像是正常方向、逆时针旋转90/180/270度。可选值包括: - true：检测朝向； - false：不检测朝向。 |
| probability      | 否       | string | true false |        | 是否返回识别结果中每一行的置信度                             |

**通用文字识别（高精度版） 返回数据参数详情**

| 字段             | 必选 | 类型   | 说明                                                         |
| ---------------- | ---- | ------ | ------------------------------------------------------------ |
| direction        | 否   | number | 图像方向，当detect_direction=true时存在。 - -1:未定义， - 0:正向， - 1: 逆时针90度， - 2:逆时针180度， - 3:逆时针270度 |
| log_id           | 是   | number | 唯一的log id，用于问题定位                                   |
| words_result_num | 是   | number | 识别结果数，表示words_result的元素个数                       |
| words_result     | 是   | array  | 定位和识别结果数组                                           |
| +words           | 否   | string | 识别结果字符串                                               |
| probability      | 否   | object | 行置信度信息；如果输入参数 probability = true 则输出         |
| +average         | 否   | number | 行置信度平均值                                               |
| +variance        | 否   | number | 行置信度方差                                                 |
| +min             | 否   | number | 行置信度最小值                                               |

**通用文字识别（高精度版） 返回示例**

```
{
"log_id": 2471272194,
"words_result_num": 2,
"words_result":
    [
        {"words": " 乔越博客"},
        {"words": "wwww.79bk.cn"}
    ]
}
```

## 开始开发

1.  创建文件将sdk放进去
2.  创建首页 index.html
3.  创建自己的php `myOcr.php` 后期会讲这个是什么

### index.html 文件说明

这个文件是首页展示的样子

#### HTML 结构

我将html分为中间一个整体 然后在整体里面分为上中下三个部分

在上面分为 标题H1 上传图片按钮

中间为识别内容的区域

底部为图片上传返回的url和图片到区域

```html
<div class="wrap">
		<h1 class="h1">Change.Q</h1>
        <div class="inputc">上传图片
            <input type="file" name="imagefile" onchange="uploadImage(event)" style="opacity: 0;">
        </div>
        <div class="type-wrap">
            <div id="typed-strings">
                <span class="text">请上传图片哦！</span>
            </div>
		</div>
		<div class="ImageUrl">
			<div class="imgShow">
				<img id="UPimg" src="" alt="乔越博客">
			</div>
			<div class="urlList">
				
				<div class="okURL">
					URL：<span class="textURL">https://www.79bk.cn</span>
				</div>
				<!-- <div class="okHTML">
					HTMl：<span>http://www.baidu.com</span>
				</div>
				<div class="okMD">
					Markdown：<span>http://www.baidu.com</span>
				</div> -->
			</div>
		</div>

    </div>
```

### CSS

css通俗的说就是给这个页面装修下 你只写了html的话那就是个毛坯房

css样式你就可以随心所欲了你喜欢什么样子就做成什么样子这个可以自己进行

```css
@import url(http://fonts.googleapis.com/css?family=Ubuntu:400,500);

*{
	padding:0;
	margin:0;
}

body{
	font-family: "Ubuntu", sans-serif;
	font-size: 100%;
	background:#f8f8f8;
}

a{
	text-decoration: none;
	color:#666;
}
	a:hover{
		color:#999;
	}
p{
	line-height: 2em;
	margin:0 0 20px;
	text-align: center;
}

.wrap{
	max-width: 600px;
	margin:150px auto;
}

.type-wrap{
	margin:10px auto;
	padding:20px;
	background:#f0f0f0;
	border-radius:5px;
	border:#CCC 1px solid;
}

.links{
	margin:20px 0;
	font-size: 0.75em;
	text-align: center;
}
h1{
	display: inline-block;
}
input{
	width: 70px;
	height: 25px;
	position: relative;
	top: -26px;


}
.inputc{
	width: 70px;
	height: 25px;
	float: right;
	margin-top:10px;
	cursor:pointer;
	border:1px solid rgb(168, 170, 168);
	text-align: center;
	font-size: 15px;
	line-height: 25px;
	border-radius:4px;
	color: rgb(70, 70, 70);

}
.inputc:hover{
	background: #333435;
	border:1px solid #333435;
	color: white;
}



.ImageUrl{
	margin:10px auto;
	padding:20px;
	background:#f0f0f0;
	border-radius:3px;
	border:#CCC 1px solid;
	opacity: 0;
}
.imgShow{
	text-align: center;
	padding-bottom: 20px;
}
.imgShow img{
	max-width: 88%;
	height: auto;
}
.urlList{
	padding-bottom:15px ;
	/* background:blue; */
	/* display: inline-block; */
	
}
.urlList .okURL,.okHTML,.okMD{
	margin: 0 auto ;
	width: 88%;
	height:40px;
	border:#CCC 1px solid;
	margin-top:10px;
	color: rgb(124, 124, 124);
	line-height: 40px;
	padding-left:10px ;
	border-radius:3px;
	overflow:hidden; 
	text-overflow:ellipsis; 
	white-space:nowrap; 
}
```

### JavaScript

这个就比较重要了，因为需要和 myOcr.php进行交互

#### uploadImage()

这个方法是将图片发送给 myOcr.php文件

首先我们进行获取你所点击的event对象 在对象中获取文件的属性

创建一个Form表单进行传递

我这里用的是js原生的请求方式 大家可以用jquery的请求会看着简单方便一些

```js
    		// 获取图片信息
    		var image = event.target.files[0];
    		//创建表单
    		var formData = new FormData();
    		//将文件添加进去
			formData.append("imageFile", image);
			//创建请求
			var xhr = new XMLHttpRequest();
			//打开连接
			xhr.open("post", "myOcr.php");
    		//发送数据
			xhr.send(formData);
 
    		xhr.onreadystatechange = function () {//请求后的回调接口，可将请求成功后要执行的程序写在其中
			    if (xhr.readyState == 4 && xhr.status == 200) {//验证请求是否发送成功
			        var json = xhr.responseText;//获取到服务端返回的数据
			        // console.log(json);
			        show(json) //查看数据的
			        addImageUrl(json) //处理图片url
			    }
			}
```

#### show()

接受数据然后进行格式化

```js
//接受到数据然后转换
var data = JSON.parse(json)
//	获取文字
			var res = data["words_result"]
			//创建个空的后面进行拼接使用
	        var str = ''
	      //循环
			for (var i = 0;i<res.length;i++) {
			//拼接
				str += res[i]['words'] + "<br/>"
			}
			//将 拼接好的文字内容进行渲染到首页上去
			document.getElementsByClassName("text")[0].innerHTML = str
```

#### addImageUrl()

```js
var data = JSON.parse(json)
			//找出后缀
			var res = data["imageUrl"]
			//拼接当前url
			var URL = window.location.href+res
      //把url渲染到首页
			document.getElementById('UPimg').src = URL;
//再次显示
			document.getElementsByClassName("textURL")[0].innerHTML = URL
//改变底部的显隐度 给显示出来
			document.getElementsByClassName("ImageUrl")[0].style.opacity = 1
```

### myOcr.php

这个是 前端html和百度ocr交互的一个文件

首先 index.html 将数据图片传递给 myOcr.php 然后他在传给百度ocr

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200916132637.png)

接收到前段传过来的数据进行判断是否有文件 有文件在判断是否是图片

一系列完成后 将图片存储起来 然后获取存储的路径

获取到路径 后传递给百度Ocr

```php
//判断是否有文件传输
if(empty($_FILES["imageFile"])){
    //为空直接返回错误
    echo "请上传图片";
    //结束
    die();
}else{
    //有东西
    $image = $_FILES['imageFile'];
    //判断后缀名
	$image_extension = array("png","jpeg","jpeg");
	//以数组返回
	$data = (pathinfo($image["name"]));
	//判断是否有这个后缀
	if(!in_array($data['extension'], $image_extension)){
		echo "请上传正确的图片类型";
		die();
	}
}

```

## 坑

我首先是不想利用 php 获取node等服务因为百度ocr的文档直接提供了 api接口 而就是这个接口让我浪费整个半天

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200916132721.png)

**请求示例**

HTTP 方法：`POST`

请求URL： `https://aip.baidubce.com/rest/2.0/ocr/v1/accurate_basic`

我刚开始的时候先用postman进行测试 嗯很好用 反应等都很快

然后我就愉快的进行开发了

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200916132744.png)

开发我不论怎么进行请求都会显示跨域请求就是不会请求成功

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200916132803.png)

而我们正在学习怎么解决跨域请求 嗯 啥用没有都是在服务端进行解决的前端 jsonp 是不显示跨域显示错误 在我使用多种办法都没办法解决的情况下我 就去百度提交了工单

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200916132913.png)

然后工程师回复我了

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200916132941.png)

TMD不支持跨域你不能在文档写下吗？？？？

也就是说你怎么请求都不会请求过去的 然后我就换成了node服务进行编写 为啥用node呢？因为我们刚学完node 我在写node然后脑海里面一直想个事部署好麻烦哈哈哈哈 然后我就用了php这个世界上最好的语言（好久没用过了好手生 ）一些语法都忘记了 现学习有点高成本 然后在看各种参考代码就写完了！

其实百度ocr已经把php代码给你写完了！ 直接复制就行哈哈哈哈哈！又是偷懒的一天！

[GitHub](https://github.com/duogongneng/ChangeQ)

