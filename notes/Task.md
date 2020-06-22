---
deleted: true
tags: [HappyWriting]
title: Task
created: '2020-06-12T22:53:42.296Z'
modified: '2020-06-18T08:36:25.952Z'
---

# Task
1. 登录模块
2. 获取任务列表，分为 submitted 和 reviewed
接口 getTaskList
请求参数：
{
  task_id: String
}
响应格式：
{
  status：200 || 404，
  data: {
    submitted: [
      {
        task_id: String,
        submitted_date: Date,
        user_name: String,
        course_name: String,
        chapter_index: Number,
        state: '未批改',
        recommend: Boolean
      }, {
        ...
      },
      ...
    ],
    reviewed: [
      {
        task_id: String,
        reviewed_date: Date,
        user_name: String,
        course_name: String,
        chapter_index: Number,
        state: '已批改',
        recommend: Boolean
      }, {
        ...
      },
      ...
    ]
  }
}
3. 获取点击对应的任务传递参数，跳转路由
接口 getTaskDetail
请求参数：
{
  task_id: String
}
响应格式：
{
  status: 200 | 404,
  data: {
    task_id: String,
    user_name: String,
    course_name: String,
    chapter_index: Number,
    state: String,
    homework_content: String,
    review_data: null,
    recommend: Boolean,
    homework_image: String
  }
}

5. 上墙
接口 recommendTask
请求参数 {
  task_id: String
}
响应格式：
{
  status: 200 | 404
}
6. 完成评教
接口 commitReview
请求参数 {
  task_id: String, 
  type: 'finish | save',
  data: Object
}
响应格式：
{
  status: 200 | 404
}
7. 下一个
8. 保存
接口 commitReview
请求参数 {
  task_id: String, 
  type: 'save',
  data: {}
}
响应格式：
{
  status: 200 | 404
}

