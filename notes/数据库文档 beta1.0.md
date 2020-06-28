---
tags: [HappyWriting]
title: 数据库文档 beta1.0
created: '2020-06-18T07:36:32.337Z'
modified: '2020-06-28T14:00:58.082Z'
---

# 数据库文档 beta1.0

## task | 任务表
### 字段
| 字段名 | 类型 | 描述 |
|  ----  | ----  | ---- |
| **_id** | String | 任务 ID |
| *course_id*  | String | 课程 ID |
| *user_id* | String | 用户 ID |
| *operator_id* | String | 操作账户 ID |
| chapter_index | Number | 章节索引 |
| review_data | Object | 点评数据 |
| state | String | 任务状态，分为 submitted 和 reviewed 两种 |
| viewed | Boolean | 用户查看状态 |
| recommend | Booolean | 是否推荐 |
| liked_sum | Number | 点赞数量 |
| homework_image | String | 作业图片 URL |
| submit_date | Date | 用户提交作业时间 |
| review_date | Date | 教师点评作业时间 |
| recommend_date | Date | 推荐时间 |
* 字段 review_data 对象说明
  * 以课程 ID 为键名
  * 课程键值格式
```JavaScript
{
}
```
## lecturer | 讲师表
### 字段
| 字段名 | 类型 | 描述 |
|  ----  | ----  | ---- |
| **_id** | String | 讲师 ID |
| nickname  | String | 讲师昵称 |
| avatar  | String | 讲师头像 |
| create_date | Date | 创建时间 |

## course | 课程表
### 字段
| 字段名 | 类型 | 描述 |
|  ----  | ----  | ---- |
| **_id** | String | 课程 ID |
| *lecturer_id*  | String | 讲师 ID |
| *assistant_id* | String | 助教 ID |
| poster_image | String | 海报图片 |
| previous_price | Number | 课程原价 |
| current_price | Number | 课程价格 |
| title | String | 课程标题 |
| subtitle | String | 课程副标题 |
| recommend | Boolean | 课程推荐 |
| duration | Number | 课程时间，单位为天 |
| cover_image | String | 封面图片 |
| chapter | Array | 课程章节 |
| create_date | Date | 创建时间 |
* 字段 chapter 数组说明
  * 以课程 ID 为键名
  * 课程键值格式
```JavaScript
[
    {
        content: String,  // 作业内容
        subtitle: String, // 小节副标题
        video: [          // 小节视频数组
            {
                cover_image: String,  // 视频封面图片
                duration: Number,     // 视频时长
                url: String,          // 视频 URL
                titile: String        // 视频标题
            },
            ...
        ]
    },
    ...
]
```
## user | 用户表
### 字段
| 字段名 | 类型 | 描述 |
|  ----  | ----  | ---- |
| **_id** | String | 用户 ID |
| *_openid*  | String | Open ID |
| phone_number | String | 用户手机号 |
| information | Object | 用户微信信息 |
| nickname | Strign | 用户昵称 |
| avatar  | String | 用户头像 |
| create_date | Date | 创建时间 |

## operator | 操作账户表
### 字段
| 字段名 | 类型 | 描述 |
|  ----  | ----  | ---- |
| **_id** | String | 账户 ID |
| operator_id  | String | 登录 ID |
| operator_pwd | String | 登录密码 |
| create_date | Date | 创建时间 |


