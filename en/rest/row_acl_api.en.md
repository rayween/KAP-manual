## Row ACL REST API

> **Tip**
>
> Before using API, please make sure that you have read the previous chapter of *Access and Authentication* in advance and know how to add verification information in API. 


* [Get Row ACL](#get-row-acl)
* [Add Row ACL](#add-row-acl)
* [Modify Row ACL](#modify-row-acl)
* [Delete Row ACL](#delete-row-acl)

### Get Row ACL
`Request Mode GET`

`Access Path http://host:port/kylin/api/acl/row/{project}/{table}`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Path Variable
* project - `required` `string`, project name
* table - `required` `string`, table name

#### Request Example
`Request Path:http://host:port/kylin/api/acl/row/learn_kylin/DEFAULT.KYLIN_SALES`

#### Response Example
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

### Add Row ACL
`Request Mode POST`

`Access Path http://host:port/kylin/api/acl/row/{project}/{type}/{table}/{username}`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Path Variable
* project - `required` `string`, project name
* type - `required` `string`, indicate the type of action, value: user/group
* table - `required` `string`, table name
* username - `required` `string`, user name

#### Request Body
* condsWithColumn - `required` `map`, key-value pairs of column and conditions. For details, see the request body in the following request example.

#### Request Example
`Request Path:http://host:port/kylin/api/acl/row/learn_kylin/DEFAULT.KYLIN_SALES/ADMIN`

```
Request Body:TRANS_ID's values:1,2,3.(TRANS_ID=1 OR TRANS_ID=2 OR TRANS_ID=3)

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

#### Response Example
```json
{"code":"000","data":"","msg":"add user row cond list."}
```

### Modify Row ACL
`Request Mode PUT`
`Access Path http://host:port/kylin/api/acl/row/{project}/{type}/{table}/{username}`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Path Variable
* project - `required` `string`, project name
* type - `required` `string`, indicate the type of action, value: user/group
* table - `required` `string`, table name
* username - `required` `string`, user name

#### Request Body
* condsWithColumn - `required` `map`, key-value pairs of column and conditions. For details, see the request body in the following request example.

#### Request Example
`Request Path:http://host:port/kylin/api/acl/row/learn_kylin/user/DEFAULT.KYLIN_SALES/ADMIN`

```
Request Body:TRANS_ID's values:1,2,3.(TRANS_ID=1 OR TRANS_ID=2 OR TRANS_ID=3)

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

#### Response Example
```json
{"code":"000","data":"","msg":"update user's black column list"}
```

### Delete Row ACL
`Request Mode DELETE`

`Access Path http://host:port/kylin/api/acl/row/{project}/{type}/{table}/{username}`

`Content-Type: application/vnd.apache.kylin-v2+json`

#### Path Variable
* project - `required` `string`, project name
* type - `required` `string`, indicate the type of action, value: user/group
* table - `required` `string`, table name
* username - `required` `string`, user name

#### Request Body

`Request Path:http://host:port/kylin/api/acl/row/learn_kylin/user/DEFAULT.KYLIN_SALES/ADMIN`

#### Response Example
```
{"code":"000","data":"","msg":"delete user's row cond list"}
```