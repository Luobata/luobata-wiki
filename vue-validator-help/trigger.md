## Trigger

- **Type:** `string`
- **Default:** `''`
- **Usage:**
```js
    // trigger: validate-name.event
    // trigger: event
    // trigger: data.change
    // triggerArr: join with ;

    <input type="text" validate-name="input" trigger="blur"/>
    <input type="text" v-model="abc" v-validate trigger="$input.blur"/>
    <input type="text" v-validate trigger="$$abc.change"/>
    <input type="text" v-validate trigger="$$abc.change;blur"/>
```

- **Detail:**

    `event: blur | focus | input | keyup | keydown | keycode(default code = 13 enter) | keycode=\d`

    dom must be input textarea or contenteditable=true.

    `event: change`

    **dom must has attribute with v-model.**
