```
/**
 * @description 注册全局函数
 * @params * fn {Function|Array<Function>} 注册的全局内容，可以是函数或者函数数组
 * @params * name {string} 注册的函数关键字，用于update remove识别
 */
match.register(fn, name);

/**
 * @description 更新全局函数
 * @params * fn {Function|Array<Function>} 注册的全局内容，可以是函数或者函数数组
 * @params * name {string} 更新的函数关键字
 */
match.update(fn, name);

/**
 * @description 删除全局函数
 * @params * name {string} 删除的函数关键字
 */
match.remove(name);
```

## etc:

1. register 函数
   ```
   // 映射枚举值
   let params = {
       type: 1
   };

   const enumConf = {
       typeOptions: [
           {
               id: 1,
               name: 'one'
           },
           {
               id: 2,
               name: 'two'
           }
       ]
   };

   let format = function (options, id, key, value) {
       let k = '';

       for (let i of options) {
           if (i[key] === id) {
               k = i[value];
           }
       }

       return k;
   };

   match.register(format, 'format');

   let data = match.parse(params, {
       typeId: '$${{type}}',
       typeName: function (data, format) {
           return format(enumConf.typeOptions, this.typeId, 'id', 'name');
       },
       typeNameAgain: (data, format) => format(enumConf.typeOptions, data.type, 'id', 'name') // es6
   });

   // 测试register为一个数组
   match.register([format], 'format');
   data = match.parse(params, {
       pid: 1,
       id: function (data, format) {
           // this 指向自身 data 指向params
           return data.pid + data.id + this.pid + format.b();
       }
   });
   ```
2. update 函数
   ```
   // 测试update
   let format = {
       a: function () {
           return 11;
       },
       b: function () {
           return 22;
       }
   };
   match.update(format, 'format');

   let data = match.parse(params, {
       pid: 1,
       id: function (data, format) {
           // this 指向自身 data 指向params
           return data.pid + data.id + this.pid + format.b();
       }
   });
   ```
3. remove 函数
   ```
   // 移除format 抛出异常
   match.remove('format');
   var data = match.parse(params, {
       pid: 1,
       id: function (data, format) {
           // this 指向自身 data 指向params
           return data.pid + data.id + this.pid + format.b();
       }
   });
   ```



