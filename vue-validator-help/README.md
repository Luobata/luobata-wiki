# Introduction
A vue plugin to solve the situation when form is need to validate by complex conditions.

**You can not validate a form item triger by other item's change with other validate plugin.**

## Installation
```npm install --save-dev vue-validator-help```

## Usage
```js
import Vue from 'vue';
import validate from 'vue-validator-help';

Vue.use(validate);
```

then in your component template, add the validate-form.
```js
<validate-form ref="form" :rule="rule" :config="config">
    <input type="text" v-validate/>
    <input type="text" validate-name="input"/>
</validate-form>
```
**if you need to show error text or other style when error, you need to add a object errors in your component like**
```js
// vue js
export default {
    data() {
        return {
            errors: {},
        }
    }
};
// when the validate-name="input" is error, the validate from will add the key into errors
errors: {
    input: true,
    inputError: '', // the error text is defined by you config more detail in component
};
```
