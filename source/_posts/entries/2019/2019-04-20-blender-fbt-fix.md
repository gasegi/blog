---
title: Blender いくつかの作業メモ
date: 2019-04-20 21:25:21
tags:
- blender
- vrchat
- フルボディトラッキング
---

## はじめに

いくつかの作業のメモ

Blender: 2.79b
Cat Blender Tool: 0.12.2

<!-- toc -->

## 読み込んだアバターのフルボディトラッキング

あらかじめ下の[Import Model]ボタンを押してインポートしています

![blender0.png](./blender0.png)

以下の画像のように、すべてチェックを入れればとりあえず問題なく動作します

![blender1.png](./blender1.png)

## メッシュを分割する

Model Option の `Separate By [Material]` を使用する

分割されたメッシュを再度結合するには、基本的に Fix Model を使用します  
あるいはアウトライナーで Shift キーを使い結合する対象をすべて選択後、 3Dビューにカーソルを持って行ったあと [Ctrl+J] を押すことでも達成できるかもしれません

## シェイプキーを Unity で扱う際に扱いやすい形に並び変える

![blender2.png](./blender2.png)

この画面で右の▲▼ボタンで直します  
下にA-Zなどで並び替える機能がありますが、表示上のみで記録されないので、手で直す必要があります
