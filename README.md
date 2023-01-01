# learning-firestore

Cloud Firestoreの学習用プロジェクト。  

![成果物](./.development/img/fruit.webp)  

## 環境情報

| モジュール | バージョン |
| ---- | ---- |
| Windows | 11 Home |
| Firebase CLI | 11.19.0 |

## 説明イロイロ

1. <https://console.firebase.google.com/u/0/?hl=ja>からプロジェクトを作成。
2. 「構築 - Firestore Database」から新規データベースを作成。
3. プロジェクトトップからアプリを作成。
4. アプリページに書かれている設定情報をコピー。
5. JS(TS)コードに貼り付け。

## プロジェクト初期化

以下のコマンドでプロジェクトを初期化。  

```shell
firebase init
```

Cloud Firebaseを選択し、それ以外は適当に選択する。  

また、`yarn add firebase`コマンドでFirebase操作用のnpmモジュールをインストール。  

## コードの説明

```ts
// 必要なモジュールをインポート
import { initializeApp } from 'firebase/app';
import { getFirestore, collection, getDocs, doc, setDoc, deleteDoc } from 'firebase/firestore/lite';

// 設定オブジェクトを定義して
const firebaseConfig = {
  apiKey: "*****"
  authDomain: "*****",
  projectId: "*****",
  storageBucket: "*****",
  messagingSenderId: "*****",
  appId: "*****",
  measurementId: "*****"
};

// 設定情報をもとにデータベース操作オブジェクトを初期化
const app = initializeApp(firebaseConfig);
const db = getFirestore(app);

// その他イロイロ
// -> https://firebase.google.com/docs/firestore?authuser=0&hl=ja
```
