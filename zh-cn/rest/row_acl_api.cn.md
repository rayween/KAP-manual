## Row ACL REST API

> **提示**
>
> 使用API前请确保已阅读前面的 访问及安全验证 章节，知道如何在API中添加认证信息
>


* [获得用户的行级ACL](#获得用户的行级ACL)
* [添加用户的行级ACL](#添加用户的行级ACL)
* [修改用户的行级ACL](#修改用户的行级ACL)
* [删除用户的行级ACL](#删除用户的行级ACL)

### 获得用户的行级ACL
`请求方式 GET`

`访问路径 http://host:port/kylin/api/acl/row/{project}/{table}`

#### 路径变量
* project - `必选` `string` project
* table - `必选` `string` table

#### 请求示例
`请求路径:http://host:port/kylin/api/acl/row/learn_kylin/DEFAULT.KYLIN_SALES`

#### 响应示例
```json
{
  "code": "000",
  "data": {
    "{ADMIN,u}": {
      "condsWithColumn": {
        "TRANS_ID": [
          {
            "type": "CLOSED",
            "leftExpr": "1",
            "rightExpr": "1"
          },
          {
            "type": "CLOSED",
            "leftExpr": "2",
            "rightExpr": "2"
          },
          {
            "type": "CLOSED",
            "leftExpr": "3",
            "rightExpr": "3"
          }
        ]
      }
    },
    "{ALL_USERS,g}": {
      "condsWithColumn": {
        "TRANS_ID": [
          {
            "type": "CLOSED",
            "leftExpr": "1",
            "rightExpr": "1"
          },
          {
            "type": "CLOSED",
            "leftExpr": "2",
            "rightExpr": "2"
          },
          {
            "type": "CLOSED",
            "leftExpr": "3",
            "rightExpr": "3"
          }
        ]
      }
    }
  },
  "msg": "get column cond list in table"
}
```

### 添加用户的行级ACL
`请求方式 POST`

`访问路径 http://host:port/kylin/api/acl/row/{project}/{type}/{table}/{username}`

#### 路径变量
* project - `必选` `string` project
* type - `必选` `string`用来标示操作是用户还是组,取值:user/group
* table - `必选` `string` table
* username - `必选` `string` username

#### 请求主体
* condsWithColumn - `必选` `map` 列与conditions的键值对,详见下面的请求示例中的request body

#### 请求示例
`请求路径:http://host:port/kylin/api/acl/row/learn_kylin/DEFAULT.KYLIN_SALES/ADMIN`

```
request body:代表 TRANS_ID 的取值为1,2,3.(TRANS_ID=1 OR TRANS_ID=2 OR TRANS_ID=3)

{
  "condsWithColumn": {
    "TRANS_ID": [
      {
        "type": "CLOSED",
        "leftExpr": "1",
        "rightExpr": "1"
      },
      {
        "type": "CLOSED",
        "leftExpr": "2",
        "rightExpr": "2"
      },
      {
        "type": "CLOSED",
        "leftExpr": "3",
        "rightExpr": "3"
      }
    ]
  }
}
```

#### 响应示例
```json
{"code":"000","data":"","msg":"add user row cond list."}
```

### 修改用户的行级ACL
`请求方式 PUT`
`访问路径 http://host:port/kylin/api/acl/row/{project}/{type}/{table}/{username}`

#### 路径变量
* project - `必选` `string` project
* type - `必选` `string`用来标示操作是用户还是组,取值:user/group
* table - `必选` `string` table
* username - `必选` `string` username

#### 请求主体
* condsWithColumn - `必选` `map` 列与conditions的键值对,详见下面的请求示例

#### 请求示例
`请求路径:http://host:port/kylin/api/acl/row/learn_kylin/user/DEFAULT.KYLIN_SALES/ADMIN`

```
request body: 代表 TRANS_ID 的取值为1,2,3.(TRANS_ID=1 OR TRANS_ID=2 OR TRANS_ID=3)

{
  "condsWithColumn": {
    "TRANS_ID": [
      {
        "type": "CLOSED",
        "leftExpr": "1",
        "rightExpr": "1"
      },
      {
        "type": "CLOSED",
        "leftExpr": "2",
        "rightExpr": "2"
      },
      {
        "type": "CLOSED",
        "leftExpr": "3",
        "rightExpr": "3"
      }
    ]
  }
}
```

#### 响应示例
```json
{"code":"000","data":"","msg":"update user's black column list"}
```

### 删除用户的行级ACL
`请求方式 DELETE`

`访问路径 http://host:port/kylin/api/acl/row/{project}/{type}/{table}/{username}`

#### 路径变量
* project - `必选` `string` project
* type - `必选` `string`用来标示操作是用户还是组,取值:user/group
* table - `必选` `string` table
* username - `必选` `string` username

#### 请求示例
http://host:port/kylin/api/acl/row/learn_kylin/user/DEFAULT.KYLIN_SALES/ADMIN

#### 响应示例
```
{"code":"000","data":"","msg":"delete user's row cond list"}
```