# chain 链式调用

````js
/**
 * @description parse tmpConfig 用法会返回一个match对象 可以链式调用
 * tmpConfig 返回的对象会保留临时的config设置
 */
```

## etc:
```js
let params = {
    code: '200',
    msg: 'ok',
    http: 200,
    data: {
        cityId: '1',
        provinceId: 2,
        dis: 2,
    },
};
let data = match
    .tmpConfig({
        autoComplete: true,
        ignoreTokenKey: ['test'],
    })
    .parse(params, {
        data: {
            city: '$${{data.cityId}}',
            province: '$${{data.provinceId}}',
        },
    });
assert.deepEqual(data, {
    code: '200',
    msg: 'ok',
    http: 200,
    data: {
        cityId: '1',
        provinceId: 2,
        dis: 2,
        city: '1',
        province: 2,
    },
});

params = null;
data = match
    .tmpConfig({
        filterDefaultArray: true,
    })
    .parse(params, [
        {
            id: '$${{id}}',
            title: 'string',
            type: "$${{type}} || 'abc'",
        },
    ]);
assert.deepEqual(data, undefined);

params = {};
data = match
    .tmpConfig({
        filterDefaultArray: true,
    })
    .parse(params, [
        'data',
        {
            id: '$${{id}}',
            title: 'string',
            type: "$${{type}} || 'abc'",
        },
    ]);
assert.deepEqual(data, undefined);

params = {
    code: 200,
    msg: 'ok',
    data: null,
};
data = match
    .tmpConfig({
        filterDefaultArray: true,
    })
    .parse(params, {
        code: '$${{code}}',
        msg: '$${{msg}}',
        array: '$${{array}} || []',
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
    array: [],
});
````
