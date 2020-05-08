---
tags: [Git]
title: 规范化团队 git 提交信息
created: '2020-05-06T06:22:00.990Z'
modified: '2020-05-06T06:24:56.639Z'
---

# 规范化团队 git 提交信息

> 同一个工程项目，为了方便管理，git 的 commit 信息最好按照一定的格式规范，以便在需要的时候方便使用。什么是方便的时候，比如出现了一个线上 bug，所以需要回滚操作，知道了提交信息可以方便的定位问题。代码 review 的时候也知道了该次 commit 干了什么，所以 commit 标准化好处很多，不再举例。

## 实现

可以马上想到的是利用 shell 结合 git hook 实现在 git commit 阶段检查输入是否符合规范。符合就通过，不符合就终止，并给出提示信息。

### 规范是什么

常见的分类有下面几种：

- build：修改项目的的构建系统（xcodebuild、webpack、glup等）的提交
- ci：修改项目的持续集成流程（Kenkins、Travis等）的提交
- chore：构建过程或辅助工具的变化
- docs：文档提交（documents）
- feat：新增功能（feature）
- fix：修复 bug
- pref：性能、体验相关的提交
- refactor：代码重构
- revert：回滚某个更早的提交
- style：不影响程序逻辑的代码修改、主要是样式方面的优化、修改
- test：测试相关的开发

### 轮子

在 github 上有 [commitlint](https://github.com/conventional-changelog/commitlint) 这个项目，它可以很方便的在工程中做配置，并允许你自定义上面说的「规范」、「分类」。

[commitlint](https://github.com/conventional-changelog/commitlint)：用于检查提交信息 [husky](https://github.com/typicode/husky)：hook 工具，用于 git-commit 和 git-push 阶段。

怎么用？

1. 初始化一个 node 项目：`npm init -y`

2. 安装所需依赖。`npm install --save-dev @commitlint/config-conventional @commitlint/cli husky`

3. 在工程根目录下新建配置文件，名称为 `commitlint.config.js`。

4. 在 commitlint.config.js 中添加配置信息

   ```
   const types = [
     'build', 
     'ci', 
     'chore',
     'docs', 
     'feat', 
     'fix', 
     'pref', 
     'refactor', 
     'revert', 
     'style', 
     'test'
   ];
   
   typeEnum = {
     rules: {
       'type-enum': [2, 'always', types]
     },
     value: () => types
   }
   
   module.exports = {
       extends: [
         "@commitlint/config-conventional"
       ],
       rules: {
         'type-case': [0],
         'type-empty': [0],
         'scope-empty': [0],
         'scope-case': [0],
         'subject-full-stop': [0, 'never'],
         'subject-case': [0, 'never'],
         'header-max-length': [0, 'always', 72],
         'type-enum': typeEnum.rules['type-enum']
       }
     };
   复制代码
   ```

5. 在 package.json 文件中添加以下代码，代码层级跟

    

   devDependencies

    

   同级。

   ```
   "husky": {
       "hooks": {
           "pre-commit": "echo '哈喽，小伙伴们，在这里可以做测试相关的逻辑哦，一般结合公司的 ci'",
           "commit-msg": "commitlint -E HUSKY_GIT_PARAMS",
           "pre-push": "echo 提交代码前需要先进行单元测试 && 可以做测试相关"
       }
   }
   复制代码
   ```

上面的流程配置完成，当你在提交 commit 信息的输入的内容，如果不符合 `: ` 规则，会终止并给出提示信息。

type 就是上面的种类；subject 就是需要提交的文字概括。比如：feature：增加摇一摇推荐酒店功能。

小说明：如果某次提交想禁用 husky，可以添加参数 **--no-verify**。`git commit --no-verify -m "xxx"`

贴个效果图



![commitlint](https://user-gold-cdn.xitu.io/2020/3/3/1709ee16b3fc77ac?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



### 流程说明

安装包 husky 的时候，会在目录 `.git/hooks/` 下生成一堆 shell 脚本，负责 git 的 hook。

`"commit-msg": "commitlint -E HUSKY_GIT_PARAMS"` 这个配置告诉 git hooks，当执行 `git commit -m` 的时候触发 commit-msg 钩子，并通知 husky，从而执行 `commitlint -E HUSKY_GIT_PARAMS`，实际上执行的是 `./node_modules/husky/bin/run.js`，读取 commitlint.config.js 里的配置，然后对我们 commit -m 里的字符串校验，如不通过则输出错误信息并终止。

## 拓展篇

git commit 的几个钩子，也暴露出来了，所以可以结合时机做一些额外的逻辑。

- pre-commit：在 git commit 之前触发
- commit-msg：在编写 commit 信息的时候触发
- pre-push：在 git push 之前触发

所以基于上述时机，可以根据项目特点做一些别的事情。比如 code lint、unit test 代码覆盖率检测、changelog 自动生成、unit test 脚本等、也可以借此机会产生 lint 报表
