## Trigger

- **Type:** `string`
- **Default:** `''`
- **Usage:**

    ```html
// trigger: validate-name.event
// trigger: event
// trigger: data.change
// triggerArr: join with ';'
<validate-form>
    <input type="text" validate-name="input" trigger="blur"/>
    <input type="text" v-model="abc" v-validate trigger="$input.blur"/>
    <input type="text" v-validate trigger="$$abc.change"/>
    <input type="text" v-validate trigger="$$abc.change;blur"/>
</validate-form>
    ```

- **Detail:**

    `event: blur | focus | input | keyup | keydown | keycode(default code = 13 enter) | keycode=\d`

    dom must be input textarea or contenteditable=true.

    `event: change`

    **dom must has attribute with v-model.**
