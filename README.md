## easyIDB
一个封装IndexedDB的简陋模块

通过几个简单（？）的函数操作IDB

## 使用方法
### 1、引入
```js
import easyIDB from './easyIDB.js'
```
or
```js
var easyIDB = require('./easyIDB.js')
```
or
```html
<script src="./easyIDB.js"></script>
<script>
  var myIDB = easyIDBMain('mydatabase')
</script>
```


### 2、实例化一个easyIDB对象

实际上是调用了openDB方法,具体参数参考第三条第一个方法

```js
let myIDB = easyIDB(IDBName,newIndex)
```
如果name为空,则返回一个未绑定数据库的easyIDB对象,需要使用openDB打开数据库

实例化之后可以通过该实例的方法例如

```js
    myIDB.get(storeName,key,value)
```

来操作数据库

### 3、具体方法

#### function openDB(IDBName,newIndexs)
打开数据库，没有则创建

参数说明

|  参数名  |        类型     | 必须  |   说明    |
| :------: | :------------: | :--: | :-------------------: |
| IDBName |  string\object   |  是  |       数据库名      |
| newIndexs | array          |  否  |     store列表  |

当IDBName是字符串时，直接作为数据库名字，打开当前已存在的版本的数据库，不进行数据库更新

当IDBName为对象时，可以指定数据库版本进行强制更新，参考参数如下
```js
｛
    name:"myIDB",
    ver:10
｝
```
仅当数据库进行版本更新或者在该用户上第一次创建数据库，会根据newIndexs数组进行store的创建

参考的newIndexs数组
```js
[
  {
     name:"xxx",//storeName
     indexs:[//添加索引
         ｛
              name:"mystore",
              unique:false//是否唯一 默认false
          ｝,

              name:"mystore2",
              unique:true
          ｝,
     ],
     option:{
       keyPath:'id',//主键,默认 'id'
       autoIncrement:true//是否自增,默认 true
     }
  },
  {
     name:"xxxx",//storeName
     indexs:[//添加索引
         ｛
              name:"mystore",
              unique:false//是否唯一 默认false
          ｝,

              name:"mystore2",
              unique:true
          ｝,
     ],
     option:{
       keyPath:'id',//主键,默认 'id'
       autoIncrement:true//是否自增,默认 true
     }
  }
]
```

---

#### function get(storeName:string,key:string,value:string?regexp)

\*这不是TS,只是方便看才这样写,下同

根据给定的storeName和key和value,在指定的store中寻找该对象,并返回一个数组(promise对象)

|  参数名  |        类型     | 必须  |   说明    |
| :------: | :------------: | :--: | :-------------------: |
| storeName |  string   |  是  |             |
| key | string          |  否  |     查找根据的键,既可以是主键也可以是索引  |
| value | string/regexp/array | 否| 查找值|

```js
async function(){
  let data = await myIDB.get('mystore','id','23')
  Array.isArray(data) // true
  data = await myIDB.get('mystore','name',/2$/)
  Array.isArray(data) // true
}
```
---

#### function push(storeName:string,data:object)

向指定的store添加记录

|  参数名  |        类型     | 必须  |   说明    |
| :------: | :------------: | :--: | :-------------------: |
| storeName |  string   |  是  |             |
| data | object/array          |  是  |     添加的数据  |

```js
myIDB.push('mystore',{name:'2233'})
myIDB.push('mystore',[
  {
    name:22
  },
  {
    name:33
  }
]])
```
---

#### function del(storeName:string,key:string,value:string)

在指定的store中删除一条记录

|  参数名  |        类型     | 必须  |   说明    |
| :------: | :------------: | :--: | :-------------------: |
| storeName |  string   |  是  |             |
| key | string          |  是  |     查找根据的键,既可以是主键也可以是索引  |
| value | string | 是| 查找值|

```js
async function(){
  await myIDB.del('mystore','id','2')
}
```

---

#### function clearStore(storeName:string)

删除一张表的全部记录

```js
myIDB.clearStore(storeName)
```

---

#### function deleteDatabase()
删除整个数据库

```js
myIDB.deleteDatabase()
```
