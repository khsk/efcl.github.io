---
title: GreasemonkeyにGrowlのような通知を呼ぶ機能を加える「Dbus Notify」
author: azu
layout: post
permalink: /2010/0606/res1708/
SBM_count:
  - '00019<>1355431106<>12<>0<>3<>4<>0'
dsq_thread_id:
  - 300872874
categories:
  - Greasemonkey
  - アドオン
tags:
  - Firefox
  - Greasemonkey
  - growl
  - アドオン
---
紹介する**[Dbus Notify for GreaseMonkey][1]**はGreasemonkeyスクリプトにGrowlのような通知を行うAPIを加えるアドオンです。  
Greasemonkeyに*callout.notify*というAPIを追加するだけのシンプルなアドオンです。  
使い方は単純でDbus Notify for GreaseMonkeyをインストールして、Greasemonkeyスクリプト内にcallout.notifyのAPIを使った記述を加えるだけで動きます。(当たり前だが、インストールしてない環境だと動かない)

[<img class="alignnone size-full wp-image-1709" title="sshot-2010-06-06-1" src="https://efcl.info/wp-content/uploads/2010/06/sshot-2010-06-06-1.png" alt="" width="247" height="80" />][2]

APIは凄くシンプルで、*callout.notify(title, message, [options])*となっていて、タイトルはそのまま、メッセージはタイトルの下に表示されていて、optionのhrefが設定されている場合はリンクになる。

optionで設定できるのもhrefとiconぐらいで、Greasemonkeyからページの外側に通知を出したいなーって思う人はそれだけを求めるならアドオンなどにしないでこれを使うのもいいかも。

簡単なサンプル

**gist: 426775 &#8211; Dbus Notify for GreaseMonkeyのテスト- GitHub**
:   [http://gist.github.com/426775][3]

APIの解説

**lackac&#8217;s callout at master &#8211; GitHub**
:   [http://github.com/lackac/callout][4]

Greasemonkeyに何か機能を加えるアドオンって意外と見かけない感じがする。  
[Greasemonkey でクリップボードを扱う…悪い方法（？） &#8211; Griever][5] でも言っているように受け口を持つと悪用の可能性も出てくるが、セキュリティ的な影響がでないように狭い範囲で機能追加できれば楽しそうだなーと思った。

 [1]: https://addons.mozilla.org/ja/firefox/addon/80827/
 [2]: https://efcl.info/wp-content/uploads/2010/06/sshot-2010-06-06-1.png
 [3]: http://gist.github.com/426775 "gist: 426775 - Dbus Notify for GreaseMonkeyのテスト- GitHub"
 [4]: http://github.com/lackac/callout "lackac's callout at master - GitHub"
 [5]: http://d.hatena.ne.jp/Griever/20090617/1245256102
