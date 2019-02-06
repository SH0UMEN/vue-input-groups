# Vue Input Groups
Vue input groups with validation

[Demo and guide]() (Still in progress)

## Usage
Import package in your project:

```Javascript
import InputGroup from "vue-input-groups"

//...

components: {
    InputGroup
}
```

Or, without bundlers:

```html
<link rel="stylesheet" href="css/VueInputGroups.css">
<!--...-->
<script src="js/VueInputGroups.umd.min.js"></script>
```
```Javascript
new Vue({
    //...
    components: {
        InputGroup: VueInputGroups
    }
})
```
Then, you should make data-container and field description
```javascript
//...
components: {
    InputGroups
},
data() {
    return {
        formValue: {
            name: "",
            password: "",
        },
        fields: {
            //keys have to be named as well as keys in formValue
            name: {
                type: "text",
                placeholder: "Enter your name"
            },
            password: {
                type: "password",
                placeholder: "Enter password"
            }
        }
    } 
}
```
Now, you can make input group in your app
```html
<input-group v-model="formValue"
             :fields="fields">
</input-group>
```
For more information, go to [docs]()