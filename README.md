## easyIDB
一个封装IndexedDB的简陋模块

通过几个简单（？）的函数操作IDB

## 使用方法
1、引入
```js
import easyIDB from './easy-idb.js'
```

2、实例化一个easyIDB对象
（即打开数据库，没有则创建）
当前只支持初始化创建一个store
```js
let myIDB = easyIDB(IDBName,newIndex)
```
参数说明
|   参数名    |         类型          | 必须  |        说明       |
| :------: | :-------------------: | :--: | :-------------------: |
| IDBName |      string\object       |  是  |       数据库名      |
|   newIndex   | object               |  否  |     单个store的索引列表  |
当IDBName是字符串时，直接作为数据库名字，打开当前已存在的版本的数据库，不进行数据库更新
当IDBName为对象时，可以指定数据库版本进行强制更新，参考参数如下
```js
｛
    name:"myIDB",
    ver:10
｝
```
仅当数据库进行版本更新或者在该用户上第一次创建数据库，会根据newIndex对象进行store的创建
参考newIndex对象
```js
{
   name:"xxx",//storeName
   indexs:[//添加索引
       ｛
            name:"mystore",
            unique:false//是否唯一 默认false
        ｝,
        
            name:"mystore2",
            unique:true//默认值
        ｝,
   ],
   option:{
     keyPath:'id',//主键
     autoIncrement:true//是否自增
   }
}
```
实例化之后可以通过该实例的方法例如
```js
    myIDB.get(storeName,key,value)
```
来操作数据库

3、具体方法
