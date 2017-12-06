## Column ACL REST API

> **提示**
>
> 使用API前请确保已阅读前面的 访问及安全验证 章节，知道如何在API中添加认证信息
>


* [获得表下列的黑名单](#获得表下列的黑名单)
* [添加用户不能访问的列](#添加用户不能访问的列)
* [修改用户不能访问的列](#修改用户不能访问的列)
* [删除用户的列级ACL](#删除用户的列级ACL)

### 获得表下列的黑名单
`请求方式 GET`

`访问路径 http://host:port/kylin/api/acl/column/{project}/{table}`

#### 路径变量
* project - `必选` `string` project
* table - `必选` `string` table

#### 请求示例
`请求路径:http://host:port/kylin/api/acl/column/learn_kylin/DEFAULT.KYLIN_SALES`

#### 响应示例
```json
{
  "code": "000",
  "data": {
    "{a,u}": [
      "TRANS_ID"
    ],
    "{ADMIN,u}": [
      "TRANS_ID"
    ],
    "{ANALYST,u}": [
      "BUYER_ID",
      "OPS_REGION",
      "OPS_USER_ID",
      "SELLER_ID",
      "TRANS_ID"
    ],
    "{g1,g}": [
      "LSTG_FORMAT_NAME",
      "PART_DT",
      "TRANS_ID"
    ],
    "{u1,u}": [
      "c1",
      "c2"
    ]
  },
  "msg": "get column acl"
}
```

### 添加用户不能访问的列
`请求方式 POST`

`访问路径 http://host:port/kylin/api/acl/column/{project}/{type}/{table}/{username}`

#### 路径变量
* project - `必选` `string` project
* type - `必选` `string`用来标示操作是用户还是组,取值:user/group
* table - `必选` `string` table
* username - `必选` `string` username

#### 请求主体
* columns - `必选` `string列表` columns,详见下面的请求示例中的request body

#### 请求示例
`请求路径:http://host:port/kylin/api/acl/column/learn_kylin/user/DEFAULT.KYLIN_CAL_DT/ADMIN`

`request body:["CAL_DT", "YEAR_BEG_DT"]`

#### 响应示例
```json
{"code":"000","data":"","msg":"add user to column balck list."}
```

### 修改用户不能访问的列
`请求方式 PUT`
`访问路径 http://host:port/kylin/api/acl/column/{project}/{type}/{table}/{username}`

#### 路径变量
* project - `必选` `string` project
* type - `必选` `string`用来标示操作是用户还是组,取值:user/group
* table - `必选` `string` table
* username - `必选` `string` username

#### 请求主体
* columns - `必选` `string列表` columns,详见下面的请求示例中的request body

#### 请求示例
`请求路径:http://host:port/kylin/api/acl/column/learn_kylin/user/DEFAULT.KYLIN_CAL_DT/ADMIN`

`request body:["YEAR_BEG_DT", "CAL_DT", "QTR_BEG_DT"]`

#### 响应示例
```json
{"code":"000","data":"","msg":"update user's black column list"}
```

### 删除用户的列级ACL
`请求方式 DELETE`

`访问路径 http://host:port/kylin/api/acl/column/{project}/{type}/{table}/{username}`

#### 路径变量
* project - `必选` `string` project
* type - `必选` `string`用来标示操作是用户还是组,取值:user/group
* table - `必选` `string` table
* username - `必选` `string` username

#### 请求示例
http://host:port/kylin/api/acl/column/learn_kylin/user/DEFAULT.KYLIN_CAL_DT/ADMIN

#### 响应示例
```
{"code":"000","data":"","msg":"delete user from DEFAULT.KYLIN_CAL_DT's column black list"}
```