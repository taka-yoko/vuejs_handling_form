# vue-cli

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

For detailed explanation on how things work, consult the [docs for vue-loader](http://vuejs.github.io/vue-loader).

# What's this?
Udemyの`Vue JS - The Complete Guide`のSection11`Handling User Input with Forms`をまとめたもの。
https://www.udemy.com/vuejs-2-the-complete-guide

# A Basic <input> Form Binding

`v-model`によるtwo-way binding。

```javascript
export default {
        data() {
            return {
                email: ''
            }
        }
    }
```

```html
<input
  type="text"
  id="email"
  class="form-control"
  v-model="email">
```

```html
<p>Mail: {{ email }}</p>
```

vue.jsでのtwo way bindingはこれだけで可能。

# Grouping Data and Pre-Populating Inputs

関連性の高いデータをまとめる

Userの属性として email, password, ageをひとまとまりにする。
ageのように、値を入れれば初期値として表示される。
もちろんinputに入力を行えば更新される。

```javascript
data() {
    return {
        userData: {
            email: '',
            password: '',
            age: 27
        }
    }
}
```

v-modelでは以下のように指定。

```html
<input
v-model="userData.email">
```

# Modifying User Input with Input Modifiers

Input Modifiers(修飾子)
https://jp.vuejs.org/v2/guide/forms.html#lazy

`.lazy`
inputイベント後ではなく、changeイベント後にデータと
入力を同期する。入力の度ではなくfocusが外れたら同期

`.trim`
入力の前後の空白を自動的にトリミングする

`.number`
ユーザの入力を数値として自動的に型変換したいときに使用

# Binding <textarea> and Saving Line Breaks



# Using Checkboxes and Saving Data in Arrays

# Using Radio Buttons

# Handling Dropdowns with <select> and <option>

# What v-model does and How to Create a Custom Control

# Creating a Custom Control(Input)

# Submitting a Form

# Time to Practice - Forms

