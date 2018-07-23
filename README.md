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

以下は動作しない

```html
<textarea>{{ test }}</textarea>
```

以下のようにv-modelを指定すれば、default値が設定される。

```html
<textarea
  id="message"
  rows="5"
  class="form-control"
  v-model="message"></textarea>
```

```javascript
data() {
    return {
        userData: {
            email: '',
            password: '',
            age: 27
        },
        message: "A default message"
    }
}
```

## textareaに入力した改行を保った状態で表示したい場合

出力htmlのstyleを以下のようにする。

```html
 <p style="white-space: pre">Message: {{ message }} </p>
 ```

# Using Checkboxes and Saving Data in Arrays

checkboxで、チェックされた値を配列に格納する方法

sendMail（配列）にcheckboxでチェックされた値を格納。

```javascript
data() {
    return {
        sendMail: []
    }
}
```

checkbox側では、`v-model`に同じ名前を指定する。ここでは`sendMail`。
そうすると、同じグループとみなされ、`sendMail`の配列に、チェックされたcheckboxのvalueが格納されるようになる。

```html
<div class="form-group">
    <label for="sendmail">
        <input
                type="checkbox"
                id="sendmail"
                value="SendMail"
                v-model="sendMail"> Send Mail
    </label>
    <label for="sendInfomail">
        <input
                type="checkbox"
                id="sendInfomail"
                value="SendInfoMail"
                v-model="sendMail"> Send Infomail
    </label>
</div>
```

配列は`v-for`でループ処理して表示

```html
<ul>
    <li v-for="item in sendMail">{{ item }}</li>
</ul>
```

# Using Radio Buttons

radio buttonはcheckboxと違い、選択できる値は1つだけ。

dataにradio buttonの値を格納するpropertyを追加。defaultを`Male`とする。

```javascript
data() {
    return {
        gender: 'Male'
    }
}
```

同じグループにしたいradio buttonに同じ`v-model`名を設定

```html
<div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6col-md-offset-3 form-group">
    <label for="male">
        <input
                type="radio"
                id="male"
                value="Male"
                v-model="gender"> Male
    </label>
    <label for="female">
        <input
                type="radio"
                id="female"
                value="Female"
                v-model="gender"> Female
    </label>
</div>
```

表示はdataのpropertyを指定すればよい

```html
<p>Gender: {{ gender }}</p>
```

# Handling Dropdowns with select and option

動的にselectタグのoptionを設定する。

dataにoptionタグに設定する値を配列で設定

```javascript
priorities: ['Hight', 'Medium', 'Low']
```

optionタグ内で`v-for`を使って設定する。

```html
<option v-for="priority in priorities">{{ priority }}</option>
```

defaultで選択される値を設定
`:selected`が`true`になる場合がdefault値となる。

```html
<option v-for="priority in priorities" :selected="priority == 'Medium'">{{ priority }}</option>
```

データバインディングを行うには、selectタグに`v-model`を指定する
`selectedPriority`にdefault値を設定すると、それが選択されたdefault値となる。
なので上記の`:selected="priority == 'Medium'"`の部分は無効となる。

```javascript
selectedPriority: 'High'
```

```html
<select
  v-model="selectedPriority">
```

# What v-model does and How to Create a Custom Control

`v-model`が裏側で行っていること

```html
<input v-model="userData.email">
```

上記は、以下と同じ処理を行っている。
`:value`でデータバインディングを行い、
`@input`イベント発生時に、inputタグに入力されている値を
`userData.email`に格納する。

```html
<input
  :value="userData.email"
  @input="userData.email = $event.target.value">
```

これを踏まえてカスタマイズしたinputタグを作成する。

# Creating a Custom Control(Input)

トグル可能なSwitchコンポーネントを作成

Switch.vue
```javascript
<template>
  <div>
    <div
        id="on"
        @click="swithed(true)"
        :class="{active: value}">On</div>
    <div
        id="off"
        @click="swithed(false)"
        :class="{active: !value}">Off</div>
  </div>
</template>

<script>
  export default {
    props: ['value'],
    methods: {
      swithed(isOn) {
        this.$emit('input', isOn)
      }
    }
  }
</script>
```

App.vue側
```html
<appSwitch v-model="dataSwitch"></appSwitch>
```

前のchapterの通り、`v-model`は以下の設定と同じ
(コンポーネントにした場合は、@inputイベントが受ける引数の値は、`$event`になる)

詳細は以下
https://jp.vuejs.org/v2/guide/components.html#%E3%82%B3%E3%83%B3%E3%83%9D%E3%83%BC%E3%83%8D%E3%83%B3%E3%83%88%E3%81%A7-v-model-%E3%82%92%E4%BD%BF%E3%81%86

```html
<appSwitch
  :value="dataSwitch"
  @input="dataSwitch = $event">
</appSwitch>
```

# Submitting a Form

ここではsubmitボタンを押したイベントを拾い、
`Your Data`に表示するまでを行う。

submit用のボタンに`@click`イベントを設定。
今回はサーバーへデータを送らないので`prevent`モディファイアをつける。

```html
<button
  class="btn btn-primary"
  @click.prevent="submitted">Submit!
</button>
```

clickイベントが発生したら、以下のメソッドで受ける。

```javascript
methods: {
    this.submitted() {
        this.isSubmitted = true;
    }
},
```
