## Trigger

- **Type:** `string`
- **Default:** `''`
- **Usage:**

    ```html
// trigger: validate-name.event
// trigger: event
// trigger: data.change
// trigger: data.prototype
// triggerArr: join with ';'
<validate-form>
    <!-- trigger when blur -->
    <input type="text" validate-name="input" trigger="blur"/>

    <!-- trigger when the validate-name="input" blur -->
    <input type="text" v-model="abc" v-validate trigger="$input.blur"/>

    <!-- trigger when the data abc change -->
    <input type="text" v-validate trigger="$$abc.change"/>

    <!-- trigger when the data arr.length change -->
    <input type="text" v-validate trigger="$$arr.length"/>

    <!-- trigger when the validate-name="input" blur && the data abc change -->
    <input type="text" v-validate trigger="$$abc.change;blur"/>
</validate-form>
```
```js
data() {
    return {
        abc: '1',
        arr: [],
    };
};
    ```

- **Detail:**

    `event: blur | focus | input | keyup | keydown | keycode(default code = 13 enter) | keycode=\d`
    `prototype: length`

    dom must be input textarea or contenteditable=true.

    `event: change`

    **dom must has attribute with v-model.**

    `$validateName` begin with $ is a validate-name.
    `$$data` begin with $$ is a data.
