---
title: TypeScript面向对象-寄存器,静态成员方法的使用
categories: 技术笔记
tags:
  - TypeScript
  - 面向对象
excerpt: TypeScript面向对象-寄存器,静态成员方法的使用
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgtypescriptmianxiangduix.png'
articleLink: 7afa5ecf
date: 2020-11-06 17:32:33
---

## 寄存器

有的时候，我们需要对类成员 `属性` 进行更加细腻的控制，就可以使用 `寄存器` 来完成这个需求

通过 `寄存器`，我们可以对类成员属性的访问进行拦截并加以控制

更好的控制成员属性的设置和访问边界

### getter

访问控制器，当访问指定成员属性时调用

### setter

### 实例

我们从一个没有使用存取器的例子开始

```
class Employee {
    fullName: string;
}
let employee = new Employee();
employee.fullName = "Bob Smith";
if (employee.fullName) {
    console.log(employee.fullName);
}
```

我们可以随意的设置 `fullName`，这是非常方便的，但是这也可能会带来麻烦。

下面这个版本里，我们先检查用户密码是否正确，然后再允许其修改员工信息。

我们把对 `fullName`的直接访问改成了可以检查密码的 `set`方法。

 我们也加了一个 `get`方法，让上面的例子仍然可以工作。

```
let passcode = "secret passcode";

class Employee {
    private _fullName: string;
		
    get fullName(): string {
        return this._fullName;
    }

    set fullName(newName: string) {
        if (passcode && passcode == "secret passcode") {
            this._fullName = newName;
        }
        else {
            console.log("Error: Unauthorized update of employee!");
        }
    }
}

let employee = new Employee();
employee.fullName = "Bob Smith";
if (employee.fullName) {
    alert(employee.fullName);
}
```

## 静态成员

前面我们说到的是成员属性和方法都是实例对象的

但是有的时候，我们需要给类本身添加成员，区分某成员是静态还是实例的：

- 该成员属性或方法是类型的特征还是实例化对象的特征
- 如果一个成员方法中没有使用或依赖 `this` ，那么该方法就是静态的

```typescript
type IAllowFileTypeList = 'png'|'gif'|'jpg'|'jpeg'|'webp';

class VIP extends User {
  
  // static 必须在 readonly 之前
  static readonly ALLOW_FILE_TYPE_LIST: Array<IAllowFileTypeList> = ['png','gif','jpg','jpeg','webp'];
  
  constructor(
  		id: number,
      username: string,
      private _allowFileTypes: Array<IAllowFileTypeList>
    ) {
        super(id, username);
  }
  
  info(): void {
    // 类的静态成员都是使用 类名.静态成员 来访问
    // VIP 这种类型的用户允许上传的所有类型有哪一些
    console.log(VIP.ALLOW_FILE_TYPE_LIST);
    // 当前这个 vip 用户允许上传类型有哪一些
    console.log(this._allowFileTypes);
  }
}

let vip1 = new VIP(1, 'zMouse', ['jpg','jpeg']);
// 类的静态成员都是使用 类名.静态成员 来访问
console.log(VIP.ALLOW_FILE_TYPE_LIST);
this.info();
```

- 类的静态成员是属于类的，所以不能通过实例对象（包括 this）来进行访问，而是直接通过类名访问（不管是类内还是类外）
- 静态成员也可以通过访问修饰符进行修饰
- 静态成员属性一般约定（非规定）全大写

