# match 全局config

**请谨慎修改全局config，一旦修改全局生效，建议控制改变全局config的入口，以免多人开发的过程中因为全局config的变化造成的影响**

```javascript
    // 默认config
    let config = {
        filterUndefined: true, // 过滤undefined
        filterNull: true, // 过滤null
        autoComplete: false, // 自动补全
        ignoreTokenKey: [] // 忽略解析的key
    };
    filterUndefined: match完如果是undefined则忽略该字段
    filterNull: match完如果是null则忽略该字段
    autoComplete: 自动补全原对象与目标对象下字段名相同的值
    ignoreTokenKey: 忽略关键字，命中关键字的key不进行匹配
```

###### 修改默认config

```javascript
   // 修改默认config
   let params = {
       pid: 1,
       id: 2
   };

   match.config({autoComplete: true});
   let data = match.parse(params, {
       id: '$${{id}}'
   });
   // {id: 2, pid: 1}
```
