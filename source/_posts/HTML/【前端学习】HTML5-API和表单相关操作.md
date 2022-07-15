---
title: 【前端学习】HTML5-API和表单相关操作
categories: 技术笔记
tags:
  - HTML
  - 个人笔记
  - 剪切板
  - 表单
excerpt: >-
  表单验证 HTML5验证API 验证API的属性： 属性名称 描述 validity 一个ValidityState对象，描述元素的验证状态
  validity.customError 如果元素设置了自定义错误，返回true；否则返回fals
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200916140001.png'
articleLink: 88b516fc8e691414
date: 2020-05-14 07:48:14
---

## 表单验证 

#### HTML5验证API

验证API的属性：

| 属性名称                 | 描述                                                         |
| ------------------------ | ------------------------------------------------------------ |
| validity                 | 一个ValidityState对象，描述元素的验证状态                    |
| validity.customError     | 如果元素设置了自定义错误，返回true；否则返回false            |
| validity.patternMismatch | 如果元素的值不匹配所设置的正则表达式，返回true；否则返回false |
| validity.rangeOverflow   | 如果元素的值高于所设置的最大值，返回true，否则返回false      |
| validity.rangeUnderflow  | 如果元素的值低于所设置的最小值，返回true；否则返回false      |
| validity.stepMismatch    | 如果元素的值不符合step属性的规则，返回true；否则返回false    |
| validity.tooLong         | 如果元素额的值超过所设置的最大长度，返回true；否则返回false  |
| validity.typeMismatch    | 如果元素的值出现语法错误，返回true；否则返回false            |
| validity.valueMissing    | 如果元素设置了required属性且值为空，返回true；否则返回false  |
| validity.valid           | 如果元素的值不存在验证的问题，返回true；否则返回false        |

验证API的方法：

| 方法                       | 描述                                                         |
| -------------------------- | ------------------------------------------------------------ |
| checkValidity( )           | 如果元素的值不存在验证问题，返回true，否则返回false          |
| setCustomValidity(message) | 为元素添加一个自定义的错误消息；如果设置了自定义错误消息，则该元素被认为是无效的，并显示制定的错误 |

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <!-- 
        HTML表单组建元素的验证属性
        - required  验证当前元素不能为空
        - pattern  验证当前元素输入的是否是跟正则表达式相匹配
        

     -->
    <form action="#">
        <input type="text" id="username" required pattern="^[0-9a-zA-Z]{6-12}$">
        <input type="submit">
    </form>
    <script>
        var username = document.getElementById('username')
        console.log(username.validity);


        /*
        - validity属性由表单元素组建所提供；由此得到validityState对象，这个对象提供了一系列的验证属性
            setCustomValidity(message)
                - 作用：替换浏览器默认的错误提示信息
                - 注意：如果输入正确，必须将错误提示信息设置为空
        */
        username.addEventListener('blur', function () {
            if (username.validity.valueMissing) {
                username.setCustomValidity('不能为空')
            }else{
                username.setCustomValidity('')
            }

            /*
                patternMismatch属性
                    - 作用：配合元素中pattern属性来使用
            */
            if (username.validity.patternMismatch) {
                username.setCustomValidity('格式错误')
            }else{
                username.setCustomValidity('')
            }

            /*
                - rangeOverflow：配合元素中max属性使用
                - rangeUnderflow：配合元素中min属性使用
                - stepMismatc：配合元素中step属性使用
                - tooLong：配合元素中maxlength属性使用
                - tooShort：配合元素中minlength属性使用
                - typeMismatch：配合<input type="url">等元素来使用
                - customError：配合setCustomValidity()方法使用
                - valid：验证当前元素是否验证通过
                    * true：表示当前元素验证通过
                    * false：表示当前元素验证不通过
            */
         })

    </script>
</body>

</html>
```

## 表单提交

#### submit事件

提交表单时，会触发submi事件。

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <form action="" method="POST">
        <input type="text" name='data'>
        <input type="submit" value="提交">
    </form>
    <script>
        var form = document.forms[0];
        form.addEventListener('submit', function () {
            alert('表单已被提交')
        })
    </script>
</body>

</html>
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <form action="#">
        <input type="text" id="username">
        <input type="button" id="btn" value="提交">
    </form>
    <script>
        var btn = document.getElementById('btn')
        btn.addEventListener('click', function () {
            var form  = document.forms[0]
            form.submit()
            console.log("表单被提交");
            
        })
    </script>
</body>

</html>
```

**表单还提供了submit()方法用于提交表单，使用该方法时允许表单内使用任一普通按钮即可（并非提交按钮）**

