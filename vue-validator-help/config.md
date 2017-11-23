## Global Config

```js
import validate from 'vue-validator-help';
const config = {}
Vue.use(validate, config);
```

### length-type
- **Type:** `string`
- **Default:** `eng`
- **Usage && Value:**
```js
const config = {
    lengthType: chi, // 中文字符算2个字符长度 其余1个字符长度
    lengthType: eng, // 中文字符 全角半角一律1个字符长度
};
```
