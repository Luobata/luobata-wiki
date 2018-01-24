```javascript
/**
 * @description match字段映射转换函数
 * @params * params {object|Array} 用于转化的数据对象或数组
 * @params * matchObj {object|Array} 用于转化的规则对象或数组
 */
match.parse(params, matchObj);

/**
 * @description match字段映射转换函数
 * @params * params {object|Array} 用于转化的数据对象或数组
 * @params * matchObj {object|Array} 用于转化的规则对象或数组
 * @params * tmpConfig {object} 临时的config配置 只对本次转化有效
 */
match.parseConfig(params, matchObj, tmpConfig);
```

## etc:

###### 映射普通字段

```javascript
const match = import 'lib-match';

let params = {
   title: 'title',
};
let data = match.parse(params, {
   title: '$${{title}}',
   msg: 'this is string',
});
assert.deepEqual(data, {
    title: 'title',
    msg: 'this is string',
});
```

###### 映射对象属性

```javascript
let params = {
    title: 'title',
    data: {
        city: '北京市',
        province: '北京',
    },
};

// 映射普通字段
let data = match.parse(params, {
    title: '$${{title}}',
    data: {
        city: '$${{data.city}}',
        pro: '$${{data.province}}',
    },
});
assert.deepEqual(data, {
    title: 'title',
    data: {
        city: '北京市',
        province: '北京',
    },
});
```

###### 映射对象所有字段为空

```js
let params = {};
let data = match.parse(params, {
    title: '$${{abc}}',
    data: {
        id: '$${{name.id}}',
    },
    text: {
        title: '$${{name.abc}}',
    },
});
// 为空返回空对象 由config.filterEmptyObject 或者 config.filterDefaultObject 控制 详见config部分
assert.deepEqual(data, {
    data: {},
    text: {},
});
```

###### 映射字段带有默认值

```javascript
let params = {
    title: 'title',
    id: 1,
};

// 映射带有默认值
let data = match.parse(params, {
    title: '$${{title}} || 123', // 默认值为number类型 123
    id: '$${{id}} || "123"', // 默认值为string类型 123
    name: '$${{name}} || []', // 默认值为数组类型 数组类型暂时只能空数组
    value: '$${{value}} || {}', // 默认值为对象类型 数组类型暂时只能空对象
});
assert.deepEqual(data, {
    title: 'title',
    id: 1,
    name: [],
    value: {},
});
```

###### || 语法嵌套

```js
let params = {
    id: 0,
    c: 1,
    city: 2,
};
let data = match.parse(params, {
    id: '$${{ids}} || 123',
    city: '$${{c}} || $${{city}} || 1',
    city2: '$${{province}} || 4',
    city3: '$${{c2}} || $${{city}} || 1',
});
let assert.deepEqual(data, {
    id: 123,
    city: 1,
    city2: 4,
    city3: 2,
});
```

###### 映射入口为数组

```javascript
let params = [
    {
        code: 200,
        msg: 'ok',
        data: [1, 2, 3],
    },
    {
        code: 500,
        msg: 'error',
        data: [4, 5, 6],
    },
];

let data = match.parse(params, {
    code: '${0.code}',
    msg: '${1.msg}',
    data: function(data) {
        return data[0].data.concat(data[1].data); // [1, 2, 3, 4, 5, 6]
    },
});
let assert.deepEqual(data, {
    code: 200,
    msg: 'error',
    data: [1, 2, 3, 4, 5, 6],
});
```

###### 映射返回数组/对象数组

```javascript
let params = {
    code: 200,
    msg: 'success',
    data: [
        {
            id: 1,
            type: 'a',
        },
        {
            id: 2,
        },
    ],
};

// 映射对象数组 结果返回对象
let data = match.parse(params, {
    data: [
        'data',
        {
            id: '$${{id}}',
            title: 'string',
            type: "$${{type}} || 'abc'",
        },
    ],
});
assert.deepEqual(data, [
    {
        id: 1,
        title: 'string',
        type: 2,
    },
    {
        id: 2,
        title: 'string',
        type: 'abc',
    },
]);

params = [
    {
        id: 1,
        type: 'a',
    },
    {
        id: 2,
    },
];

// 直接映射数组 结果返回数组
data = match.parse(params, [
    {
        id: '$${{id}}',
        title: 'string',
        type: "$${{type}} || 'abc'",
    },
]);
assert.deepEqual(data, [
    {
        id: 1,
        title: 'string',
        type: 'a',
    },
    {
        id: 2,
        title: 'string',
        type: 'abc',
    },
]);

params = {
    data: [
        {
            id: 1,
            type: 'a',
        },
        {
            id: 2,
        },
    ],
};
// 映射 params.data 结果返回数组
data = match.parse(params, [
    'data',
    {
        id: '$${{id}}',
        title: 'string',
        type: "$${{type}} || 'abc'",
    },
]);
assert.deepEqual(data, [
    {
        id: 1,
        title: 'string',
        type: 'a',
    },
    {
        id: 2,
        title: 'string',
        type: 'abc',
    },
]);

params = {
    data: {
        table: [
            {
                id: 1,
                type: 2,
            },
            {
                id: 2,
            },
        ],
    },
};
data = match.parse(params, [
    'data.table',
    {
        id: '$${{id}}',
        title: 'string',
        type: "$${{type}} || 'abc'",
    },
]);

assert.deepEqual(data, [
    {
        id: 1,
        title: 'string',
        type: 2,
    },
    {
        id: 2,
        title: 'string',
        type: 'abc',
    },
]);

// 映射匹配的数组为空 返回空数组 可以由config.filterDefaultArray控制
params = null;
data = match.parse(params, [
    {
        id: '$${{id}}',
        title: 'string',
        type: "$${{type}} || 'abc'",
    },
]);
assert.deepEqual(data, []);

params = {};
data = match.parse(params, [
    'data',
    {
        id: '$${{id}}',
        title: 'string',
        type: "$${{type}} || 'abc'",
    },
]);
assert.deepEqual(data, []);

params = {
    code: 200,
    msg: 'ok',
    data: null,
};
data = match.parse(params, {
    code: '$${{code}}',
    msg: '$${{msg}}',
    data: [
        'data',
        {
            id: '$${{id}}',
            title: 'string',
            type: "$${{type}} || 'abc'",
        },
    ],
});
assert.deepEqual(data, {
    code: 200,
    msg: 'ok',
    data: [],
});
```

###### 通过字符串字段名映射数组对象(只需要一个字段)

```js
let params = {
    code: 200,
    msg: 'ok',
    data: [
        {
            id: 1,
            name: 2,
        },
        {
            id: 2,
            name: 3,
        },
    ],
};

let data = match.parse(params, ['data', 'id']);

assert.deepEqual(data, [1, 2]);

data = match.parse(params, {
    code: '$${{code}}',
    msg: '$${{msg}}',
    data: ['data', 'id'],
});
assert.deepEqual(data, {
    code: 200,
    msg: 'ok',
    data: [1, 2],
});
```

###### **映射 function**

```javascript
let params = {
    pid: 2,
    id: 3,
};

// 映射 function
let data = match.parse(params, {
    pid: 1,
    id: function(data) {
        // this 指向自身 data 指向params
        return data.pid + data.id + this.pid; // 6
    },
});
```

###### **映射带有临时 config**

```javascript
// 详细config见config模块
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

data = match.parse(params, {
    id: '$${{id}}',
});
expect(data).to.be.eql({
    id: 2,
});

data = match.parseConfig(
    params,
    {
        id: '$${{id}}',
        pid: '$${{pid}}',
    },
    {
        ignoreTokenKey: ['id'],
    },
);

expect(data).to.be.eql({
    id: '$${{id}}',
    pid: 1,
});

data = match.parse(params, {
    id: '$${{id}}',
});
expect(data).to.be.eql({
    id: 2,
});
```

###### **映射带有类型转换**

```javascript
// boolean 布尔值 'true' 转化为true 'false'转化为false
// Boolean 布尔值 对转化后的值强制类型转换 !!(变量)
// !boolean 布尔值 boolean的结果取非
// !Boolean 布尔值 Boolean的结果取非
// int 数值值 parseInt(x, 10);
// float 书纸值 parseFloat(x, 10);
// string 字符串值 x.toString();
let params = {
    pid: 'false',
    name: 1,
    id: '2',
    city: 1,
    district: '1.56',
};
let data = match.parse(params, {
    pid: '(boolean)$${{pid}}', // false
    pid2: '!(boolean)$${{pid}}', // true
    Pid: '(Boolean)$${{pid}}', // true
    Pid2: '!(Boolean)$${{pid}}', // false
    name: '(Boolean)$${{name}}', // true
    id: '(int)$${{id}}', // 2
    city: '(string)$${{city}}', // '1'
    dis: '(float)$${{district}}', // 1.56
});

data = match.parse([params], {
    pid: '(boolean)${0.pid}', // false
    pid2: '!(boolean)${0.pid}', // true
    Pid: '(Boolean)${0.pid}', //true
    Pid2: '!(Boolean)${0.pid}', // false
    name: '(Boolean)${0.name}', // true
    id: '(int)${0.id}', // 2
    city: '(string)${0.city}', // '1'
    dis: '(float)${0.district}', // 1.56
});

params = [
    {
        id: 1,
        type: 2,
    },
    {
        id: 2,
    },
];
data = match.parse(params, [
    {
        id: '(string)$${{id}}',
        title: 'string',
        type: "$${{type}} || 'abc'",
    },
]);
```
