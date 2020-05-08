---
tags: [Mini Program]
title: Mini Program Tricks
created: '2020-05-06T08:22:55.959Z'
modified: '2020-05-07T02:20:23.612Z'
---

# Mini Program Tricks

### 获取用户信息

```HTML
    <button 
      open-type="getUserInfo" 
      bindgetuserinfo="onGetUserInfo"
      class="userinfo-avatar"
      style="background-image: url({{avatarUrl}})"
      size="default"
    ></button>
```
```JavaScript
    wx.getSetting({
      success: res => {
        if (res.authSetting['scope.userInfo']) {
          // 已经授权，可以直接调用 getUserInfo 获取头像昵称，不会弹框
          wx.getUserInfo({
            success: res => {
              this.setData({
                avatarUrl: res.userInfo.avatarUrl,
                userInfo: res.userInfo
              })
            }
          })
        }
      }
    })
```

[获取用户信息方案介绍](https://developers.weixin.qq.com/community/develop/doc/c45683ebfa39ce8fe71def0631fad26b)
