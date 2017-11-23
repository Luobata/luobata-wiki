## Component
```js
<validate-form ref="form" :rule="rule" :config="config">
    <input type="text" v-validate/>
    <input type="text" validate-name="input"/>
</validate-form>
```
## v-validate
- **Type:** `dom attribute`

A rule for the dom must have v-validate or validate-name.

## validate-name
- **Type:** `dom attribute`

if has rule in validate-form must have an unique validate-name.

### config

- **Default:** `{}`

same field as global-config, extend global-config to the current form.[config](config.md)

### rule

- **Default:** `{}`

the rule for validator, provides simply rules or complexity rules.[rule](rule.md)
