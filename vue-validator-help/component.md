## Component
```js
<validate-form ref="form" :rule="rule" :config="config">
    <input type="text" v-validate/>
    <input type="text" validate-name="input"/>
</validate-form>
```
---
## attribute

### v-validate
- **Type:** `dom attribute`

A rule for the dom must have v-validate or validate-name.

### validate-name
- **Type:** `dom attribute`

if has rule in validate-form must have an unique validate-name.

### config

- **Default:** `{}`

same field as global-config, extend global-config to the current form.[config](config.md)

### rule

- **Default:** `{}`

the rule for validator, provides simply rules or complexity rules.[rule](rule.md)

---
## methods

### validateAll
- **Description:** validate all item in the validate-form
- **Return:** `false | true`
- **Usage:** 
```js
    const form = this.$refs['form'];
    form.validateAll();
```

### validate(name)
- **Description:** validate specific item
- **Arguments:**
    - `{string} validate-name`
- **Return:** `false | true`
- **Usage:**
```js
    const form = this.$refs['form'];
    // input is a validate-name(attribute in dom)
    form.validate('input');
```
