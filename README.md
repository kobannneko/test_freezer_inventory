# ふりこれ

冷凍庫の在庫を夫婦で共有するためのWebアプリです。

## 公開方法

GitHub Pagesで以下の構成をそのままアップロードします。

- `index.html`
- `manifest.webmanifest`
- `css/style.css`
- `js/app.js`
- `icons/icon-180.png`
- `icons/icon-192.png`
- `icons/icon-512.png`

## Firebase側で必要な設定

1. AuthenticationでGoogleログインを有効化
2. Cloud Firestoreを作成
3. `firestore.rules` の内容をFirestore Rulesへ貼り付けて公開
