### vue-pos项目笔记
#### 使用Iconfont图标
##### 挑选图标的过程（共6步）
- 进入网站：Iconfont网址：http://www.iconfont.cn
- 点击网站上方的“官方图标库”，选择自己喜欢的图标。在这里我选择天猫的图标库。
- 选择好自己喜欢的图标，你可以有两个选择，下载代码 和 添加至项目。
- 我们这两选择添加至项目，然后新建项目，并输入名称。
- 项目添加好后，会自动给我们转入到我们项目库中。点击查看在线链接。
- 生产css引入的代码，生成后就可以在项目首页index.html引入了。
 `<link rel="stylesheet" href="http://at.alicdn.com/t/font_wyhhdpv5lhvbzkt9.css">`
##### 图标的使用：

- 图标顺利引入到项目中，已经可以使用它们了，在“我的项目中”你会看到图标的font class值。可以直接复制代码粘贴，也可以自己写代码。
`<i class="icon iconfont icon-hanbao"></i>`

- 这样在页面中就可以看到图标了。

##### 添加更多图标：
- 如果在项目中觉的图标不够用了，需要添加更多图标。可以利用下面四步进行添加。

* 去Iconfont网站继续挑选，把相中的图标加入购物车中。
* 把购物车中的图标加入到项目中。
* 重新生成在线链接。（这部很重要）
* 在项目主页中(index.html)，更换css引入链接。
---
### 解决100%高的问题

- 在页面中使用了Element组件，这样他会自动给我们生产虚拟DOM，我们无法设置高度100%；

- 这时候可以利用javascript，来设置100%高度问题。先要给我们的<el-col>标签上添加一个id，我们这里把ID设置为order-list。然后在vue构造器里使用mounted钩子函数来设置高度。
```
  mounted:function(){
      var orderHeight=document.body.clientHeight;
      document.getElementById("order-list").style.height=orderHeight+'px';
  },
```
