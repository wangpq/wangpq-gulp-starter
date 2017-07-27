# wangpq-gulp-study

> 一个简单的gulp示例，使用了gulp-connect-multi自动检测html、css、js的改动并自动刷新,并提供合并压缩CSS、JS个功能。以及自动在html中替换压缩过后的css和js，并可以给css、js添加版本号，避免上传服务器后处出现缓存。

怎样运行示例?

**首先确保全局安装了gulp**
```bash
npm install gulp -g
```

**下载依赖到本地**
```bash
npm install  
```

**在本地服务器运行**
```bash
gulp
```
然后打开浏览器，输入 http://localhost:8080 查看页面。

***

**Gulp自动添加版本号**

使用 gulp-rev + gulp-rev-collector 是比较方便的方法，结果如下:

```bash
"style.min.css" => "style-88795f4f15.min.css"
"app.min.js" => "app-5339a8b623.min.js",
"widget.min.js" => "widget-97d5000b1b.min.js"
```

用上面方法实现，每次更新都会积压过多过期无用的文件，我们预期效果是:

```bash
"style.min.css" => "style.min.css?v=88795f4f15"
"app.min.js" => "app.min.js?v=5339a8b623",
"widget.min.js" => "widget.min.js?v=97d5000b1b"
```

怎么办啊?改上面两个Gulp插件是最高效的方法了。

1. 安装gulp-rev、gulp-rev-collector

```bash
npm install --save-dev gulp-rev 
npm install --save-dev gulp-rev-collector
```

2. 打开 node_modules\gulp-rev\index.js,
    第134行 manifest[originalFile] = revisionedFile; 

    更新为: manifest[originalFile] = originalFile + '?v=' + file.revHash;


3. 打开 nodemodules\gulp-rev\nodemodules\rev-path\index.js,
    10行 return filename + '-' + hash + ext; 

    更新为: return filename + ext;


4. 打开 node_modules\gulp-rev-collector\index.js,
    40行  path.basename(json[key]).replace(new RegExp( opts.revSuffix ), '' ) 

    更新为: path.basename(json[key]).split('?')[0] 


5. 配置gulpfile.js, 可参考下面 gulpfile.js 代码


6. 结果达到预期


***
**批处理文件(脚本)快速执行命令**
   
> 在和gulpfile.js文件同级的目录下，我创建了好几个不同名的批处理脚本，以便快速执行我们需要的命令，执行它，只需要我们在下载下来的项目中，在所有的依赖模块都成功安装的情况下，双击它即可。

1. dev.bat

    主要用于开发调试阶段，使用gulp-connect-multi启动本地服务器,自动检测html、css、js的改动并自动刷新

2. uat.bat

3. live.bat
   
    用于生产项目，可压缩合并CSS、JS,并在html中替换合并后的css和js。(前提是在gulpfile.js和html文件中配置正确啦！)

4. gulpRevCssJs.bat

    用户生产的CSS、JS文件再次发到生产环境，可能会因为缓存的原因不能正确运行，我们可以在引入的css和js文件上添加版本号
    双击gulpRevCssJs.bat文件就会产生这样的版本号。

5. gulpRevHtml.bat
    
    将带版本号的css和js文件自动替换到html文件中。执行gulpRevHtml.bat前，请先确保执行过live.bat和gulpRevCssJs.bat
