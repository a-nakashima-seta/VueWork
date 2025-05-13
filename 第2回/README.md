# 🎓 第2回 Vue 勉強会

![Vue アイキャッチ](../vue.png "Vue アイキャッチ")

[▶ Vue Play Ground](https://play.vuejs.org/)


## 📘 今回取り扱う内容

* Vueの状態管理の選択肢について
* `v-model` の理解を深める
* Nuxtの概要

---

# 🧠 Vueの状態管理について

### 💡 状態管理とは？

> アプリ内で変化するデータ（状態）を、**整理して扱いやすく保つ仕組み**。
> 複数コンポーネント間で状態を共有しやすくし、**コードの見通しを良くする**目的で使います。

---

### 📝 状態管理が必要になる例

| ユースケース         | 状態の内容         | 詳細                   |
| -------------- | ------------- | -------------------- |
| 🛒 ショッピングカート   | カートに入っている商品情報 | 複数のページでカートの中身を共有したい  |
| 🔐 タブ間での認証同期   | ログイン状態        | 他タブでもログアウト状態を反映させたい  |
| 📝 フォーム内容の一時保存 | 入力中のデータ       | ページ遷移後も入力内容を保持しておきたい |

---

## 🔗 Vueにおける状態管理の3つの手法

1. **Ref × Props / Emit**
2. **Provide / Inject**
3. **Pinia（公式ストア）**

---

### ① Ref / Props / Emit

[▶ サンプルコードを見る](https://github.com/a-nakashima-seta/VueWork/tree/main/%E7%AC%AC2%E5%9B%9E/codes/src/components/PropsEmit)

* 親→子：`props` でデータを渡す
* 子→親：`emit` でイベントを発火し、変更を伝える

#### ⚠️ 注意点

* `props` は **読み取り専用**
* 階層が深くなると **propsのバケツリレー** が発生しやすい

---

### ② Provide / Inject

[▶ サンプルコードを見る](https://github.com/a-nakashima-seta/VueWork/tree/main/%E7%AC%AC2%E5%9B%9E/codes/src/components/ProvideInject)

* 深い階層の子孫コンポーネントへ直接データを渡す手段
* 「バケツリレー」の回避に有効

#### ⚠️ 注意点

* `inject` で受け取る値は基本 **非リアクティブ**
* **双方向バインディングには非推奨**
* **親子の依存関係が見えにくくなる**ため、設計に注意

---

### ③ Pinia（Vue公式状態管理ライブラリ）

[▶ Pinia公式🍍](https://pinia.vuejs.org/)
[▶ サンプルコード](https://github.com/a-nakashima-seta/VueWork/tree/main/%E7%AC%AC2%E5%9B%9E/codes/src/components/ProvideInject)

#### ✅ ユースケース

* ユーザー情報、認証状態などの**グローバル状態**
* コンポーネント間の**リアクティブな共有**
* 中〜大規模アプリでの**状態の一元管理**

#### 🚀 導入方法

1. **インストール**

   ```bash
   npm install pinia
   ```

2. **main.ts に登録**

   ```ts
   import { createApp } from 'vue'
   import { createPinia } from 'pinia'
   import App from './App.vue'

   const app = createApp(App)
   app.use(createPinia())
   app.mount('#app')
   ```

3. **ストア定義**

   ```ts
   import { defineStore } from 'pinia'
   import { ref } from 'vue'

   export const useCounterStore = defineStore('counter', () => {
     const count = ref(0)
     const increment = () => count.value++
     return { count, increment }
   })
   ```

4. **ストア利用**

   ```vue
   <script setup>
   import { useCounterStore } from '@/stores/counter'
   const counter = useCounterStore()
   </script>

   <template>
     <button @click="counter.increment">
       カウント: {{ counter.count }}
     </button>
   </template>
   ```

---

### 💡 Tips：Pinia で分割代入するには？

Piniaの状態やアクションを分割代入したい場合、`storeToRefs()` を使います。

```ts
// composable.ts
import { useCounterStore } from '@/stores/counter'
import { storeToRefs } from 'pinia'

export default () => {
  const store = useCounterStore()
  return {
    ...store,
    ...storeToRefs(store)
  }
}
```

```ts
// 呼び出し元
const { count, increment } = useCounterStore()
```

---

### ❓ なぜ Provide/Inject ではなく Pinia？

| 比較観点    | Provide / Inject      | Pinia              |
| ------- | --------------------- | ------------------ |
| スコープ    | コンポーネント階層内            | グローバル              |
| リアクティブ性 | 明示的に `ref` などを使う必要がある | 自動でリアクティブ          |
| 管理のしやすさ | 設計次第では依存関係が不明瞭になりがち   | 明示的で一元化されたストア構成が可能 |

---

## 🧭 状態管理のまとめ

アプリにとって状態管理の手法は **1つの正解があるわけではない**。
適切な手法を選ぶことが大事！

| 状況                         | 推奨手法                 |
| -------------------------- | -------------------- |
| 単純な親子間のデータ受け渡し             | `Ref / Props / Emit` |
| 深い階層に渡すが、バケツリレーは避けたい       | `Provide / Inject`   |
| グローバルに共通する状態（認証情報、ユーザー情報等） | `Pinia`              |

---

# 📝 v-model の理解を深める

## 🎯 `v-model` とは？

`v-model` は、Vue の「双方向バインディング」を簡単に実現する仕組み。
Vue開発の「快適さ・宣言的な書き方」の中心となる機能です。

---

## 🔁 複数の v-model を 1 コンポーネントに指定可能

Vue 3 では、**名前付き v-model** が使えます。

### 呼び出し側

```vue
<MyForm v-model:title="postTitle" v-model:content="postContent" />
```

### コンポーネント側（従来）

```ts
<script setup>
defineProps(['title', 'content'])
defineEmits(['update:title', 'update:content'])
</script>

<template>
  <input v-model="title" />
  <textarea v-model="content" />
</template>
```

---

## 🚀 `defineModel()` マクロの活用

Vue 3.3 以降では、`defineModel()` により、**props / emits を自動的に統合し、v-modelを簡潔に扱えます。**

### 使用例

```ts
<script setup>
const title = defineModel('title')
const content = defineModel('content')
</script>

<template>
  <input v-model="title" />
  <textarea v-model="content" />
</template>
```

---

### ✅ defineModel() のメリット

| メリット                   | 説明                                    |
| ---------------------- | ------------------------------------- |
| ✅ シンプルな記述              | `props`/`emits` の定義不要                 |
| ✅ 型推論対応                | `defineModel<Type>()` で型補完が効く         |
| ✅ 複数のモデル対応             | 名前付き `v-model` にもスムーズに対応              |
| ✅ Composition APIとの統一性 | `ref` のように使え、Compositionパターンと自然に統合できる |

---

### 🔄 従来 vs defineModel 比較

#### 🧱 従来

```ts
<script setup>
const props = defineProps(['title', 'content'])
const emit = defineEmits(['update:title',
```


'update\:content'])

const updateTitle = (value) => emit('update\:title', value) </script>

````

#### ✅ defineModel 使用後

```ts
<script setup>
const title = defineModel('title')
</script>
````

➡️ **コード量が半減し、型安全性と可読性が向上！**

---

### 💡 活用ガイド

| 状況                        | 推奨手法            |
| ------------------------- | --------------- |
| 単一の双方向バインディング             | 通常の `v-model`   |
| 複数の状態を扱う必要がある場合           | 名前付き `v-model`  |
| Composition API 中心で開発している | `defineModel()` |


### 🎯 使うべきシーン

* 複数のフォーム項目を扱う場合
* 型安全性を求めるチーム開発
* Props/Emit を毎回書くのが面倒な場合
* Composition API 派の開発スタイル
