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



















