---
title: 简书Api接口的展示与React相结合项目练习
categories: 项目案例
tags:
  - React
  - 简书Api
excerpt: 简书Api接口的展示与React相结合项目练习
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201119205317.png'
articleLink: 88e76eca
date: 2020-11-19 19:31:20
---

本Api全部是简书提供的Api

起因：最近在学习React 想写个小项目练练 网上找了很多很多 感觉简书这个还不错 于是我就抓了抓

我会分几个部分来进行 说明这个Api （完全参考简书）

React Dome演示

[React 源代码下载](https://wwa.lanzous.com/i9xVjik4tde)

## 首页

首先我来查看简书的首页布局是什么样的

![jianshuindex](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgjianshuindex.png)

我们给首页分了几块（我自己分的）

![jianshuindexxian](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgjianshuindexxian.png)

我们知道首页的布局了那么我们来看Api有什么

### 热门专题推荐

也就是首页上面的专题展示

#### 请求：

`https://www.jianshu.com/asimov/subscriptions/recommended_collections`

#### 描述：

获取简书手机网页的热门专题，会显示当前推荐的7个专题

#### 响应实例：

```
[
    {
        "id": 4,
        "image_url": "http://upload.jianshu.io/collections/images/4/sy_20091020135145113016.jpg",
        "title": "读书",
        "slug": "yD9GAd"
    },
  ...
]
```

#### 分析：

- id: 专题的id号(同文章id一样用处不明)
- image_url: 每个专题都有一张配图，这就是配图的地址
- title: 专题的名称
- slug: 专题的地址 (用法: `https://www.jianshu.com/asimov/collections/slug/7b2be866f564`)

### 首页文章

#### 请求：

`https://www.jianshu.com/asimov/trending/now`

#### 描述：

获取当前的最新文章默认显示最新20条（随机的）

#### 响应实例：

```
[
    {
        "object": {
            "type": 1,
            "data": {
                "id": 39771634,
                "title": "Python模拟登陆万能法selenium来执行javascript命令，来避开封锁",
                "slug": "61bfbd924d69",
                "list_image_url": "http://upload-images.jianshu.io/upload_images/14581851-a55e957ebc1aee86",
                "public_abbr": "今天分享一下解决方案。就是通过让selenium来执行javascript命令，来避开封锁。（此处应该有掌声） 文...",
                "commentable": true,
                "important_collection": null,
                "user": {
                    "id": 2642813,
                    "nickname": "loonslo",
                    "slug": "906f252b4c34",
                    "avatar": "http://upload.jianshu.io/users/upload_avatars/2642813/99d8f83542fd"
                },
                "total_fp_amount": 4788,
                "public_comments_count": 0,
                "total_rewards_count": 0,
                "likes_count": 21
            }
        }
    },
    ...
]
```

#### 分析：

- type: 作用未知
- data
  - id: 文章的id (这个id其实没啥用)
  - title: 文章的标题
  - slug: 文章真正的地址 (用法：`https://www.jianshu.com/p/61bfbd924d69` )
  - list_image_url: 文章的配图，没有图片则为空
  - public_abbr: 文章的预读部分
  - commentable: 是否可评论
  - important_collection: 用处未知
- user:
  - id: 用户的id
  - nickname: 用户昵称
  - slug: 用户的主页 (用法：`https://www.jianshu.com/u/b7da6852198a` )
  - avatar: 用户头像
- total_fp_amount: 当前文章的字数
- public_comments_count: 用户评论数量
- total_rewards_count: 收到的赞赏数量
- likes_count: 收到的喜欢

### 推荐作者

#### 请求：

`https://www.jianshu.com/users/recommended?seen_ids=&count=5&only_unfollowed=true`

#### 描述：

获取右侧的推荐作者（其实那个随机显示作者的就是这个seen_ids=不知道是什么没办法使用）

#### 请求头：

`Accept:*/*`

#### 响应实例：

```
{
    "users": [
        {
            "id": 9988193,
            "slug": "51b4ef597b53",
            "nickname": "董克平日记",
            "avatar_source": "https://upload.jianshu.io/users/upload_avatars/9988193/fc26c109-1ae6-4327-a298-2def343e9cd8.jpg",
            "total_likes_count": 5134,
            "total_wordage": 1163732,
            "is_following_user": false
        },
    ],
    "total_count": 43532
}
```

#### 分析：

- slug：用户id
- nickname：用户名称
- avatar_source：用户头像
- total_likes_count：喜欢数量
- total_wordage：字数

我们首页就这三个就可以完成了！

接下来是我们文章页面的编写

## 文章页

文章页面有文章-用户信息-推荐相似文章-评论

![article](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgarticle.png)

评论在下面我就没有截图

### 文章内容

#### 请求：

`https://www.jianshu.com/asimov/p/c475ed97ad95`

 (c475ed97ad95用上个api获取到的slug替代)

#### 描述：

获取查看的文章具体内容（这里也会把作者信息也传递回来）

#### 响应实例：

```
{
    "is_author": false,
    "liked_note": false,
    "public_comment_count": 0,
    "featured_comments_count": 0,
    "likes_count": 0,
    "trial_open": false,
    "description": "个人练手项目： 仅保留了每日热门与详情. 下为实际截图： 数据来源请点击这里 项目地址请点击这里 ",
    "gupao_eligible": 0,
    "total_fp_amount": 0,
    "id": 14545899,
    "slug": "c475ed97ad95",
    "public_title": "仿-（极简）知乎日报",
    "free_content": "<h1>个人练手项目：</h1>....项目地址请点击这里</a>    </h4>",
    "paid_type": "free",
    "first_shared_at": "2017-07-13T22:06:26.000+08:00",
    "notebook_id": 5463424,
    "commentable": true,
    "wordage": 43,
    "paid_content_accessible": false,
    "share_image_url": "http://upload-images.jianshu.io/upload_images/2642813-33bc390691a5a4f7.png",
    "show_paid_comment_tips": false,
    "user": {
        "id": 2642813,
        "slug": "b7da6852198a",
        "nickname": "loonslo",
        "gender": 2,
        "avatar": "http://upload.jianshu.io/users/upload_avatars/2642813/99d8f83542fd",
        "intro": "哈哈哈哈",
        "wordage": 629,
        "likes_count": 0,
        "badges": [],
        "vip": null,
        "liked_by_user": false,
        "liked_user": false
    },
    "distribution_more_earn_percent": 24,
    "last_updated_at": 1512737246
}
```

#### 分析：

- is_author: 是否作者
- liked_note: 是否为喜欢的文章
- public_comment_count": 评论数量
- featured_comments_count": 未知(可能是赞赏数量吧)
- likes_count": 点了喜欢的人数
- trial_open": 用处未知
- description": 文章描述
- gupao_eligible": 0,
- total_fp_amount": 0,
- id": 文章id
- slug": 文章的真正id
- public_title": 文章标题
- free_content": html的文章正文
- paid_type": 未知(可能和推广有关吧…)
- first_shared_at": 文章初始发布时间
- notebook_id": 文章所属的文件夹id
- commentable": 是否可评论
- wordage": 文章的字数
- paid_content_accessible": 是否可赞赏(估计得登录后才是true吧)
- share_image_url": 文章的预览图地址
- show_paid_comment_ips": 用处未知
- user
  - id": 用户id,
  - slug": “用户的真实id”,
  - nickname": “用户名称”,
  - gender": 性别,
  - avatar": 头像
  - intro": "用户的个人简介
  - wordage": 该用户写的总字数
  - likes_count": 被喜欢数量
  - badges": 未知
  - vip": null,
  - liked_by_user": 未知
  - liked_user": 未知
- distribution_more_earn_percent": 不清楚，是指赚了多少钱吗？
- last_updated_at:最后更新时间(时间戳)

### 作者下面的文章

#### 请求：

`https://www.jianshu.com/shakespeare/notes/79260753/user_notes`

#### 描述：

获取作者下面的热门文章

#### 响应实例：

```
[
    {
        "id":78072297,
        "slug":"91a3b5e37591",
        "title":"矛盾",
        "view_count":917,
        "user":{
            "id":17548494,
            "nickname":"努力的天使姐姐",
            "slug":"27aacac0c762",
            "avatar":"https://upload.jianshu.io/users/upload_avatars/17548494/2473dc78-a098-4a06-8e11-742ed1a5dd72.jpg"
        }
    },
]
```

#### 分析：

- slug：文章id
- title：文章名称
- view_count：阅读数量

### 系统推荐相似文章

#### 请求：

`https://www.jianshu.com/shakespeare/notes/267077f02631/recommendations`

#### 描述：

根据当前文章给你推荐一些文章

#### 响应实例：

```
[
    {
        "id":78229168,
        "slug":"8f6ae9385bc3",
        "list_image_url":"",
        "title":"远离私生活混乱的人",
        "views_count":8730
    },
]
```

#### 分析：

- slug：文章id
- title：文章名称
- views_count：阅读数量
- list_image_url：应该是缩略图

### 获取文章的类别

#### 请求：

`https://www.jianshu.com/shakespeare/v2/notes/f72004f46d08/book`

#### 描述：

也就是作者创建的一个分类吧然后可以获取文章属于那个

#### 响应实例：

```
{
    "notebook_id":31371194,
    "notebook_name":"日记本",
    "liked_by_user":false
}
```

#### 分析：

- notebook_id：类别id
- notebook_name：类别名称

### 文章评论

#### 请求：

`https://www.jianshu.com/shakespeare/notes/76616286/comments?page=1&count=10&author_only=false&order_by=desc`

#### 参数：

- 76616286 ：请替换文章id 不是那个slug
- page ： 页数
- count：每次请求过来多少个
- author_only
- order_by ：排序

#### 响应实例：

```
{
    "comments":[
        {
            "id":62546270,
            "created_at":"2020-11-19T11:33:03.000+08:00",
            "compiled_content":"男男女",
            "floor":18,
            "parent_id":null,
            "images_count":0,
            "images":[

            ],
            "likes_count":0,
            "children_count":0,
            "liked":false,
            "user":{
                "id":24886329,
                "nickname":"一米娱乐246",
                "slug":"0b9aab718337",
                "avatar":"https://upload.jianshu.io/users/upload_avatars/24886329/d3c67ad3-f122-45d2-9cb2-b149286e65ab.png"
            },
            "children":[

            ]
        },  
    ],
    "page":1,
    "total_pages":2
}
```

#### 分析：

- created_at：时间
- compiled_content：评论内容
- user ：用户信息
- children：子评论
- likes_count：喜欢的数量

### 文章专题

#### 请求：

`https://www.jianshu.com/shakespeare/notes/76616286/included_collections?page=1&count=100`

#### 描述：

获取这篇文章被那个专题收录了可以参考简书怎么写的

（我们找到专题详情的接口 有的可以分享下）

#### 响应实例：

```
{
    "collections":[
        {
            "id":1793792,
            "slug":"2e7f1ec37cd8",
            "title":"心情日记",
            "avatar":"https://upload.jianshu.io/collections/images/1793792/1561961719.jpg",
            "owner_name":"猫女乖且闲Kitty"
        },
    ],
    "page":1,
    "total_pages":1
}
```

#### 分析：

- slug：id
- title ：名称
- avatar：图片
- owner_name：创建人的名字吧

## 用户页

![user](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imguser.png)

### 用户信息

#### 请求：

`https://www.jianshu.com/asimov/users/slug/b7da6852198a/`

将b7da6852198a替换成要查询用户的slug即可

#### 描述：

获取该用户的个人介绍/设置

#### 响应实例：

```
{
    "wechat_reward_url": "",
    "id": 2642813,
    "slug": "b7da6852198a",
    "nickname": "loonslo",
    "intro": "哈哈哈哈",
    "intro_compiled": "哈哈哈哈",
    "gender": 2,
    "avatar": "http://upload.jianshu.io/users/upload_avatars/2642813/99d8f83542fd",
    "background_image": null,
    "badges": [],
    "following_users_count": 0,
    "followers_count": 0,
    "total_wordage": 629,
    "total_likes_count": 0,
    "following_user": true,
    "followed_by_user": true,
    "jsd_balance": 1002,
    "last_updated_at": 1552642020
}
```

#### 分析：

- wechat_reward_url: 微信授权登录相关信息
- id: 用户id（不清楚啥用）
- slug: 用户的实际id，查询用户相关都是用这个slug
- nickname: 用户昵称
- intro: 用户的个人介绍
- intro_compiled": 具体的个人介绍
- gender": 性别
- avatar": 头像图片
- background_image": 背景图，没有则为空
- badges": 用处不明
- following_users_count": 关注的用户数量
- followers_count": 被关注的数量
- total_wordage": 总共写的字数
- total_likes_count": 总共被喜欢的数量
- following_user": 用处未知（怀疑是接收关注人的简书动态）
- followed_by_user": 同理（怀疑是接收被关注人的动态）
- jsd_balance": 用处未知
- last_updated_at": 最后登录的时间，显示为时间戳

### 获取用户专题文集

#### 请求：

`https://www.jianshu.com/users/394b381154d7/collections_and_notebooks?slug=394b381154d7`

#### 描述：

这个可以获取到用户创建的专题用户管理的专题还有用户的文集

#### 响应实例：

```
{
    "own_collections": [
        {
            "id": 1847580,
            "slug": "84cac61c716e",
            "title": "提升自己",
            "avatar": "https://upload.jianshu.io/collections/images/1847580/crop1577889916843.jpg"
        }
    ],
    "own_collections_page": 1,
    "own_collections_total_pages": 1,
    "manageable_collections": [
        {
            "id": 1851868,
            "slug": "86897ca17c7d",
            "title": "文学与生活",
            "avatar": "https://upload.jianshu.io/collections/images/1851868/crop1579960565257.jpg"
        },
        {
            "id": 1838364,
            "slug": "99f7e2ad7f63",
            "title": "悦目娱心 ",
            "avatar": "https://upload.jianshu.io/collections/images/1838364/crop1589103234285.jpg"
        }
    ],
    "manageable_collections_page": 1,
    "manageable_collections_total_pages": 1,
    "notebooks": [
        {
            "id": 35145308,
            "name": "日记本",
            "book": false
        }
    ],
    "notebooks_page": 1,
    "notebooks_total_pages": 1
}
```

#### 分析：

- own_collections：创建的专题
- manageable_collections：管理的专题
- notebooks：创建的文集

### 用户文章获取

#### 请求：

`https://www.jianshu.com/asimov/users/slug/b7da6852198a/public_notes`

#### 请求参数：

- order_by：top 获取热门文章
- rder_by ： commented_at 最新被评论的文章
- order_by： shared_at 最新发布的文章
- 什么都不传递 就是全部

## 搜索接口

#### 请求：

`ttps://www.jianshu.com/search/do?q=乔越博客&type=note&page=1&order_by=default`

#### 请求参数：

- type：note｜user｜collection｜notebook
  - 分别对应 默认 用户 专题 文集
- order_by：default  排序有多

#### 响应实例：

```
{
    "q":"乔越",
    "page":1,
    "type":"note",
    "total_count":10000,
    "per_page":10,
    "total_pages":100,
    "order_by":"default",
    "entries":[
        {
            "id":15520021,
            "title":"《楚<em class='search-result-highlight'>乔</em>传》：<em class='search-result-highlight'>越</em>通透的女子，越能一世单纯",
            "slug":"8d5277796a3b",
            "content":"……燕洵全家被魏帝杀死，九幽台上，楚<em class='search-result-highlight'>乔</em>生死相随；莺歌院里，楚<em class='search-result-highlight'>乔</em>与燕洵精心谋划，反出大魏、重返燕北。……05 所以，<em class='search-result-highlight'>越</em>通透的女子，越能看穿纷芜繁杂的世事，在鸡零狗碎的生活中找到自己想要的东西； 越能跨越磨难、坚强生活、在重重迷雾中挣脱重围、寻找真我； 越能坚守自己内心的一亩三分地，保持最初单纯简单的自我，……<em class='search-result-highlight'>越</em>通透的女子，越能一世单纯。（全文完）……",
            "user":{
                "id":5656074,
                "nickname":"柒菇qigu",
                "slug":"032646703dd4",
                "avatar_url":"https://upload.jianshu.io/users/upload_avatars/5656074/88310b7c-ae81-494e-9066-34796eff4ead.jpg"
            },
            "notebook":{
                "id":15367791,
                "name":"观点"
            },
            "commentable":true,
            "public_comments_count":0,
            "likes_count":8,
            "views_count":322,
            "total_rewards_count":0,
            "first_shared_at":"2017-08-08T21:25:34.000Z"
        },
      
    ],
    "related_users":[
        {
            "id":2243684,
            "avatar_url":"https://upload.jianshu.io/users/upload_avatars/2243684/709b1ef9863c",
            "nickname":"乔越",
            "slug":"1dac2dfadba7",
            "total_wordage":0,
            "total_likes_count":0
        },
    ],
    "more_related_users":true,
    "related_collections":[

    ],
    "more_related_collections":false
}
```

#### 分析：

- q：搜索词
- type：类型
- entries：文章
- related_users：用户

还有很多的api等待开发 

> 如果有什么不妥的地方联系我进行更改或者删除！！！