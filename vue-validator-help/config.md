## Global Config
**The global-config can be extend by config on component **
```js
import validate from 'vue-validator-help';
const config = {}
Vue.use(validate, config);
```

### length-type
- **Type:** `string`
- **Default:** `eng`
- **Usage && Value:**

### errorName
- **Type:** `string`
- **Default:** `errors`
- **Usage && Value:**
**用来定义vm实例下的对象名，当校验错误时会自动在该对象下增加对应的属性值*，必须存在且是一个对象**

```js
const config = {
    lengthType: 'chi', // 中文字符算2个字符长度 其余1个字符长度
    lengthType: 'eng', // 中文字符 全角半角一律1个字符长度
    errorName: 'errors',
};

// for errorName
export default {
    data() {
        return {
            errors: {},
        };
    }
};
```

## Component Config
**Component config 与Global config内容相同 在component上作用:cofnig属性，或者直接赋值对应的规则属性。Component cofnig可以覆盖Global config。**

**权重 component-config对象 > component config属性 > global-config对象**
```js
// global
Vue.use(validate, {
    lengthType: 'chi',
    errorName: 'er',
});

// component-config && component-attribute
<validate-form :config="config" length-type="chi" errorName="err">
</validate-form>

// :config对象的优先级 > attrtibute > global
```
