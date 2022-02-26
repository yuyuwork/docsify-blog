
<!-- # 使用docsify搭建博客 -->

?> [docsify](https://docsify.js.org/#/zh-cn/) 可以快速帮你生成文档网站。不同于 GitBook、Hexo 的地方是它不会生成静态的 `.html` 文件，所有转换工作都是在运行时。如果你想要开始使用它，只需要创建一个 `index.html` 就可以开始编写文档并直接部署在 GitHub Pages


### 快速开始 <!-- {docsify-ignore} -->

**安装docsify-cli**

推荐全局安装 `docsify-cli` 工具，可以方便地创建及在本地预览生成的文档

```bash
npm i docsify-cli -g
```

**初始化项目**

如果想在项目的 `./docs` 目录里写文档，直接通过 `init` 初始化项目。

```bash
docsify init ./docs
```

**开始写文档**

初始化成功后，可以看到 `./docs` 目录下创建的几个文件

- `index.html` 入口文件，也叫配置文件，相关配置均在其中；
- `README.md` 会做为主页内容渲染
- `.nojekyll` 用于防止 GitHub Pages 忽略掉下划线开头的文件

直接编辑 `docs/README.md` 就能更新文档内容


**本地预览**

通过运行 `docsify serve` 启动一个本地服务器，可以方便地实时预览效果。默认访问地址 http://localhost:3000 

```bash
docsify serve docs
```

### 项目配置 <!-- {docsify-ignore} -->

配置文件 `index.html` 中主要配置了文档网站的名字以及开启一些配置选项

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Java技术栈</title>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <meta name="description" content="Description">
  <meta name="viewport" content="width=device-width, user-scalable=no, 
    initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
  <link rel="stylesheet" href="//unpkg.com/docsify/lib/themes/dark.css">
</head>

<body>
  <div id="app"></div>
  <!-- docsify-edit-on-github -->
  <script src="//unpkg.com/docsify-edit-on-github/index.js"></script>
  <script>
    window.$docsify = {
      name: 'Java技术栈',
      repo: 'https://github.com/yuyuwork/docsify-blog',
      maxLevel: 5,//最大支持渲染的标题层级
      subMaxLevel: 3,
      homepage: 'README.md',
      coverpage: true, // 封面
      loadSidebar: true, // 侧边栏
      autoHeader: true,
      notFoundPage: true, // 找不到页面时
      auto2top: true,//切换页面后是否自动跳转到页面顶部
      //全文搜索
      search: {
        //maxAge: 86400000, // 过期时间，单位毫秒，默认一天
        paths: 'auto',
        placeholder: '搜索',
        noData: '未找到结果',
        // 搜索标题的最大程级, 1 - 6
        depth: 3,
      },
      plugins: [
        EditOnGithubPlugin.create('https://githee.com/test/')
      ],
    }
  </script>
  <script src="//unpkg.com/docsify/lib/docsify.min.js"></script>
  <!--Java代码高亮-->
  <script src="//unpkg.com/prismjs/components/prism-java.js"></script>
  <script src="//unpkg.com/prismjs/components/prism-python.js"></script>
  <!--全文搜索,直接用官方提供的无法生效-->
  <script src="https://cdn.bootcss.com/docsify/4.5.9/plugins/search.min.js"></script>
  <!-- 复制到剪贴板 -->
  <script src="//unpkg.com/docsify-copy-code"></script>
  <!-- emoji -->
  <script src="//unpkg.com/docsify/lib/plugins/emoji.js"></script>
  <!-- 图片缩放 -->
  <script src="//unpkg.com/docsify/lib/plugins/zoom-image.js"></script>
  <!-- 字数统计 -->
  <script src="//unpkg.com/docsify-count/dist/countable.js"></script>
</body>

</html>
```

### 项目部署 <!-- {docsify-ignore} -->

将我们搭建的 Blog 托管到 Gitee或者Github，在项目的 Settings 里开启 Pages 功能，便可以实时访问


!> 注意：Markdown文件不可为空，否则路由过去会出现404

