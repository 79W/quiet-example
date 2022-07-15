---
title: 自己第一个vue项目通讯录作业-模仿通讯录用户列表
categories: 项目案例
tags:
  - HTML
  - 个人笔记
  - 作业练习
  - JS
  - vue
  - vue.js
  - 作业
  - 通讯录
excerpt: 自学vue框架!第一眼的感觉是很简单很便捷然后就潦草的看了点视频！然后就有了这个作业然后就试着用vue写后来也有很多的难点
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5cc2c0bb9bc68.jpg'
articleLink: 370df43e2af64810
date: 2018-12-10 11:12:48
---

![自己第一个vue项目通讯录作业](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5cc2c0bb9bc68.jpg "自己第一个vue项目通讯录作业")

#### 自学vue框架!第一眼的 感觉是很简单 很便捷 然后就潦草的看 了点视频！

#### 然后就有了这个作业 然后就试着用vue写 后来也有很多的难点 就开始不断的找人去问！

#### 一点点的也学到了很多知识！熬了2个夜晚才完成的！

### **总重要的一点就是要多看 官方文档！ 官方文档！ 官方文档！**

 

先把前端css写完然后在嵌入VUE.js

  前端页面就没什么好说的了！上js！

* * *

```
var app = new Vue({
    el: '#app',
    data: {
        /**第1 第2  */
        title: '',
        name:'',
        /*显隐 */
        zeng:false,
        wenzi:true,
        xiu:false,
        /* */
        styles:'nei',
        gnjiankongzhi:'contact1_moban2',
        diankongzhi : 'sandian',
        /*存进去 */
        list: [],
        sjshu:'',
        /** */
        index:-1,
        touxiang:['img/tx/0.jpg','img/tx/1.jpg','img/tx/2.jpg','img/tx/3.jpg','img/tx/4.jpg','img/tx/5.jpg','img/tx/6.jpg','img/tx/7.jpg','img/tx/8.jpg','img/tx/9.jpg']
    },
    methods: {
        addFun() {
            if(this.title!='' || this.name!=''){
                /*存己去 push */
                this.list.push({
                    title: this.title,
                    name:  this.name,
                    /*头像 */
                    url:this.touxiang[this.sjshu],
                    sfbj:false
                });
                this.title='',
                this.name=''
                this.zeng=false;
                this.wenzi=false;
                this.styles='nei'
            }
        },
        zengjia(){
                 /**显隐  */
                this.zeng=true;
                this.styles='nei2'
                /**随机数  */
                this.sjshu = Math.floor(Math.random() *10);
                /*箭头函数 function */
                this.list.map((item,i)=>{
                    item.sfbj=false
                    return item
                })
                /**扩展运算符  spread 新数 */
                this.list = [...this.list]   
                /*为空 */
                this.title = "" 
                this.name = ""
        },
        quxiao(){
            this.zeng=false
            this.xiu=false 
            this.styles='nei'
        },
        sandian(index){
             /*箭头函数=> function */
            /**map for */
            this.list.map((item,i)=>{
                /**注释 */
                if(index==i){
                    /**sfbj 状态 是否编辑 */
                    item.sfbj=!item.sfbj
                }
                else{
                    item.sfbj=false
                }
                return item
            })
            /**扩展运算符  spread 新数 */
            this.list = [...this.list]    
            /*为空 */
            this.title = "" 
            this.name = ""        
        },
        /*删除 */
        deleteInfo(index){
            this.list.splice(index,1);
        },
        bianjixia(index){
            this.styles='nei2'
            this.xiu=true;
            this.index = index
        },
        addFun2(){
            this.styles='nei'
            this.xiu=false 
            /*箭头函数=> function */
            /**map for */
            this.list.map((item,i)=>{
                if(i==this.index){
                    item.title = this.title
                    item.name = this.name
                    item.url = this.touxiang[this.sjshu]
                }
                return item
            })
            /**扩展运算符  spread   新数 */
            this.list = [...this.list]
        },
        sjtouxiang(){
            this.sjshu = Math.floor(Math.random() *10);
        },

        
    },


})

```



[资源下载](https://www.lanzous.com/i2l2ryb)

