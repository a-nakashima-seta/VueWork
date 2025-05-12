# 第２回　 Vue 勉強会

![Vue アイキャッチ](../vue.png "Vue アイキャッチ")

[▶ Vue Play Ground](https://play.vuejs.org/#eNp9kUFLwzAUx7/KM5cqzBXR0+gGKgP1oKKCl1xG99ZlpklIXuag9Lv7krK5w9it7//7v/SXthP3zo23EcVEVKH2yhEEpOhm0qjWWU/QgccV9LDytoWCq4U00tTWBII2NDBN/LJ4Qq0tfFuvlxfFlTRVORzHB/FA2Dq9IOQJoFrfzLouL/d9VfKUU2VcJNhet3aJeioFcymgZFiVR/tiJCjw61eqGW+CNWzepX0pats6pdG/OVKsJ8UEMklswXa/LzkjH3G0z+s11j8n8k3YpUyKd48B/RalODBa+AZpwPPPV9zx8wGyfdTcPgM/MFgdk+NQe4hmydpHvWz7nL+/Ms1XmO8ITdhfKommZp/7UvA/eTxz9X/d2/Fd3pOmF/0fEx+nNQ==)

## 今回取り扱う内容
・Vueの状態管理の選択肢について<br>
・Vueのv-modelを深掘る<br>
・Nuxtの概要<br>

<br>

### Vueの状態管理について
#### 【そもそも状態管理って何？】<br>
    「アプリの中で変化するデータの値を、ちゃんと整理して覚えておく仕組み」のことです。
    データを整理しておくことで、「複数コンポーネントで同じデータを共有する」ことが容易に実現できるようになり、コードの見晴らしもよくなります。

#### 【状態管理が必要になる場面の例】<br>

| ユースケース               | 状態の内容                     | 詳細                              |
|----------------------------|-------------------------------|-----------------------------------------------|
| **ショッピングカート**        | カートに入っている商品情報             | 複数のページ（商品一覧、詳細、カート確認）でカートの中身を共有したい    |
| **別タブ間の認証状態同期**       | 複数タブでログイン状態が共有されている     | ログアウトしたら他のタブでもログアウトさせたい               |
| **フォーム入力内容の一時保存**        | 入力途中のフォームデータ              | ページ遷移後も入力内容を記憶し残しておきたい場合など                  |

<br>

### 【Vueにおけるコンポーネント間の状態管理】<br>
    1. Props/Emit
    2. Provide/Inject
    3. Pinia
    4. VueUse


### 1. Ref / Reactive × Props / Emit

Vueアプリケーションにおける最も基本的な状態管理の方法が、「**Props ダウン、Emit アップ**」という考え方です。<br>
親コンポーネントから子コンポーネントへは **`props`** を使ってデータを渡し、子から親へは **`emit` を使ってイベントを発火し、データの変更を通知**します。

#### ⚠️ 注意点

* `props` は読み取り専用のため、子コンポーネントから直接変更することはできません。
* コンポーネント階層が深くなると、`props` を何層にも渡していく「バケツリレー」状態になりやすく、データの流れが複雑になる点に注意が必要です。


<br>

### 2. Provide / Inject

`provide` と `inject` は、**親コンポーネントから離れた子孫コンポーネントへ状態を共有する**ための仕組みです。<br>
深い階層のコンポーネントに直接データを渡すことができるため、「propsのバケツリレー問題」を解消する手段として有効です。

親コンポーネント側で `provide` によってデータを提供し、子孫コンポーネント側で `inject` を使ってそのデータを受け取ります。

#### ⚠️ 注意点

* `inject` で受け取ったデータは基本的にリアクティブではないため、**状態の変化に応じて再レンダリングさせたい場合は、`ref` や `reactive` を使って渡す必要があります**。
* 双方向バインディングには対応していないため、**子孫コンポーネントから状態を直接変更することは推奨されません**。
* 親と子孫の**依存関係が不透明になりやすく、可読性が低下する**可能性があるため、設計には注意が必要です。



#### 【双方向バインディング】<br>

- ほかのフレームワークとは違い相互にデータをやりとりできる双方向のデータフロー<br>
  → 設計の自由度が高く素直に設計できる反面、プロジェクトが大きくなった際にデータの流れが複雑化してしまうというデメリット？？？<br>
  → データの複雑性を解消できるライブラリ(Pinia、Vuex3、useState)もすでに標準化している。<br>
  ネット上によくある「大規模開発には向かない」という意見は信用しなくてよいという個人的な見解

### Vue できること

- SPA(シングルページアプリケーション)の実装<br>

      SPAとは、単一のページで表示する内容の切り替えを行うことで、サイト内の挙動をリッチに見せることができ、
      UX の向上につなげることができるという技術です。

初期レンダリングに時間がかかるというデメリット<br>
→SSR、SSG、ISG の選定によって解消可能

【Vue が採用されている例】

- https://jp-news.mercari.com/10th-anniversary/

- https://whitemouth.jp/

- https://nakameguro-shogakuji.or.jp/

<br>

# Vue の基礎を学ぶ

ここからは Vue の基本的な構文やシステムへの理解を深めていきましょう。

## 「SFC」単一ファイルコンポーネント

Vue ではコンポーネントと呼ばれる.vue ファイルを複数組み合わせてアプリケーションを完成させていきます。<br>
そしてその.vue は単一ファイルコンポーネント(通称 SFC)といいます。<br>
この SFC が Vue をたらしめている要素の一つであり、直感的にコードを書くことができるシステムになっています。
<br>

### 2 パターンの記述スタイル

    CompositionAPI

```html
<script setup>
  import { ref, onMounted } from "vue";

  // リアクティブな状態
  const データ = ref(0);

  // 状態を変更し、更新をトリガーする関数。
  function 関数() {}
</script>

<template>
  <!-- HTMLの記述 -->
</template>

<style scoped>
  /* CSSの記述 */
</style>
```

<br>

    OptionAPI

```html
<script setup>
  export default {
    data() {
      return {
        データ: 値,
      };
    },
    methods: {
      関数() {},
    },
  };
</script>

<template>
  <!-- HTMLの記述 -->
</template>

<style scoped>
  /* CSSの記述 */
</style>
```

<br>

## コンポーネントについて

Vue.js のコンポーネントは、再利用可能なユーザーインターフェースの部品です。<br>
script タグでインポートした部品を子孫コンポーネントとして自由に呼び出すことができます。

```html
<!-- ▼ 親コンポーネント ▼ -->
<template>
  <!-- ▼ 子コンポーネントの埋め込み ▼ -->
  <childComp />
</template>

<script setup>
  // ▼ 子コンポーネントのインポート ▼
  import childComp from "./Comp.vue";
</script>

<style scoped></style>
```

<br>

## Vue の持つ機能

Vue に標準で備わっている API の機能について見ていきましょう。

### `ref` と `reactive`

    Vue 3では、 `ref` と `reactive` を使用してリアクティブなデータを作成します。
    これらはComposition APIの重要な部分であり、それぞれ異なるデータ型や用途に適しています。

#### `ref` は、プリミティブなデータ型（文字列、数値、ブール値など）や単一の値をリアクティブにするための関数です。`ref` を使うことで、値が変更されると自動的にコンポーネントが再レンダリングされます。

```html
<template>
  <div>
    <p>Count: {{ count }}</p>
    <button @click="increment">Increment</button>
  </div>
</template>

<script setup>
  import { ref } from "vue";

  // refを使用してリアクティブなデータを定義
  const count = ref(0);

  // メソッドを定義
  const increment = () => {
    count.value++; // count.value を更新
  };
</script>
```

#### `reactive` は、オブジェクトや配列など複雑なデータ型をリアクティブにするための関数です。これを使うことで、ネストされたプロパティもリアクティブになります。

```html
<template>
  <div>
    <p>User Name: {{ user.name }}</p>
    <button @click="updateName">Update Name</button>
  </div>
</template>

<script setup>
  import { reactive } from "vue";

  // reactiveを使用してリアクティブなオブジェクトを定義
  const user = reactive({
    name: "John Doe",
  });

  // メソッドを定義
  const updateName = () => {
    user.name = "Jane Doe"; // user.name を更新
  };
</script>
```

<br>

### `computed`

    算出プロパティと呼ばれ、リアクティブなデータの変更時に自動的に再計算を実行することができる機能です。
    パフォーマンスの最適化やロジックの複雑化を防ぐことができます。

```html
<template>
  <div>
    <p>Count: {{ count }}</p>
    <p>Double Count: {{ doubleCount }}</p>
    <button @click="increment">Increment</button>
  </div>
</template>

<script setup>
  import { ref, computed } from "vue";

  // refを使用してリアクティブなデータを定義
  const count = ref(0);

  // computedを使用して算出プロパティを定義
  const doubleCount = computed(() => count.value * 2);

  // メソッドを定義
  const increment = () => {
    count.value++; // count.value を更新
  };
</script>
```

- ポイント<br>
  メソッドで定義した値を呼び出す場合と何が違うの？という疑問をもつ方がいるでしょう。<br>
  メソッド呼び出しの場合は画面の再描画の際に常に関数を実行します。<br>
  一方で conputed で設定した結果は対象のリアクティブデータの更新時にのみ計算が実行され、再描画のたびに計算されません。要するに、computed の値はリアクティブデータに基づきキャッシュされるということです。
  <br>

### `watch`

    特定のリアクティブなデータや計算されたプロパティの変更を監視し、その変更のたびにコールバック関数を実行します。
    副作用を引き起こす処理（APIリクエスト、データの保存など）に役立ちます。

```html
<template>
  <div>
    <p>Count: {{ count }}</p>
    <button @click="increment">Increment</button>
  </div>
</template>

<script setup>
  import { ref, watch } from "vue";

  // refを使用してリアクティブなデータを定義
  const count = ref(0);

  // watchを使用してデータの変更を監視
  watch(count, (newValue, oldValue) => {
    console.log(`Count changed from ${oldValue} to ${newValue}`);
  });

  // メソッドを定義
  const increment = () => {
    count.value++; // count.value を更新
  };
</script>
```

<br>

### Vue のディレクティブ

Vue.js では、テンプレート内で様々なディレクティブを使用して、動的な UI の構築やデータのバインディングが可能です。主要なディレクティブを紹介します。

### `v-on`

    イベントリスナーを登録するためのディレクティブです。ユーザーの操作（クリック、キーボード入力など）に応じて関数を呼び出すことができます。
    イベントハンドラをコンポーネントのメソッドや関数にバインドします。ショートハンドとして `@click` のように書くこともできます。

```html
<template>
  <button v-on:click="handleClick">Click me</button>
</template>

<script setup>
  import { ref } from "vue";

  const handleClick = () => {
    console.log("Button clicked!");
  };
</script>
```

### `v-for`

    リストをレンダリングするためのディレクティブです。配列やオブジェクトのプロパティを繰り返し表示する際に使用します。
    配列やオブジェクトを反復処理してUIに表示します。`key` 属性は、リストアイテムを一意に識別するために使用します。

```html
<template>
  <ul>
    <li v-for="item in items" :key="item.id">{{ item.name }}</li>
  </ul>
</template>

<script setup>
  import { ref } from "vue";

  const items = ref([
    { id: 1, name: "Apple" },
    { id: 2, name: "Banana" },
    { id: 3, name: "Cherry" },
  ]);
</script>
```

<br>

### `v-model`

    フォーム入力要素とデータを双方向にバインディングするためのディレクティブです。入力フィールドの値とデータが自動的に同期されます。
    入力フィールドの値をリアクティブに管理します。`v-model` は、`input`、`textarea`、`select` などのフォーム要素で使用できます。

```html
<template>
  <input v-model="message" placeholder="Type something" />
  <p>{{ message }}</p>
</template>

<script setup>
  import { ref } from "vue";

  const message = ref("");
</script>
```

<br>

### `v-bind`

    HTML属性にデータをバインディングするためのディレクティブです。データプロパティをHTML属性に動的に結びつけます。
    HTMLの属性（例えば、`src`、`href`、`class` など）にデータを動的にバインディングします。
    ショートハンドとして `:` を使用することもできます。

```html
<script setup>
  import { ref } from "vue";

  const color = ref("green");

  function toggleColor() {
    color.value = color.value === "green" ? "blue" : "green";
  }
</script>

<template>
  <p :style="{ color }" @click="toggleColor">クリックで色がスイッチする</p>
</template>
```

<br>

### `v-if` / `v-else`

    条件に基づいて要素の表示・非表示を制御するためのディレクティブです。
    条件に応じて異なる要素を表示します。
    `v-if` は条件が `true` のときに要素をレンダリングし、`v-else` は `v-if` が `false` のときにレンダリングされます。

```html
<template>
  <p v-if="isVisible">This is visible</p>
  <p v-else>This is hidden</p>
  <button @click="toggleVisibility">Toggle Visibility</button>
</template>

<script setup>
  import { ref } from "vue";

  const isVisible = ref(true);

  const toggleVisibility = () => {
    isVisible.value = !isVisible.value;
  };
</script>
```

<br>

### `v-show`

    条件に基づいて要素の表示・非表示を制御しますが、`v-if` とは異なり、要素は常にDOM内に存在しますが、
    CSSの `display` プロパティで表示・非表示が切り替わります。
    要素を常にDOM内に保持しつつ、CSSの `display` プロパティで表示・非表示を制御します。`v-show` は、頻繁に表示を切り替える場合に適しています。

```html
<template>
  <p v-show="isVisible">This is visible</p>
  <button @click="toggleVisibility">Toggle Visibility</button>
</template>

<script setup>
  import { ref } from "vue";

  const isVisible = ref(true);

  const toggleVisibility = () => {
    isVisible.value = !isVisible.value;
  };
</script>
```

<br>

## コンポーネント間のコミュニケーション

コンポーネントベースのシステムでアプリケーションを構築していくためには、コンポーネント間のデータ流れを理解しておかなくてはなりません。<br>
独自の機能を活用することで、動的なデータをコンポーネント間で共有し効率的に実装することができます。

### `props`

    propsは親コンポーネントから子孫コンポーネントにデータを渡す仕組みです。

#### 使用の流れ

1. 親コンポーネントでデータを準備し、子コンポーネントに props として渡します。
2. 子コンポーネント内で受け取るデータの型を定義し、その型を利用して defineProps 関数を実行することで props を利用できるようになります。<br>

```html
<!-- 親コンポーネント -->
<script setup>
  import ChildComponent from "./Comp.vue";
  import { ref } from "vue";

  // 親コンポーネントでデータを準備
  const parentMessage = ref("Hello from Parent!");
  const itemCount = ref(10);

  // データを更新するメソッドを定義
  const updateValues = () => {
    parentMessage.value = "Updated Message!";
    itemCount.value = 20;
  };
</script>

<template>
  <div>
    <!--準備したデータを、子コンポーネントに渡す -->
    <ChildComponent :message="parentMessage" :count="itemCount" />

    <!-- ボタンをクリックして親コンポーネントのデータを更新する -->
    <button @click="updateValues">Update Values</button>
  </div>
</template>
```

```html
<!-- 子孫コンポーネント.vue -->
<script setup>
  import { defineProps, watch } from "vue";

  // 子コンポーネントで props を定義
  const props = defineProps({
    message: {
      type: String,
      required: true,
      default: "Default message",
    },
    count: {
      type: Number,
      default: 0,
    },
  });

  // props の変更を監視
  watch(
    () => props.count,
    (newValue, oldValue) => {
      console.log(`Count changed from ${oldValue} to ${newValue}`);
    }
  );
</script>

<template>
  <div>
    <!-- 子コンポーネントで受け取った props を表示 -->
    <p>{{ message }}</p>
    <p>Count: {{ count }}</p>
  </div>
</template>
```

#### Props 利用の注意点

- props の値を直接更新できない<br>
  子コンポーネント内の受け取った props のデータは readonly(読み取り専用)となっているため、次のような直接的な変更の処理を記述することはできません。

```html
func(){
<!-- NG! propsの値を直接参照している -->
props.points++;× }
```

子コンポーネント内で props の値を更新する必要がある場合には、新たにインスタンスとなる変数を作成し、その値を更新すればよいのです。

```html
<!-- props内の値のインスタンスを作成する -->
const localPoints = ref(props.points)

<!-- 作成したインスタンスを更新する -->
func(){ localPoints.value++; }
```

<br>

### `emit`

`props`では親から子へデータを渡すことできますが、逆に子から親へデータを渡すことはできません。<br>
そこで、親コンポーネントでデータとそのデータを更新するメソッドを用意しておき、子コンポーネント側でそのメソッドを実行するようにお願いすることでデータを更新し、子から親へのデータの変更を実現することができます。<br>
その仕組みが`emit`です。

#### 使用の流れ

1. **親コンポーネントへの記述**:

- 発火させる処理メソッドの記述
- v-on ディレクティブで子コンポーネントからのイベントを受け取り、処理メソッドと紐づける

2. **子コンポーネントへの記述**:

- defineEmits()の記述、emit でイベントを発火させるメソッドを定義
- emit の実行。例だと@click="sendUpdate"で実行される。

```html
<!-- 親コンポーネント -->
<script setup>
  import ChildComponent from "./Comp.vue";
  import { ref } from "vue";

  // 親コンポーネントでのデータ
  const updatedMessage = ref("Initial Message");

  // 子コンポーネントからのイベントをハンドリングするメソッド
  const handleUpdate = (newMessage) => {
    updatedMessage.value = newMessage;
  };
</script>

<template>
  <div>
    <!-- 子コンポーネントからのイベントをリスン -->
    <ChildComponent @update="handleUpdate" />

    <p>Updated message: {{ updatedMessage }}</p>
  </div>
</template>
```

```html
<!-- 子コンポーネント -->
<script setup>
  import { defineEmits } from "vue";

  // イベントを発火させるための定義
  const emit = defineEmits(["update"]);

  // イベントを発火させるメソッド
  const sendUpdate = () => {
    emit("update", "Message from Child");
  };
</script>

<template>
  <div>
    <button @click="sendUpdate">Send Update</button>
  </div>
</template>
```

#### emit の注意点

- **イベント名の一貫性**: 子コンポーネントが発火させるイベント名は、親コンポーネントでリスンする際に指定するイベント名と一致している必要があります。
- **イベントパラメータ**: `emit`で発火させるイベントには、任意のデータをパラメータとして渡すことができます。これにより、親コンポーネントに必要な情報を提供することができます。

このように、`emit`を利用することで、子コンポーネントから親コンポーネントに対して情報や通知を効果的に送信することができます。

<br>

## Vue × TypeScript

### 1. 基本的な記述の違い

#### TypeScript を使用しない場合

```html
<script setup>
  import { ref } from "vue";

  // ref を使用してリアクティブなデータを定義
  const message = ref("Hello, Vue!");

  // メソッドを定義
  const changeMessage = () => {
    message.value = "Hello, TypeScript!";
  };
</script>
<template>
  <div>
    <p>Message: {{ message }}</p>
    <button @click="changeMessage">Change Message</button>
  </div>
</template>
```

#### TypeScript を使用する場合

```html
<script lang="ts" setup>
  import { ref } from "vue";

  // 型を指定してリアクティブなデータを定義
  const message = ref<string>("Hello, Vue!");

  // メソッドを定義
  const changeMessage = (): void => {
    message.value = "Hello, TypeScript!";
  };
</script>

<template>
  <div>
    <p>Message: {{ message }}</p>
    <button @click="changeMessage">Change Message</button>
  </div>
</template>
```

### 2. Props と Emits

#### TypeScript を使用する場合の Props の記述

```html
<!-- 親コンポーネント -->
<script setup lang="ts">
  import { ref } from "vue";
  import Comp from "./Comp.vue";

  const itemId = ref<number>(1);
  const itemName = ref<string>("John");
</script>

<template>
  <main>
    <Comp :id="itemId" :name="itemName" />
  </main>
</template>
```

```html
<!-- 子コンポーネント -->
<script setup lang="ts">
  interface Props = {
    id: number;
    name: string;
  };

  const props = defineProps<Props>();
</script>

<template>
  id: {{ props.id }}<br />
  name: {{ props.name }}
</template>
```
#### 実装の流れ
親コンポーネント
- 渡したいデータをscript内に定義
- 子コンポーネントの属性で渡す

子コンポーネント
- 受け取るデータの型を定義(typeまたはInterfaceとして受け取ることで簡潔に記述できる)
- 定義した型を利用して defineProps 関数を実行


<br>

#### TypeScript を使用する場合の Emits の記述

```html
<!-- 親コンポーネント -->
<script setup lang="ts">
  import ChildComponent from "./Comp.vue";
  import { ref } from "vue";

  // 親コンポーネントでのデータ
  const updatedMessage = ref("Initial Message");

  // 子コンポーネントからのイベントをハンドリングするメソッド
  const handleUpdate = (newMessage: string): void => {
    updatedMessage.value = newMessage;
  };
</script>

<template>
  <div>
    <!-- 子コンポーネントからのイベントをリスン -->
    <ChildComponent @update="handleUpdate" />

    <p>Updated message: {{ updatedMessage }}</p>
  </div>
</template>
```

```html
<!-- 子コンポーネント -->
<script setup lang="ts">
  import { defineEmits } from "vue";

  interface Emits{
  // ラベル付きタプルで型の定義
    update: [newMessage: string];
  }

  const emit = defineEmits<Emits>();
  const sendUpdate = () :void => {
    emit("update", "Message from Child");
  };
</script>

<template>
  <div>
    <button @click="sendUpdate">Send Update</button>
  </div>
</template>
```

#### 実装の流れ
親コンポーネント
- 発火するメソッドの用意
- v-onディレクティブの用意

子コンポーネント
- Emits型の定義、
- defineEmit()の定義
- ハンドラ関数とディレクティブの追加

`defineProps` や `defineEmits` に対して型を指定することでコンポーネント間のデータやイベントのやり取りを型安全に管理でき見晴らしの良いコードを書くことにもつながります。
