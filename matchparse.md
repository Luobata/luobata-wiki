

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

    ### etc:
    1. 映射普通字段
    ```
    const match = import 'lib-match';

    let params = {
        title: 'title'
    };

    // 映射普通字段
    let data = match.parse(params, {
        title: '$${{title}}',
        msg: 'this is string'
    });
    ```

    2. 映射对象属性
    ```
    let params = {
        title: 'title',
        data: {
            city: '北京市',
            province: '北京'
        }
    };

    // 映射普通字段
    let data = match.parse(params, {
        title: '$${{title}}',
        data: {
            city: '$${{data.city}}',
        pro: '$${{data.province}}'
    }
    });
    ```

    3. 映射字段带有默认值
    ```
    let params = {
        title: 'title',
        id: 1
    };

    // 映射带有默认值
    let data = match.parse(params, {
        title: '$${{title}} || 123', // 默认值为number类型 123
        id: '$${{id}} || "123"', // 默认值为string类型 123
        name: '$${{name}} || []', // 默认值为数组类型 数组类型暂时只能空数组
        value: '$${{value}} || {}', // 默认值为对象类型 数组类型暂时只能空对象
    });
    ```

    4. 映射入口为数组
    ```
    let params = [
        {
            code: 200,
            msg: 'ok',
            data: [1, 2, 3]
        },
        {
            code: 500,
            msg: 'error',
            data: [4, 5, 6]
        }
    ];

    let data = match.parse(params, {
        code: '${0.code}',
        msg: '${1.msg}',
        data: function (data) {
            return data[0].data.concat(data[1].data); // [1, 2, 3, 4, 5, 6]
        }
    });
    ```

    5. 映射返回数组/对象数组
    ```
    let params = {
        code: 200,
        msg: 'success',
        data: [
            {
                id: 1,
                type: 'a'
            },
            {
                id: 2
            }
        ]
    };

    // 映射对象数组 结果返回对象
    let data = match.parse(params, {
        code: '$${{code}}',
        msg: '$${{msg}}',
        data: ['data', {
            id: '$${{id}}',
            title: 'string',
            type: "$${{type}} || 'abc'"
        }]
    });

    params = [
        {
            id: 1,
            type: 'a'
        },
        {
            id: 2
        }
    ];

    // 直接映射数组 结果返回数组
    data = match.parse(params, [{
        id: '$${{id}}',
        title: 'string',
        type: "$${{type}} || 'abc'"
    }]);

    params = {
        data: [
            {
                id: 1,
                type: 'a'
            },
            {
                id: 2
            }
        ]
    };
    // 映射 params.data 结果返回数组
    data = match.parse(params, ['data', {
        id: '$${{id}}',
        title: 'string',
        type: "$${{type}} || 'abc'"
    }]);
    ```

    6. 映射function
    ```
    let params = {
        pid: 2,
        id: 3
    };

    // 映射 function
    let data = match.parse(params, {
        pid: 1,
        id: function (data) {
            // this 指向自身 data 指向params
            return data.pid + data.id + this.pid; // 6
        }
    });
    ```

    7. 映射带有临时config
    ```
    params = {
        pid: 1,
        id: 2,
        cityId: 2
    };
    data = match.parseConfig(params, {
        id: '$${{ID}}',
        pid: '$${{pid}}'
    }, {
        autoComplete: true,
        filterUndefined: false
    });
    expect(data).to.be.eql({
        id: undefined,
        pid: 1,
        cityId: 2
    });

    data = match.parse(params, {
        id: '$${{id}}'
    });
    expect(data).to.be.eql({
        id: 2
    });

    data = match.parseConfig(params, {
        id: '$${{id}}',
        pid: '$${{pid}}'
    }, {
        ignoreTokenKey: ['id']
    });

    expect(data).to.be.eql({
        id: '$${{id}}',
        pid: 1
    });

    data = match.parse(params, {
        id: '$${{id}}'
    });
    expect(data).to.be.eql({
        id: 2
    });



