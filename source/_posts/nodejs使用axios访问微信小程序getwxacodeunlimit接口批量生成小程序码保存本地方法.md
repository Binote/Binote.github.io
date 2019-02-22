---
title: "nodejs使用axios访问微信小程序getwxacodeunlimit接口批量生成小程序码保存本地方法"
date: 2019-02-20 15:13:06
tags:
  - nodejs
  - axios
  - 微信小程序
  - 批量生成小程序码
categories:
  - nodejs
---

![微信小程序](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1550741813393&di=75757328839b4e8d128bfa2385ca9dc8&imgtype=0&src=http%3A%2F%2Fwww.jetsum.net%2Fu%2Fcms%2Fwhjxxxjsyxgs%2F201812%2F06134442avzc.png)

nodejs 使用 axios 访问微信小程序 getwxacodeunlimit 接口
批量生成小程序码保存本地方法

## axios 方法

使用 axios 访问接口，设置 responseType: 'arraybuffer'

<!-- more -->

```javascript
  /**
   * 获取小程序qrcode(不能设置 encoding: null,输出图片不能打开)
   * 设置responseType: 'arraybuffer'后成功输出图片
   * @param {String} page
   * @param {String} scene
   * @returns
   * @memberof GetQrCodeObj
   */
  getQrCode(page,scene,){
    return new Promise((resolve,reject)=>{
      axios.post('https://api.weixin.qq.com/wxa/getwxacodeunlimit?access_token='+this.access_token,{
        scene:scene,
        page,
        is_hyaline:true
      },{
        responseType: 'arraybuffer'  // 关键在于设置responseType为'arraybuffer'
      }).then(res=>{
        let data = res.data
        if(data.errcode){
          reject(new Error('生成二维码失败'))
          return
        }
        dataFn.writeFileData(path.join(__dirname,'./dist/'+scene+'.png'),data).then(res=>{
          resolve(res)
        }).catch(err=>{
          reject(err)
        })
      }).catch(err=>{
        reject(err)
      })
    })
  }
```

dataFn.writeFileData 来源于我自己 Promise 封装的 fs 模块，在给表弟编写账本软件 account-book 中封装的方法
[找到 dataFn.js，dataFn.js 还用到了 dirExists.js](https://github.com/Binote/account-book/tree/master/src/main)

## request 方法

我在网上还找到了用 request 模块的方法，关键在于设置 encoding: null,

```javascript
/**
   * 获取小程序qrcode
   *
   * @param {String} page
   * @param {String} scene
   * @returns
   * @memberof GetQrCodeObj
   */
  getQrCode2(page,pack,name,){
    return new Promise((resolve,reject)=>{
        const params = {
          url: 'https://api.weixin.qq.com/wxa/getwxacodeunlimit?access_token=' + this.access_token,
          method: "POST",
          json: true,
          encoding: null,  // 关键
          headers: {
              "content-type": "application/json",
          },
          body: {
              scene: scene,
              page,
              is_hyaline:true
          }
        }
        request(params, function(error, res, body) {
          if (!error && res.statusCode == 200) {
            dataFn.writeFileData(path.join(__dirname,'./dist/'+scene+'.png'),body).then(res=>{
              resolve(res)
            }).catch(err=>{
              reject(err)
            })
          } else {
            reject(error, body)
          }
      })
    })

  }
```
