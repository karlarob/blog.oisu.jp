---
title: Loading Spinner In React
date: '2019-11-25'
spoiler: Spinner Hell
---

非同期処理中のいわゆる **Loading Spinner** をアプリでどう制御するかって, ほとんどのフロントエンド開発者が直面するであろう問題にもかかわらず、あまり議論されることがない気がします。

![](spinner.png)

一般的なLoading Spinnerの役割は2つで、1つが時間のかかる処理を行っている最中に、ユーザーからの他のインタラクションをブロックすること。もう1つが、処理が現在正常に進行していて、アプリがフリーズしたりしていないということをユーザーに知らせることです。

Reactで、このspinnerの表示を制御する上で、いくつかの方法が考えられると思います。

## 1. 🙅‍♂️ redux内のstateを使う (それぞれのactionの中で更新、spinnerは画面ごとに配置)

```js
// reducer
const userState = {
    users: [],
    showSpinner: false,
};
const userReducer = (state, action) => {
  switch (action.type) {
    case 'FETCH_USER':
        return { ...state, showSpinner: true };
    case 'FETCH_USER_SUCCESS':
        return { ...state, users: action.payload.users, showSpinner: false };
    case 'FETCH_USER_FAILURE':
        return { ...state, showSpinner: false };
  }
};

// dispatch
const fetchUserEffect = async (dispatch) => {
    const users = await fetchUsers();
    dispatch(fetchUserSuccess(user));
};

// container
const MainScreen = ({ showSpinner }) => (
    <MainScreen>
        <>
        <UserList>
        {showSpinner && <Spinner />}
        </>
    </MainScreen>
);
```

どこかのチュートリアルページなどで見るぶんには、これで問題なく動くように見えます。しかし、実際に本番環境で使ってみると、実はいろいろな落とし穴があって、うまくいきません。

まず、spinnerの表示・非表示が、特定のactionと結びついてしまっています。これは、再利用性の観点などからも、明らかに良いとは言えません。もし `fetchUsers()` を **spinner無しで** 他の場所から呼ぶことになったらどうでしょうか。

さらに、<Spinner /> が各componentレベルの場所に置かれています。これがもしmodalの上に置かれていて、画面全体を覆っていた場合、同じ画面のmodalやalertといった別のcomponentと衝突して、予期しない動きをすることがあります。その上、非同期処理が終わるまで、アプリは別の画面に移動できないことにもなります。

なので、今から `showSpinner()` と `hideSpinner()` をこれらの非同期処理から独立した関数として分離してみましょう。加えて、各componentレベルにspinnerを置くのではなく、 `<App />` レベルの共通spinnerを1つだけ使うようにします。

## ‍2. 🙅‍♂️ redux内のstateを使う (専用のactionで更新、spinnerはアプリ全体で1つだけ配置)

```js
// reducer
const userState = {
    users: [],
    showSpinner: false,
};
const spinnerReducer = (state, action) => {
  switch (action.type) {
    case 'SHOW_SPINNER':
        return { ...state, showSpinner: true };
    case 'HIDE_SPINNER':
        return { ...state, showSpinner: false };
  }
};

// dispatch
const fetchUserEffect = async (dispatch) => {
    dispatch(showSpinner());
    const users = await fetchUsers();
    dispatch(setUser(user));
    dispatch(hideSpinner());
};

// container
const App = ({ showSpinner }) => (
    <App>
        <>
        <MainScreen>
        {showSpinner && <Spinner />}
        </>
    </App>
);
```

今度は `fetchUserEffect` がより一層imperativeになってしまいました 😨 `showSpinner` `hideSpinner` のactionをうっかり追加し忘れたり、2つのactionの間に誰かが修正を加えていった結果、おかしくなってしまう予感しかしません。

そこで今度は、 `fetchUsers()` のレスポンスの `users` をreduxのstore経由で使い、 `fetchFavorite()` というactionを新しく呼ぶことになったと仮定してみましょう。

新しいコードはこんな感じ:

```js
const fetchUserEffect = async (dispatch) => {
    dispatch(showSpinner());
    const users = await fetchUsers();
    dispatch(fetchUserSuccess(users));
};

// whenever we successfully get users, we fetch their favorites
const fetchUserSuccessEffect = async (dispatch) => {
    dispatch(fetchFavorite());
    dispatch(hideSpinner());
};

// fetchFavorite() works independently with redux store
const fetchFavoriteEffect = async (dispatch, store) => {
    dispatch(showSpinner());
    const users = getUsers(store)
    const favorites = await fetchFavorites(users);
    dispatch(fetchFavoriteSuccess(favorite));
};

const fetchFavoriteSuccessEffect = async (dispatch) => {
    dispatch(hideSpinner());
};
```

たくさんの `showSpinner()` `hideSpinner()` でごちゃごちゃしました 😂 あまりよさそうにみえません。さらに悪いことには、 `fetchFavorite(user)` も非同期処理を行っているので、この最中にもspinnerが現れたり消えたりすることになり、非同期処理の間、spinnerがかっこわるくチラついてしまうことになります。

長い間、これらの問題を同時に解決する方法が見つけられなかったのですが...、問題の大部分を解決する、すばらしいライブラリを見つけました！

## 3. 🙆‍♂️ `react-promise-tracker` を使う

[react-promise-tracker](https://github.com/Lemoncode/react-promise-tracker)
https://github.com/Lemoncode/react-promise-tracker

シンプルなpromise trackerです。シンタックスはこんな感じ:

```js
import { trackPromise } from 'react-promise-tracker';

trackPromise(
    fetchUsers(); // You function that returns a promise
);
```

```js
import React, { Component } from 'react';
import { usePromiseTracker } from "react-promise-tracker";

export const App = () => {
    const { promiseInProgress } = usePromiseTracker();
    return (
        <App>
            <>
            <MainScreen>
            {promiseInProgress && <Spinner />}
            </>
        </App>
    )
};
```

単に `Promise` 追うというだけで、この方法のほうがずっとシンプルで良さげに思えました。 `area` をpropsで定義することで複数のspinner用stateを管理できたり、 `delay` optionを使って、チラツキを抑止することもできます。

```js
const { promiseInProgress } = usePromiseTracker({area: 'big-spinner', delay: 500});

trackPromise(
    fetchUsers(),
    {area: 'big-spinner'}
);
```

しかし、複数の非同期処理におけるspinnerの表示をうまく制御する方法は、依然として難しいです。そのためには、何か別のライブラリの力を借りたり、複数のpromiseを束ねるカスタム関数みたいなものを作る必要があるのかなと思います。

別のアイデアや、よりよい方法がもしあれば、教えていただけると幸いです！
