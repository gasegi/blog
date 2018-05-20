---
title: プロダクション環境で起きたエラーの収集とハンドリング
date: 2018-05-21 00:17:53
tags:
- node.js
- webpack
- VSCode
---

## webpack で処理した Node.js アプリケーションのエラーを取得する

Visual Studio Code (VSCode) で Node.js のプログラムデバッグをする時、 webpack で処理してしまうと  
生成後のコードでブレークする。 JavaScript 特有かつ頻出の問題。

これらに対処するために、 Source Map (ソースマップ) という仕組みがある。  
生成するためのライブラリ (https://github.com/mozilla/source-map) が Mozilla から提供されている。  
ただし、ソースマップを利用するだけならこのライブラリの存在を知る必要はない。

この問題に対応するには以下の手順を踏む。

1. webpack で `devtool` (https://webpack.js.org/configuration/devtool/) の設定を行う。
1. VSCodeの `デバッグ起動` (https://code.visualstudio.com/docs/editor/debugging) の設定を行う。
1. デバッグ起動を行う。

### 個別手順

#### 1. webpack で [devtool](https://webpack.js.org/configuration/devtool/) の設定を行う。

以下のページを元に、ソースマップが出力する。

https://webpack.js.org/configuration/devtool/

設定の種類が多いが、開発 (https://webpack.js.org/configuration/devtool/#development) や  
製造 (https://webpack.js.org/configuration/devtool/#production) の通りに設定すれば問題はない。

要約すると以下のようにすればよい。ページにも記載があるが、 `devtool` を使用する場合、  
`SourceMapDevToolPlugin` / `EvalSourceMapDevToolPlugin` は必要ない。

|ケース|設定|
|--|--|
|開発時|`eval-source-map`, `eval`, `inline-source-map`|
|製造時|`hidden-source-map`|

基本的に eval から始まる設定で良いようだが、手元の環境では `zone.js` を使用しているからかわからないが、  
`eval-source-map` でビルドした結果が実行できなかったので `source-map` を使用した。

#### 2. VSCodeの [デバッグ起動](https://code.visualstudio.com/docs/editor/debugging) の設定を行う。

VSCodeのデバッグ起動に関する基本は[デバッグ起動](https://code.visualstudio.com/docs/editor/debugging) で確認することができるが、  
Node.js(Javascript) 固有の設定は[Node.js VSCodeでのデバッグ](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)の方が詳しい。

`Ctrl+Shift+P` を押しコマンドパレットで `launch.json` と入力することで「Debug: Open launch.json」という  
コマンドを実行する。  
表示された選択肢は `Node.js` 、なければ Other を選択する。

作成されたファイルに以下のように設定を記載する

```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Program",
      "program": "${file}",
      "protocol": "inspector",
      "sourceMaps": true,
      "sourceMapPathOverrides": {
        "webpack:///./~/*": "${workspaceRoot}/node_modules/*",
        "webpack:///./*": "${workspaceRoot}/*",
        "webpack:///*": "*"
      }
    }
  ]
}
```

`name` , `program` 辺りを自身の環境に合わせて設定しておく。

注意しなければならないのが、 `protocol` であり、これは必ず `"inspector"` を指定しておく。  
これをしておかないと、 **Node.js のバージョンが6系以下を使用した場合に `"legacy"` が指定された事になり、ソースマップが利用されない可能性がある**。  
`"inspector"` 設定は、現時点でサポートされている全ての Node.js バージョン (>= 6.3 (Windows: >= 6.9)) で  
サポートされているので、 Node.js が古すぎて使えない場合はアップデートを行う。 (バージョン6系も既に古すぎるというツッコミはしてはいけない)

基本はこれらの設定で問題ないはずだが、もし動作しなければ、最初に `outFiles` , `sourceMapPathOverrides` 辺りをいじってみる。

#### 3. デバッグ起動を行う。

`F5` でデバッグ実行を行う。実行する起動設定を聞かれたら 2. の `name` で指定した項目を選択する。

オリジナルソース（エディタで編集を行っているファイル）上でブレークポイント (`F9`) を設定して正しくブレークされれば成功。

<!-- ### `err.stack` に含まれる行番号をソースマップ済みのものにする -->

<!-- ### 生成後の行番号をオリジナルソースの行番号に変換する -->
