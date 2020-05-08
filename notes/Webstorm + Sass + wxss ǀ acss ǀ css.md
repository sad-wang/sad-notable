---
tags: [Sass]
title: Webstorm + Sass + wxss | acss | css
created: '2020-05-07T04:05:03.602Z'
modified: '2020-05-07T04:07:53.402Z'
---

# Webstorm + Sass + wxss | acss | css

微信小程序的wxss、阿里旗下淘宝、支付宝小程序的acss等等语法很类似原生css，但是在web开发里用惯了动态css语言，再写回原生css很不习惯，尤其是父子样式的嵌套写法非常繁琐。

因此，我希望能有一个自动化构建方案，能够简单地将scss转换成小程序的样式语言。

方案1
以前写微信小程序的依赖库时用过，使用gulp编译，将源码和编译后的代码分别放到src和dist两个目录。gulp会处理src下面的所有文件，将其中的scss转换成css，并将其他所有文件原封不动挪到dist下相应位置。

这里就不详细说了，代码参考Wux。

方案2
非常简单直接，使用Webstorm/IDEA的File Watchers功能实时转换。

安装Ruby和sass
确保命令行输入sass -v能出现版本号，安装过程略。

安装File Watchers
到插件市场上搜索并安装（已安装则跳过）


添加scss的转换脚本
现在安装完插件打开项目会自动弹出scss转css的向导，方便了很多。但还需要做一些修改，配置如下：


首先要将生成文件的后缀名改掉，比如这里我的淘宝小程序就得是acss。

其次，将Arguments改为:

```
$FileName$:$FileNameWithoutExtension$.acss --no-cache --sourcemap=none --default-encoding utf-8 --style expanded
```
如果不加--no-cache，scss文件同目录下会出现一个.sass-cache目录。

如果不加--sourcemap=none, scss文件同目录下会出现一个.map文件。

如果不加--default-encoding utf-8, scss文件如果有中文注释转换就会报错。

style可不加，这里用的是无缩进和压缩的风格，反正小程序打包发布时还会压，这里保持可读性。

现在这个scss转换是单独作用于项目的，如果新建一个小程序项目，就需要重新添加（不建议设置成global，容易误伤）。

注意到File Watchers列表的右侧操作栏下方有导入导出按钮，可以将现在配好的设置导出保存，将来新建项目时只要导入一下就行了。
之后还有一个问题，如果我手动将编译后的css（即wxss或者acss，下略）文件删除，scss文件不改动的话，就不会重新编译出css文件。
或者万一监听失效或者不够及时，css还有可能是旧的。
所以还需要一个命令，用来将整个目录下的scss文件统一转换，确保没有遗漏和保持代码最新。

不过我看了半天sass和sass-convert的文档，没有找到一个可用的写法，能让命令行遍历指定目录下的所有scss文件，将其转换成css放到源文件所在目录，并且将后缀名改为wxss或者acss。

所以遍历这个行为只能交给nodejs来实现，代码如下：

创建编译脚本build/scss-convert.js：
```
var path = require("path")
var fs = require("fs")
const { exec } = require('child_process')

const basePath = path.resolve(__dirname, '../')

function mapDir(dir, callback, finish) {
  fs.readdir(dir, function(err, files) {
    if (err) {
      console.error(err)
      return
    }
    files.forEach((filename, index) => {
      let pathname = path.join(dir, filename)
      fs.stat(pathname, (err, stats) => { // 读取文件信息
        if (err) {
          console.log('获取文件stats失败')
          return
        }
        if (stats.isDirectory()) {
          mapDir(pathname, callback, finish)
        } else if (stats.isFile()) {
          if (!['.scss'].includes(path.extname(pathname))) {
            return
          }
          callback(pathname)
        }
      })
      if (index === files.length - 1) {
        finish && finish()
      }
    })
  })
}

mapDir(
  basePath,
  function (file) {
    const newFileWithoutExt = path.basename(file, '.scss')
    if (newFileWithoutExt.startsWith('_')) {
      return  // 按照scss规则，下划线开头的文件不会生成css
    }
    // exec可以让nodejs执行外部命令
    exec(`sass --no-cache --sourcemap=none --default-encoding utf-8 --style expanded ${file}:${newFileWithoutExt}.acss`, {
      cwd: path.dirname(file) // 不写这个会导致生成的文件出现在根目录
    }, (err, stdout, stderr) => {
      if (err) {
        console.log(err)
        return
      }
      console.log(`stdout: ${stdout}`)
    })
  },
  function() {
    // console.log('xxx文件目录遍历完了')
  }
)
```
在package.json里添加一条script:
```
"scripts": {
  "scss": "node build/scss-convert",
},
```
