## Reduxの設定 Reducer+storeのセットアップ
- `npm install --save redux`
```js
//node.jsの書き方で雛形を作る
const redux = require('redux');
const createStore = redux.createStore;

const initialState = {
  counter: 0
}

// Reducer
const rootReducer = (state = initialState, action) => {
  return state;
};

// Store
const store = createStore(rootReducer);
console.log(store.getState());


//Dispatching Action



//Subscription

```
- `node xxxx(ここがファイル名)`今回はredux-basics.jsを作ったのでその名前で行う
- `node redux-basics.js`を行うと、initialStateで記述したオブジェクトがlogで表示される
