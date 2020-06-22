---
tags: [HappyWriting]
title: Code Review 问题梳理
created: '2020-06-20T01:24:03.491Z'
modified: '2020-06-20T03:06:03.453Z'
---

# Code Review 问题梳理
### getTaskList 接口的优化
* 通过参数的形式调用接口，参数为 submitted || reviewed || [submitted, reviewed]
* 添加查询参数
* 查询课程与用户的操作分离
* try catch 的改进
### getTaskDetail 接口的优化
* try catch 的改进
* 添加查询参数
### recommendTask 接口的优化
* try catch 的改进
* bug 的修复
### commitReview 接口的优化
* 在服务端获取图片以及音频的临时链接
* try catch 的改进
### admin-login 接口的优化
* then 与 await 混用的问题
### 登录页面
* 
### 后台分页
*
### 后台查询下一个 task
*
### 任务列表页面优化
* mounted 时请求 getTaskList('submited') 接口
* tab 按钮封装为函数，分别调用 getTaskList 接口
* 样式外联
### 评教页面优化
* await 与 then 混用问题
* 获取临时链接的操作写入后台
* 数据结构的优化，所有操作为一个数组，数组内有不同对象，操作对象通过 type 进行区分各个操作
* 数据结构的优化，所有步骤为一个数组，数组内有不同对象，步骤对象通过 name 进行区分各个步骤
* 撤回功能直接通过 pop 操作数组，并绘制先前数据
* initImage 的优化，函数功能不清晰
* initCanvas 的优化，this[ref] 的格式不合适
* canvasMouseUP 的优化，按照新的数据结构，在 drawState === true 的情况下直接 push 不同类型的操作对象
* drawLine, drawCircle, drawRectangle, drawTex, blobToFile 工具函数的分离
* 播放函数的优化，在注册 audio ended事件中先remove ended事件
* 优化绘制 stroke 的播放， setTimeout 回调 setTimeout 
* 优化 countReward 函数，遍历新的步骤数组数据结构，进行遍历计算 count
* setFileIDInRecordData, setFileIDInRectangleRecordData 的优化，遍历操作

