---
title: react基于webpack和Babel 6上的开发环境搭建
layout: post
tags:
  - F2e
  - Web
  - React
---

<!--more-->

## 开始一个项目

{% highlight sh%}
npm init
{% endhighlight %}

### 安装webpack

{% highlight sh%}
npm install webpack --save-dev
{% endhighlight %}

### 项目目录(demo架构)

{% highlight py%}
    - /app
        - main.js
        - component.js
    - /build
        - bundle.js （自动生成）
        - index.html
    - webpack.config.js
    - package.json
{% endhighlight %}

### 设置webpack.config.js

{% highlight  js%}
var path = require('path');
module.exports = {
    entry:path.resolve(__dirname,'app/main.js'),
    output:{
        path:path.resolve(__dirname,'build'),
        filename:'bundle.js'
    }
};
{% endhighlight %}

在package.json里面预设这个命令去启动打包功能

{% highlight sh%}
"build":"webpack"
{% endhighlight %}

### 课间练习

{% highlight  js%}
//component.js
'use strict'
module.exports = function(){
    var a = 'hello word'
    return a;
}
//main.js
'use strict'
var component = require('./component.js');
document.body.innnerHTML = component();
{% endhighlight %}

使用webpack-dev-server：监听代码自动刷新！
{% highlight  sh%}
npm install webpack-dev-server --save-dev
{% endhighlight %}
在package.json里面配置webpack-dev-server

{% highlight  sh%}
"dev":"webpack-dev-server --devtool eval --progress --colors --hot --content-base build"
//webpack-dev-server 创建一个服务器8080端口的
//devtool eval --为你的代码创建源地址，可以代码快速定位
//progress --显示进度条
//colors --命令行带颜色
//content-base build --指向设置的输出目录
//一旦启动这个就会用服务器去监听代码文件的变化，从而每次变化都自动合并
{% endhighlight %}

### 启动自动刷新功能

//配置在webpack.config.js的入口

{% highlight  js%}
entry:[
    'webpack/hot/dev-server',
    'webpack-dev-server/client?http://localhost:8080',
    path.resolve(__dirname,'app/main.js');
]
{% endhighlight %}

### 课堂练习

{% highlight sh%}
npm run dev 启动服务器
{% endhighlight %}

打开浏览器－>http://localhost:8080
修改一下前面的课堂练习时候写的代码中的compnent.js的return的东西
配置react / babel

{% highlight sh%}
//安装react
npm install react --save
npm install react-dom --save
//安装babel-loader
npm install babel-loader --save-dev
npm install babel-core --save-dev
npm install babel-preset-es2015 --save-dev  //支持ES2015
npm install babel-preset-react --save-dev //支持jsx
npm install babel-preset-stage-0 --save-dev //支持ES7
{% endhighlight %}

//但是还不够

{% highlight sh%}
endhighlight
npm install babel-polyfill --save
/*
Polyfilla是一个英国产品,在美国称之为Spackling Paste(译者注:刮墙的,在中国称为腻子).记住这一点就行:把旧的浏览器想象成为一面有了裂缝的墙.这些[polyfills]会帮助我们把这面墙的裂缝抹平,还我们一个更好的光滑的墙壁(浏览器)
*/
npm install babel-runtime  --save
npm install babel-plugin-transform-runtime --save-dev
{% endhighlight %}

/*减少打包的时候重复代码，以上要注意是放在dev还是非dev上！*/

### 配置babel

{% highlight  js%}
//在入口添加polyfill
entry[
    'babel-polyfill',
    'webpack/hot/dev-server',
    'webpack-dev-server/client?http://localhost:8080',
    path.resolve(__dirname,'app/main.js')
]
//在webpack.config.js的module.exports = {}里面增加
module:{
    loaders:[{
        'loader':'babel-loader',
        exclude:[
            //在node_modules的文件不被babel理会
            path.resolve(__dirname,'node_modules'),
        ]，
        include:[
            //指定app这个文件里面的采用babel
            path.resolve(__dirname,'app'),
        ],
        test：/\.jsx?$/,
        query:{
            plugins:['transform-runtime'],
            presets:['es2015','stage-0','react']
        }
    }]
}
{% endhighlight %}

### 课堂测试

{% highlight js%}
//component.js
'use strict'
import React from 'react'
class Component extends React.Component{
    render(){
        return <div>Helllo World</div>
    }
}
//main.js
'use strict'
//注意！这里引入了新的东西
import 'babel-polyfill'
import React from 'react'
import {render} from 'react-dom'
import Component from './component'
let main = function(){
    render(<Component />,document.getElementById('main'));
}
main();
{% endhighlight %}

### 加入CSS ／ iamge / font
// 这里先留个坑！因为暂时来说还是认为外链出来适合现在这个时代。可能在项目正式上线的时候才需要
发布配置：单文件入口版本！
//新建一个webpack.production.config.js
//in package.json
{% highlight  sh%}
"deploy": "NODE_ENV=production webpack -p --config webpack.production.config.js"
{% endhighlight %}
//in webpack.production.config.js
//和开发环境不同的是，入口和出口。相应的在HTML的JS源也要进行修改。
{% highlight js%}
var path = require('path')
var node_module_dir = path.resolve(__dirname,'node_module');
module.exports = {
    entry:[
        'babel-polyfill',
        path.resolve(__dirname,'app/main.js'),
    ],
    output:{
        path:path.resolve(__dirname,'build'),
        filename:'app.js'
    },
    module:{
        loaders:[
            {
                loader:"babel-loader",   //加载babel模块
                include:[
                    path.resolve(__dirname,'app'),
                ],
                exclude:[
                    node_module_dir
                ],
                test:/\.jsx?$/,
                query:{
                    plugins:['transform-runtime'],
                    presets:['es2015','stage-0','react']
                }
            },
        ]
    }
}
{% endhighlight %}

### 发布配置（多文件模式）
库最好就不要打包进来。因为一般库都是不会改动的。所有用户load一次就好了。所以这里就要进行库的分离。
依旧是：webpack.production.config.js

{% highlight js%}
var path = require('path');
var webpack = require('webpack');
var node_module_dir = path.resolve(__dirname,'node_module');

module.exports = {
    entry:{
        app:[path.resolve(__dirname,'app/main.js'),],
        react: ['babel-polyfill','react','react-dom']
    },
    output:{
        path:path.resolve(__dirname,'build'),
        filename:'app.js'
    },
    module:{
        loaders:[
            {
                loader:"babel-loader",   //加载babel模块
                include:[
                    path.resolve(__dirname,'app'),
                ],
                exclude:[
                    node_module_dir
                ],
                test:/\.jsx?$/,
                query:{
                    plugins:['transform-runtime'],
                    presets:['es2015','stage-0','react']
                }
            },
        ]
    },
    plugins: [
        new webpack.optimize.CommonsChunkPlugin('react', 'react.js')
      ]
}
{% endhighlight %}