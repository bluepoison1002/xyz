新生项目课程：云计算环境下的博客系统开发
# 实验概述

1. 实验名称：简单博客系统的制作。
  
2. 实验目的：学习并掌握HTML、CSS和JavaScript的基础知识，能够独立制作一个简单的博客系统。
    

                     编程工具：文本编辑器

                     浏览器：Edge浏览器
# 实验内容

1. 使用HTML构建博客系统的基本结构；
  
2. 使用CSS对博客系统进行样式设计；
  
3. 使用JavaScript实现博客系统的交互功能。
# 实验步骤

1.通过JavaScript，实现博客系统最基本的功能，如增，删，改，查。

2.引入css库，并添加各个文段属性使blog更加美观。

3.给博客系统的内容页添加更多的功能，包括显示日期并将blog按保存时间排序。

4.给博客系统增加markdown和read more，并渲染markdown，调用marked库
### 代码具体讲解
#### 基础功能
##### 增加
```js
app.post('/new', async (req,res) => {
    one = new article({ title: req.body.title, description: req.body.description, markdown: req.body.markdown });
    await one.save();
    res.render('display', { article: one })
})
```
```js
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="/css/bootstrap.min.css" />
    <link rel="stylesheet" href="css/common.css">
    <title>Blog</title>
</head>
<body>
 <div class="container">
    <h1 class="mb-4">新文章</h1>

    <form action="/new" method="POST">
       <div class="form-group">
       <label for="title">标题</label>
       <input required type="text" name="title" id="title" class="form-control">
       </div>

        <div class="form-group">
        <label for="description">介绍</label>
        <textarea name="description" id="description" class="form-control"></textarea>
        </div>

        <div class="form-group">
        <label for="markdown">详情</label>
        <textarea name="markdown" id="markdown" class="form-control"></textarea>
        </div>

        <a href="/" class="btn btn-secondary">取消</a>
        <button type="submit" class="btn btn-primary">保存</button>
    </form>
 </div>

</body>
</html>
```
通过表单的形式，以POST方法来进行文章的新增，通过label来标识输入区域的性质，在标题区域用input实现单行输入，而在内容部分用textarea进行多文本输入，同时增加多个css属性，提高美观的同时贴近一般博客系统的输入模式
##### 删除
```js
app.delete('/:id', async (req, res) => {
  await article.deleteMany({ _id: req.params.id });
  res.redirect('/')
})
```
```js
 <form action="/<%= article._id %>?_method=DELETE" method="POST" class="d-inline">
   <button type="submit" class="btn btn-danger">删除</button>
 </form>
```
使用delete函数，清空数据，实现删除的功能
##### 修改
```js
app.put('/:id', async (req, res) => {
    let data = {}
    data.title = req.body.title
    data.description = req.body.description
    data.markdown = req.body.markdown

    var one = await article.findOne({ _id: req.params.id });
    if (one != null) {
        one.title = data.title;
        one.description = data.description;
        one.markdown = data.markdown;
        await one.save();       
    }  
    res.redirect(`/display/${req.params.id}`);
})
```
```js
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="/css/bootstrap.min.css" />
    <title>Blog</title>
</head>
<body>
    <div class="container">
        <h1 class="mb-4">修改文章</h1>

        <form action="/<%= article._id %>?_method=PUT" method="POST">
         <div class="form-group">
           <label for="title">标题</label>
           <input required value="<%= article.title %>" type="text" name="title" id="title" class="form-control">
         </div>

         <div class="form-group">
            <label for="description">介绍</label>
            <textarea name="description" id="description" class="form-control"><%= article.description %></textarea>
         </div>


         <div class="form-group">
            <label for="markdown">详情</label>
            <textarea name="markdown" id="markdown" class="form-control"><%= article.markdown %></textarea>
         </div>

        <a href="/" class="btn btn-secondary">取消</a>
        <button type="submit" class="btn btn-primary">保存</button>

        </form>
    </div>
</body>
</html>
```
同样是通过label来标识输入区域的性质，在标题区域用input实现单行输入，在内容部分用textarea进行多文本输入，使用put的方法将修改后的表单传递到数据库
##### 查看
```js
app.get('/display/:id', async (req, res) => {
    const one = await article.findOne({ _id: req.params.id });
    res.render('display', { article: one }) 
})
```
通过get将文章内容输出到display界面上
#### 新增功能
##### css增加
在views目录下的display，all，new,edit文件连接了具体的css库
```js
    <link rel="stylesheet" type="text/css" href="/css/bootstrap.min.css" />
```
在这些CSS样式中，给文章标题内容改变了格式，设置了页边距可读性更强，同时也为各个按钮和输入框设置了独特的样式，使得其看起来简洁而美观。
##### 其他css
```js
<link rel="stylesheet" href="css/common.css">
```
在all文件中，引入了一个其他的css文件
```js
.container a:hover {
    background: #333;
    color: #fff;
}
.container button:hover {
    background: #333;
    color: #fff;
}
```
在上述引入的css文件中，通过使用伪类选择器，实现光标悬停时修改样式的功能
##### 添加背景
```js
html,
body {
    height: 100%;
}
 
body {
    background-image: url(../jing.jpg);
    background-repeat: no-repeat;
    background-position: center center;
    background-size: cover;
}
 
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
```
给博客系统添加背景，并完成背景设置，使页面更加美观
##### 日期增加
```js
all.ejs
   <div class="card-subtitle text-muted mb-2">
            <%= article.createdAt.toLocaleDateString() %>
   </div>
```
```js
server.js
   createdAt: {type: Date,default: Date.now},
```
在server内新建属性createdAt，在all页面插入了article.createdAt的值，通过toLocaleDateString()函数，将日期对象转换为本地日期格式的字符串。
##### 日期排序
```js
app.get('/', async (req, res) => {
  const all = await article.find().sort({ createdAt: 'desc' });
  res.render('all', { articles: all })
})
```
接受到get请求后，在这里使用sort函数，将all页面所有文章按照时间排序
##### 增加markdown栏目（未实现功能）
```js
markdown:String,
```
在数据库中添加，创建markdown属性
```js
app.post('/new', async (req,res) => {
    one = new article({ title: req.body.title, description: req.body.description,markdown: req.body.markdown });
    await one.save();
    res.render('display', { article: one })
})
```
在post中增加对markdown的渲染
```js
<div class="form-group">
<label for="markdown">详情</label>
<textarea required name="markdown" id="markdown" class="form-control"></textarea>
</div>
```
增加markdown属性
##### 实现markdown功能
```js
   const marked = require('marked');

   html:String

   articleSchema.pre('validate', function(next) {
  if (this.markdown) {
    this.html = marked(this.markdown)
  }
 next()
})
```
定义marked函数库，创建html属性，在数据保存到数据库之前，检查markdown文本，调用函数自动转换为html并保存
```js
app.put('/:id', async (req, res) => {
    let data = {}
    data.title = req.body.title
    data.description = req.body.description
    data.markdown = req.body.markdown

    var one = await article.findOne({ _id: req.params.id });
    if (one != null) {
        one.title = data.title;
        one.description = data.description;
        one.markdown = data.markdown;
        await one.save();       
    }  
    // res.render('display', { article: data })
    res.redirect(`/display/${req.params.id}`);
})
```
在put中增加对于markdown的写入，同时为防止重复的get请求，使用新功能derict，在更新操作完成后，重定向到/display/:id路径
```js
<p><%- article.html %></p>
  ```
将markdown属性改为html属性，实现功能
# 实验结果
通过以上步骤，我们制作出了一个简单而美观，基础功能齐全博客系统。
| 工作量统计表 | 基础功能 | 新增功能1 | 新增功能2 | 新增功能3 | 新增功能4 | 新增功能5 | 新增功能6 | 新增功能7 |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 描述  | 博客系统中基础的增删改查操作 | 博客系统CSS美化 | 添加博客系统背景 | 增加时间属性 | 文章日期排序功能 | 增加markdown输入框 | 实现markdown功能 | 在display页面渲染markdown | 
| 学时  | 8   | 2   | 1   | 2   | 1   | 2   | 2   | 2   | 












