---
title: second ページ
date: 2018-04-22 17:40:45
tags:
- github
- pages
---

```
設定	説明	デフォルト
layout	レイアウト	
title	タイトル	
date	公開日	ファイルが作成された日付
updated	更新日	ファイル更新日
comments	投稿のコメント機能を有効にする	真実
tags	タグ（ページでは使用できません）	
categories	カテゴリ（ページには表示されません）	
permalink	投稿のデフォルトのパーマネントリンクを上書きする

categories:
- Sports
- Baseball
tags:
- Injury
- Fight
- Shocking

<% for (var link in site.data.menu) { %> 
  <a href="<%= site.data.menu[link] %>"> <%= link %> </a>
<% } %>
```