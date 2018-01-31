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


## Dispatchingのセットアップ
- dispatchを設定して、それに応じてactionで振り分ける

```js

const redux = require('redux');
const createStore = redux.createStore;

const initialState = {
  counter: 0
}

// Reducer //ここでアクションに応じて振り分けている
const rootReducer = (state = initialState, action) => {
  if(action.type === 'INC_COUNTER'){
    return {
      ...state,
      counter: state.counter + 1
    };
  }
  if(action.type === 'ADD_COUNTER'){
    return {
      ...state,
      counter: state.counter + action.value
    };
  }

  return state;
};

// Store
const store = createStore(rootReducer);
console.log(store.getState());


//Dispatching Action //ここで設定
store.dispatch({type: 'INC_COUNTER'});
store.dispatch({type: 'ADD_COUNTER', value: 10});
console.log(store.getState());

//Subscription

```


## Adding Subscriptionのセッティング

```js

const redux = require('redux');
const createStore = redux.createStore;

const initialState = {
  counter: 0
}

// Reducer
const rootReducer = (state = initialState, action) => {
  if(action.type === 'INC_COUNTER'){
    return {
      ...state,
      counter: state.counter + 1
    };
  }
  if(action.type === 'ADD_COUNTER'){
    return {
      ...state,
      counter: state.counter + action.value
    };
  }

  return state;
};

// Store
const store = createStore(rootReducer);
console.log(store.getState());



//Subscription subscribeメソッドdispatchする前に行われることに注意！
store.subscribe(() => {
  console.log('[Subscription]', store.getState());
});


//Dispatching Action
store.dispatch({type: 'INC_COUNTER'});
store.dispatch({type: 'ADD_COUNTER', value: 10});
console.log(store.getState());

```


# Connecting React to Redux
- 先ほどの基本的なやり方をベースにReactと紐づけていく
- index.jsを編集
```js
import React from 'react';
import ReactDOM from 'react-dom';
//1
import { createStore } from 'redux';

//3
import reducer from './store/reducer';

import './index.css';
import App from './App';
import registerServiceWorker from './registerServiceWorker';

//2
const store = createStore(reducer);

ReactDOM.render(<App />, document.getElementById('root'));
registerServiceWorker();

```
- src/storeを作成,storeの中にreduer.jsを作成する
```js

const initialState = {
  counter: 0
}

const reducer = (state = initialState, action) => {
  return state;
}


export default reducer;
```

## npmでreduxをインストール
- `npm install --save react-redux`
```js
//index.jsにインポート

import React from 'react';
import ReactDOM from 'react-dom';
import { createStore } from 'redux';
//1
import { Provider } from 'react-redux';
import reducer from './store/reducer';

import './index.css';
import App from './App';
import registerServiceWorker from './registerServiceWorker';

const store = createStore(reducer);

//2 <Provider store={store}>//ここをラップする</Provider>,
ReactDOM.render(<Provider store={store}><App /></Provider>, document.getElementById('root'));
registerServiceWorker();

```

- counter.jsを編集
```js
//1 まずはインポート
import { connect }from 'react-redux';


//省略
return (
    <div>
    //4
        <CounterOutput value={this.props.ctr} />
        <CounterControl label="Increment" clicked={() => this.counterChangedHandler( 'inc' )} />
        <CounterControl label="Decrement" clicked={() => this.counterChangedHandler( 'dec' )}  />
        <CounterControl label="Add 5" clicked={() => this.counterChangedHandler( 'add', 5 )}  />
        <CounterControl label="Subtract 5" clicked={() => this.counterChangedHandler( 'sub', 5 )}  />
    </div>
  );
  }
}

//3 
const mapStateToProps = state => {
  return {
    ctr: state.counter
  };
};

//2 heigh order componentでラッピング
export default connect(mapStateToProps)(Counter);
```
