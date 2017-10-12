
## 如何开始
clone到本地
```
> git clone https://github.com/xiaolin00/grallery-by-react.git
```
安装依赖
```
> npm install
```

运行开发环境项目
```
> npm start
```
打包输出到dist目录
```
> npm run dist
```
更多命令请参考 **package.json** 文件, 从头构建项目可以参考视频教程

## 目录说明
```shell
.
├── /cfg/                       # webpack配置文件存放目录
│   ├── base.js                 # 基础配置
│   ├── default.js              # 默认配置
│   ├── dev.js                  # 开发环境配置
|   ├── dist.js                 # 生成环境配置
│   └── test.js                 # 测试环境配置
├── /dist/                      # 存放最终打包输出的用于生产环境的项目文件
├── /node_modules/              # node模块存放的目录
├── /src/                       # 存放开发环境项目源码
│   ├── /actions/               # flux actions目录（没用到）
│   ├── /components/            # 组件目录
│   ├── /config/                # 配置目录（没用到）
│   ├── /sources/               # flux datasources目录（没用到）
│   ├── /stores/                # flux stores(没用到)
│   ├── /styles/                # 样式文件目录，内有一个App.css基础css文件
│   ├── index.html              # 项目入口文件
│   └── index.js                # js入口文件
├── /test/                      # 单元测试和集成测试目录
│── .babelrc                    # Babel 配置文件
│── .eslintrc                   # ESLint代码风格检测配置文件
│── .gitignore                  # 配置不需要加入Git版本管理的文件
│── .yo-rc.json                 # yeoman的配置文件
│── karma.conf.js               # karma测试框架的配置
│── LICENSE                     # 软件使用许可
│── package.json                # npm的依赖配置项
│── README.md                   # 项目说明文件
│── server.js                   # 项目运行的js文件，命令可查看package.json中的script
└── webpack.config.js           # webpack配置文件，不同环境的配置项在cfg目录下
```

## 部分扇区示意图
```
                          |------上扇区线-----|
                          |         |         |
         |—————舞台线—————|——-——————|—————————|————————————————————|
         |                |<--------|-------->|                    |
         |                |         |         |                    |
         |                |         |         |                    |
         |----------------|----  ___|____ ----|--------------------|
         |                      |        |                         |
         |                      | 中心图片|                         |
         |                      |________|                         |
         |                          |                              |
         |                          |                              |
         |                          |                              |
         |                          |                              |
         |——————————————————————————|—————舞台线———————————————————|
                                    |
                                   中轴线
```

## ES5和ES6写法的不同点
创建组件
```shell
# ES5
Var AppComponent = React.createClass;

# ES6
class AppComponent extends React.Component{ }
```
定义组件初始化状态
```shell
# ES5
getInitialState: function(){
  return {
    name: 'ck'
  };
}

# ES6
constructor(props) {
  this.state = {
    name: 'ck'
  }
}
```
箭头函数
```shell
# ES5
Var getRangeRandom = function(low, high) {
  return Math.floor(Math.random() * (high - low) + low);
}

# ES6
let getRangeRandom = (low, high) =>  Math.floor(Math.random() * (high - low) + low );
```


## Git命令说明
```shell
# 查看更改的文件
git satus

# 添加所有更改的文件进入本地暂存区
git add -A

# 提交暂存区到仓库区
git commit -m " message "

# 将本地仓库更新到远程仓库
git push

# 强制删除本地分支
git branch -D gh-pages

# 使用git的subtree将已有项目的某个目录分离成独立项目
# 并推送到分支 gh-pages
# prefix指定本地推送的目录
git subtree push --prefix=dist origin gh-pages
```

#### 继续爬坑

1.成功发布到gh-pages,打开gh-pages的预览地址，发现app.js文件找不到404了。
    解决方法：
        - 修改default.js中的`publicPath:'/assets'` 为：`publicPath:'galler-by-react/assets'.            

        - 修改index.html中的
        ```
           `<script type="text/javascript" src="/assets/app.js"></script>`
            为：`<script type="text/javascript" src="assets/app.js"></script>` 
        ```
        - 再重新运行上面列出的git步骤命令即可。

2.app.js找到之后，却又发现图片都找不到404了。我们在package.json文件中的scripts可以看到：

```
 "serve:dist": "node server.js --env=dist",
  "dist": "npm run copy & webpack --env=dist",
  "copy": "copyfiles -f ./src/index.html ./src/favicon.ico ./src/images ./dist",
```


 而把项目发布到githbu-pages上的内容实际是dist文件夹下的内容，而我们再回看package.json文件中的"dist":"npm run copy"，因此我们再看看"copy"脚本命令中copy的内容只有./src/index.html ./src/favicon.ico，可是我们还需要展示图片。因此问题就是在运行`npm run dist`时没有把images目录复制到dist目录中。

解决方法：
    - 直接手动操作复制images粘贴到dist目录下
    - 也可以修改"copy"命令为:"copy": "copyfiles -f ./src/index.html ./src/favicon.ico ./src/images ./dist"，即添加./src/images。后再运行一遍上述列出的git步骤命令即可。


## 收获以及感想
1. 在macOS的浏览器上使用灰阶渲染字体，修复字体过粗问题：
    - 灰阶渲染是通过控制字体轮廓上像素点的亮度，达到字体原始形状的方法
    - 亚像素渲染则利用了LCD屏幕中每个像素是由RGB三个亚像素的颜色和亮度混合而成一个完整像素的颜色这一原理，将字体上的轮廓点由三个亚像素体现达到原始形状的方法，与灰阶渲染相比，分辨率在垂直方向上放大了三倍，因此，渲染效果更好。但是，所消耗的内存也更多。

因此在手机屏幕上，为了减少CPU的开销，使用灰阶渲染。但是在macOS操作系统上，采用的是亚像素渲染这种方式。

这会导致白色、亮色的字体，在深色背景下会显得过粗，严重情况下看上去会模糊。 
但是我们可以通过修改浏览器上的属性，告诉浏览器怎么来渲染字体。

```
-webkit-font-smoothing: antialiased; //开启chrome在macOS上的灰阶平滑
-moz-osx-font-smoothing: grayscale; //开启firefox在macOS上的灰阶平滑
```

2. CSS3属性 perspective ，设置元素被查看位置的视图。但是目前浏览器都不支持 perspective 属性。只有Chrome 和 Safari 支持替代的 -webkit-perspective 属性。

3. Safari浏览器transform兼容性问题。`transform: rotateY(180deg) translateZ(1px);` 在transform中使块在Z轴上2D旋转1px，使块突出来一点点，才可以正常覆盖另一面。
4. 进一步了解了React以及终于开始了React实战动手做了第一个小demo
