## Rule
字段校验规则

## Usage
- **Arguments:(key in Object)**
    - `{Object} validate` validate包含需要添加规则的validate-name
    - `{Object} data` data包含需要添加规则的data(vm.$data)
- **Usage:**

    ```html
// dom attribute
<validate-form>
    <input type="text" min=10 validate-name="input" trigger="blur"/>
</validate-form>
// rule-config
<validate-form :rule="rule">
    <input type="text" validate-name="input" trigger="blur"/>
</validate-form>
    ```
    ```js
const rule = {
    validate: {
        input: {
            min: 10,
        }
    }
};
    ```
可以作为dom的attribute字段使用

## Detail

### min Min

- **Type:** `string | number | function`

value 的最小值
**大小写表示边界值 比如min="2" 表示2符合条件 Min="2" 表示2不符合条件 即大写不包含边界 下同**

### max Max

- **Type:** `string | number | function`
- **Examples:**
    ```js
const rule = {
    validate: {
        input: {
            min: 10,
            min: '10',
            min: function (val) {
                // this vm实例
                // val 当前值
                // 注意需要使用this指针不用箭头函数
                if (this.a > 10) {
                    return 10;
                } else {
                    return 15;
                }
            }
        }
    }
};
    ```

value的最大值

### min-length / minlength(only-config)
### Min-length / Minlength(only-config)

- **Type:** `string | number | function`

value的最小长度
**only-config 表示该写法只能用于rule-config中 因为属性和原生dom的attribute命名冲突**

### max-length / maxlength(only-config)
### Max-length / Maxlength(only-config)

- **Type:** `string | number | function`

value的最大长度 **如果value的类型为数组 则校验的是数组长度**

### min-float-length / minfloatlength
### Min-float-length / Minfloatlength

- **Type:** `string | number | function`

value的小数点后最小长度

### max-float-length / maxfloatlength
### Max-float-length / Maxfloatlength

- **Type:** `string | number | function`

value的小数点后最大长度

### required

- **Type:** `any`
- **Default:** `''`
- **Example:**
    ```js
const rule = {
    validate: {
        input: {
            required: '', // 必填
            required: false, // 非必填
            min: function (val) {
                if (this.a > 10) {
                    return false; // 非必填
                }
                // 必填
            }
        }
    }
};
    ```

该字段必填
**只要不返回false 则认为该属性有效**

### phone

- **Type:** `any`
- **Default:** `''`
- **Regex:** `/^1[34578]\d{9}$/`

该字段符合一个手机号规则
**只要不返回false 则认为该属性有效**

### email

- **Type:** `any`
- **Default:** `''`
- **Regex:** `/^([a-zA-Z0-9_-])+@([a-zA-Z0-9_-])+(.[a-zA-Z0-9_-])+/`

该字段符合一个email规则
**只要不返回false 则认为该属性有效**

### positive Positive

- **Type:** `any`
- **Default:** `''`

该字段是一个正数
**大写包含0**
**只要不返回false 则认为该属性有效**

### negative Negative

- **Type:** `any`
- **Default:** `''`

该字段是一个负数
**大写包含0**
**只要不返回false 则认为该属性有效**

### number Number 

- **Type:** `string`
- **Default:** `int`
- **Value:**
    - `int` 整数
    - `float` 浮点数

该字段是一个数字
**小写表示可以是字符串数字 即 '123' 有效 大小必须是number类型**

### fun (only-config)

- **Type:** `Function`
- **Return:**
    - `false` false 校验错误
    - `Object` 返回一个rule对象 表示使用rule对象的规则进行校验
- **Example:**
    ```js
const rule = {
    validate: {
        input: {
            fun: function (val) {
                if (this.d > 40) {
                    // 满足条件 则必填
                    return {
                        required: '',
                    };
                }

                // 不满足条件 认为字段错误
                return false;
            }
        }
    }
};
    ```
