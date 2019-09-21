# mock,一个可以模拟数据的假数据库

#### 首先安装

```c
npm i -g esky-mock //你也可以安装在你的项目的根目录下
```

#### 配置

```c
// 举例：项目根目录创建mock

// 配置文件 
//mock/config.js
module.exports =  {
  // 请求默认的延迟时间
  delay: 300,
  // 基础路径，假数据的URL则省略该路径不写
  base: 'api',
  // 服务端口，配合webpack的代理使用
  port: 3334,
  // 【必须】假数据文件目录相对路径(相对于node命令的执行路径)
  dataDir: 'mock/data'
}


//在mock目录下创建data
//在data目录下创建index.js和user.js

//index.js
const user = require('./user')
module.exports = Object.assign({}, user);

//user.js
/**
 * 数据模拟文件
 * http://mockjs.com/examples.html
 * NSIP 后台管理系统 账号管理模块模拟数据模板
 * 在浏览器中输入http://localhost:3334/api/account/query即可查看效果
 * 你可以通过修改response来制造其他假数据
 */
'use strict'
module.exports = {
    //客户审核-查询接口
    'account/query': {
        response: function (req, res) {
            return {
                "retcode": 200,
                "retdesc": "retdesc",
                "data": {
                    "totalCount": 30,
                    "result": [
                        {
                            "traderId": "@string('number',10)",
                            "idCardName": "@cname",
                            "accountLevel|0-1": 1,
                            "idCardPicStatus|0-3": 1,
                            "idCardNo": "@string('number',18)",
                            "commitRejectTime|+1": 1483434569921,
                            "rejectReason": "@csentence(5,15)",
                            "memberNo": "@string('number',3)",
                            "brokerNo": "@string('number',3)",
                            "idCardFrontImg": "@image()",
                            "idCardBackImg": "@image()",
                            "idCardHoldImg": "@image()"
                        }
                    ]
                },
                "string|1-10": "★"
            }
        }
    },
    '/account/account.html': {
        response(req, res) {
            res.redirect('/account/mashang.html?status=1')
        }
    },
    '/account/mashang.html': {
        response(req, res) {
            // 可以操作response对象来响应内容
            setTimeout(() => {
                res.send(`
        <html>
          <head></head>
          <body>123</body>
        </html>
        `)
            },1000)
        }
    }
}
```

#### 运行使用

```c
npm esky-mock -c mock/config.js //你可以通过在webpack中配置脚本来简化运行命令
```

#### 查看效果

完成以上步骤后，你可以在浏览器中输入`http://localhost:3334/api/account/query`查看模拟的数据，你也可以把这个地址作为接口通过Ajax来获取其中的数据

## 更多用法参考：http://mockjs.com/
