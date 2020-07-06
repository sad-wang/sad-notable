---
title: 数据库文档 beta2.0
created: '2020-07-06T12:14:47.811Z'
modified: '2020-07-06T13:58:00.651Z'
---

# 数据库文档 beta2.0
## user | 用户表
| 字段名 | 类型 | 描述 |
|  ----  | ----  | ---- |
| **_id** | String | 用户 ID |
| _openid  | String | 唯一用户标识 |
| information  | Object | 用户信息 |
| user_nickname | String | 用户昵称 |
| user_telephone | String | 用户手机号 |
| _school_id_ | String | 学校 ID |
| _grade_id_ | String | 年级 ID |
| _class_id_ | String | 班级 ID |
| create_date | Date | 用户创建时间 |

## school | 学校表
| 字段名 | 类型 | 描述 |
|  ----  | ----  | ---- |
| **_id** | String | 学校 ID |
| school_name | String | 学校名称 |

## grade | 年级表
| 字段名 | 类型 | 描述 |
|  ----  | ----  | ---- |
| **_id** | String | 年级 ID |
| _school_id_ | String | 学校 ID |
| grade_name | String | 年级名称 |

## class | 班级表
| 字段名 | 类型 | 描述 |
|  ----  | ----  | ---- |
| **_id** | String | 年级 ID |
| _school_id | String | 学校 ID |
| _grade_id_ | String | 年级 ID |
| class_name | String | 班级名称 |
| _teacher_id_ | String | 教师 ID |

## teacher | 教师表
| 字段名 | 类型 | 描述 |
|  ----  | ----  | ---- |
| **_id** | String | 教师 ID |
| teacher_name | String | 教师姓名 |

## task | 作业表
| 字段名 | 类型 | 描述 |
|  ----  | ----  | ---- |
| **_id** | String | 作业 ID |
| _class_id_ | String | 班级 ID |
| _operator_id_ | String | 操作账户 ID |
| task_content | String | 作业内容 |
| task_type | String | 作业类型，unrevised|revised |
| task_title | String | 作业标题 |
| task_image | [String] | 作业图片 FileID 数组 |
| task_deadline | Date | 作业截至时间 |

## works | 作品表
| 字段名 | 类型 | 描述 |
|  ----  | ----  | ---- |
| **_id** | String | 作品 ID |
| _task_id_ | String | 任务 ID |
| _user_id_ | String | 用户 ID |
| task_works | String | 用户作品 FileID |
| correct_data | Object | 作业批改数据 |
| task_state | Stirng | 作业状态，submitted|revised |
| submittied_date | Date | 提交时间 |
| revised_date | Date | 点评时间 |
| viewed | Boolean | 是否查看点评 |

```JavaScript
correct_data {
  score: Number // 得分，0 - 100 分
  image_url: Stirng // 作业批改图片
  step: { // 录制步骤
    step1Record: {  // 步骤一录制数据
      name: 'step1Record',
      type: 'record',
      recordData: { //录制数据
        duration: Number // 录制时长，单位毫秒
        recordFile： String // 录音文件 FileID
      },
      drawData: [ // 绘制数据
        [ // 一笔
          { // 点
            color: String // 颜色
            position: {
              x: Number, // x坐标
              y: Number // y坐标
            },
            time: Number // 时间，单位毫秒
          },
          ...
        ],
        ...
      ],
      rewardData: [ // 奖励数据
        {
          time: Number // 时间，单位毫秒
          id： String // 奖励 ID
        }
      ]
    },
    step2Record: { // 步骤二录制数据
      name: 'step2Record',
      type: 'record',
      recordData: { //录制数据
        duration: Number // 录制时长，单位毫秒
        recordFile： String // 录音文件 FileID
      },
      drawData: [ // 绘制数据
        [ // 一笔
          { // 点
            color: String // 颜色
            position: {
              x: Number, // x坐标
              y: Number // y坐标
            },
            time: Number // 时间，单位毫秒
          },
          ...
        ],
        ...
      ],
      rewardData: [ // 奖励数据
        {
          time: Number // 时间，单位毫秒
          id： String // 奖励 ID
        }
      ]
    },
    step2Select: { // 步骤二选取数据
      name: 'step3Select',
      type: 'select',
      rectangleData: [ // 选取矩阵数据
        {
          name: 'step3SelectRecord${index}',
          type: 'select', 
          url: String, // FileID
          start： { // 起点坐标
            x: Number, // x坐标
            y: Number // y坐标
          },
          end { // 终点坐标
            x: Number, // x坐标
            y: Number // y坐标
          }
        }
      ]
    },
    step3Record: { // 步骤三录制数据
      name: 'step2Record',
      type: 'record',
      recordData: { //录制数据
        duration: Number // 录制时长，单位毫秒
        recordFile： String // 录音文件 FileID
      },
      drawData: [ // 绘制数据
        [ // 一笔
          { // 点
            color: String // 颜色
            position: {
              x: Number, // x坐标
              y: Number // y坐标
            },
            time: Number // 时间，单位毫秒
          },
          ...
        ],
        ...
      ],
      rewardData: [ // 奖励数据
        {
          time: Number // 时间，单位毫秒
          id： String // 奖励 ID
        }
      ]
    },
    step3Select: { // 步骤三选取数据
      name: 'step3Select',
      type: 'select',
      rectangleData: [ // 选取矩阵数据
        {
          name: 'step3SelectRecord${index}',
          type: 'select', 
          url: String, // FileID
          start： { // 起点坐标
            x: Number, // x坐标
            y: Number // y坐标
          },
          end { // 终点坐标
            x: Number, // x坐标
            y: Number // y坐标
          },
          recordData: { // 录制数据
            duration: Number // 录制时长，单位毫秒
            recordFile： String // 录音文件 FileID
          },
          drawData: [ // 绘制数据
            [ // 一笔
              { // 点
                color: String // 颜色
                position: {
                  x: Number, // x坐标
                  y: Number // y坐标
                },
                time: Number // 时间，单位毫秒
              },
              ...
            ],
            ...
          ],
          rewardData: [ // 奖励数据
            {
              time: Number // 时间，单位毫秒
              id： String // 奖励 ID
            },
            ...
          ]
        },
        ...
      ]
    }
  }
}
```















