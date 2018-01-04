# match 全局 config

**请谨慎修改全局 config，一旦修改全局生效，建议控制改变全局 config 的入口，以免多人开发的过程中因为全局 config 的变化造成的影响**

```javascript
    // 默认config
    let config = {
        filterUndefined: true, // 过滤undefined
        filterNull: true, // 过滤null
        filterEmptyObject: false, // 过滤空对象
        filterDefaultArray: false, // 过滤匹配数组产生的Array 不过滤自定义的返回值 []
        filterDefaultObject: false, // 过滤匹配对象产生的Object 不过滤自定的返回值 {}
        autoComplete: false, // 自动补全
        ignoreTokenKey: [] // 忽略解析的key
    };
    filterUndefined: match完如果是undefined则忽略该字段
    filterNull: match完如果是null则忽略该字段
    autoComplete: 自动补全原对象与目标对象下字段名相同的值
    ignoreTokenKey: 忽略关键字，命中关键字的key不进行匹配
```

###### 修改默认 config

```javascript
// 修改默认config(全局生效)
let params = {
    pid: 1,
    id: 2,
};

match.config({autoComplete: true});
let data = match.parse(params, {
    id: '$${{id}}',
});
assert.deepEqual(data, {
    pid: 1,
    id: 2,
});

// 修改当前操作cofnig
params = {
    pid: 1,
    id: 2,
    cityId: 2,
};
data = match.parseConfig(
    params,
    {
        id: '$${{ID}}',
        pid: '$${{pid}}',
    },
    {
        autoComplete: true,
        filterUndefined: false,
    },
);
expect(data).to.be.eql({
    id: undefined,
    pid: 1,
    cityId: 2,
});
```
