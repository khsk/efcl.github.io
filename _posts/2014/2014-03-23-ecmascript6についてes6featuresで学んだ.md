---
title: ECMAScript6についてes6featuresで学んだ
author: azu
layout: post
permalink: /2014/0323/res3750/
dsq_thread_id:
  - 2484655551
categories:
  - javascript
tags:
  - ECMAScript
  - javascript
---
## はじめに

ECMAScript6についてのは色々情報がでてきていて、またライブラリとかでも一部採用してるケースも見られてきました。

*   [ES6 &#8211; Next Generation Javascript][1]
*   [ES6 &#8211; episode 1][2]
*   [Rewriting A WebApp With ECMAScript 6 — TasteJS][3]

等など、色々紹介してるサイトはありますが、その中でも[lukehoban/es6features][4]はコード例が豊富に載っていて面白そうなので、写経してみました。

## 現在のES6とは

[harmony:specification_drafts [ES Wiki]][5]で公開されてるES6の仕様や[ES6FeatureSet.xlsx &#8211; Microsoft Excel Web App][6] 等を見るとAPI的に固まってきている部分は何となく見える感じはします。

[AngularJS 2.0 | AngularJS][7]がES6ベースで書いていくと方針が発表されたことで、[Traceur][8]等を使ったES6で書いてES5に変換して動かす部分にも興味を持った人もいるかもしれません。

[addyosmani/es6-tools][9]等を見ると、現段階でES6の機能をpolyfillやtranspilerで持ってこられるツールなどについてまとまってます。

今回は、[Traceur][8]をベースにして、[lukehoban/es6features][4]のコードを写経してみました。

## 写経

写経したコードは[azu/es6features-playground][10]に置いてあります。

環境としては traceur 0.0.30 + Node.js V0.11です。

polyfillなどでは実現が難しそうな

*   `Set`
*   `Map`
*   `WeakSet`
*   `WeakMap`

などはNode.js v0.11 で `--harmony` のオプションを使うと大体利用できます。(V8のオプションなので、Chromeとかでもフラグ付きで利用できるはず)

以下の項目は traceur 0.0.30 では変換できなさそうな感じでした。

*   &#8220;Enhanced Object Literals&#8221;
*   &#8220;Const&#8221;
*   &#8220;Comprehensions&#8221;
*   &#8220;Module Loaders&#8221;
*   &#8220;Subclassable Built-ins&#8221;
*   &#8220;Tail Calls&#8221;

`let` はtraceurに `--experimental`をフラグをつけると何故か対応してましたが、`try-catch`を使って無理やりやってたりしてて面白かったです(あんまり実用的じゃない感じだけど)

*   [let_const.js][11]

変換できない事が多く見えてしまうかもしれませんが、[lukehoban/es6features][4]に書かれてることはコレの3-4倍ぐらいあるので、意外と変換できるものは多いです。

`class` とかは `traceur-runtime.js` というランタイムファイルを読み込む必要があるのですが、[Arrows Function][12]や[Template Strings][13]、[Destructuring][14]、[Default + Rest + Spread][15]あたりはそのままrun-timeなしで展開して使えるので、この辺があるだけで結構便利になります。

traceurが対応してるものは一応以下にも書かれています。

*   [LanguageFeatures · google/traceur-compiler Wiki][16]

また、traceurのようなruntimeを使わないような変換ができることを目標にしてる[termi/es6-transpiler][17]というプロジェクトもあります。

### テスト

写経しながらテストコードもそのまま書いててて、  
テストはmocha + [power-assert][18]で書いてます。

power-assertを使うときに[intelli-espower-loader][19]をインストールしておくと楽です。([espower-loader][20]のラッパーです)

`$ mocha --require intelli-espower-loader` だけでpower-assertらしいテスト結果が出せるようになります。

<img src="https://efcl.info/wp-content/uploads/2014/03/NewImage.png" alt="NewImage" title="NewImage.png" border="0" width="600" height="303" />

<div class="highlight">
  <pre><span class="s2">"use strict"</span><span class="p">;</span>
<span class="kd">var</span> <span class="nx">assert</span> <span class="o">=</span> <span class="nx">require</span><span class="p">(</span><span class="s2">"power-assert"</span><span class="p">);</span>
<span class="nx">describe</span><span class="p">(</span><span class="s2">"Default"</span><span class="p">,</span> <span class="p">()</span><span class="o">=&gt;</span> <span class="p">{</span>
    <span class="nx">it</span><span class="p">(</span><span class="s2">"default parameter value"</span><span class="p">,</span> <span class="p">()</span><span class="o">=&gt;</span> <span class="p">{</span>
        <span class="kd">function</span> <span class="nx">f</span><span class="p">(</span><span class="nx">x</span><span class="p">,</span> <span class="nx">y</span><span class="o">=</span><span class="mi">12</span><span class="p">)</span> <span class="p">{</span>
            <span class="k">return</span> <span class="nx">x</span> <span class="o">+</span> <span class="nx">y</span><span class="p">;</span>
        <span class="p">}</span>
        <span class="nx">assert</span><span class="p">(</span><span class="nx">f</span><span class="p">(</span><span class="mi">3</span><span class="p">)</span> <span class="o">==</span> <span class="mi">15</span><span class="p">);</span>
    <span class="p">});</span>
<span class="p">});</span>
<span class="nx">describe</span><span class="p">(</span><span class="s2">"Rest"</span><span class="p">,</span> <span class="p">()</span><span class="o">=&gt;</span> <span class="p">{</span>
    <span class="nx">it</span><span class="p">(</span><span class="s2">"...parameter"</span><span class="p">,</span> <span class="p">()</span><span class="o">=&gt;</span> <span class="p">{</span>
        <span class="kd">function</span> <span class="nx">f</span><span class="p">(</span><span class="nx">x</span><span class="p">,</span> <span class="p">...</span><span class="nx">y</span><span class="p">)</span> <span class="p">{</span>
            <span class="k">return</span> <span class="nx">x</span> <span class="o">*</span> <span class="nx">y</span><span class="p">.</span><span class="nx">length</span><span class="p">;</span>
        <span class="p">}</span>
        <span class="nx">assert</span><span class="p">(</span><span class="nx">f</span><span class="p">(</span><span class="mi">3</span><span class="p">,</span> <span class="s2">"hello"</span><span class="p">,</span> <span class="kc">true</span><span class="p">)</span> <span class="o">===</span> <span class="mi">6</span><span class="p">);</span>
    <span class="p">});</span>
<span class="p">});</span>
<span class="nx">describe</span><span class="p">(</span><span class="s2">"Spread"</span><span class="p">,</span> <span class="p">()</span><span class="o">=&gt;</span> <span class="p">{</span>
    <span class="nx">it</span><span class="p">(</span><span class="s2">"pass each elem of array as argument"</span><span class="p">,</span> <span class="p">()</span><span class="o">=&gt;</span> <span class="p">{</span>
        <span class="kd">function</span> <span class="nx">f</span><span class="p">(</span><span class="nx">x</span><span class="p">,</span> <span class="nx">y</span><span class="p">,</span> <span class="nx">z</span><span class="p">)</span> <span class="p">{</span>
            <span class="k">return</span> <span class="nx">x</span> <span class="o">+</span> <span class="nx">y</span> <span class="o">+</span> <span class="nx">z</span><span class="p">;</span>
        <span class="p">}</span>
        <span class="nx">assert</span><span class="p">(</span><span class="nx">f</span><span class="p">(...[</span><span class="mi">1</span><span class="p">,</span><span class="mi">2</span><span class="p">,</span><span class="mi">3</span><span class="p">])</span> <span class="o">===</span> <span class="mi">6</span><span class="p">);</span>
    <span class="p">});</span>
<span class="p">});</span>
</pre>
</div>

Arrow Functionをそのまま使ってるので、BDD系のネストがあるやつも比較的見やすくなります。

## Editor

[azu/es6features-playground][21] にも書いていますが、  
[WebStorm 8][22]あたりからES6のサポートが色々改善されたので、traceurが扱えるコードはWebStormでも普通に書けるようになってます。(フォーマットとかリファクタリングとかできる)

また、File Watcherでtraceurは元から用意されてるので、結構気軽にtraceurを使ってES6ライクなJavaScriptを書けるようになってます。

## 感想

いろんなところで、「ES6はいつから使えるのか」という事について、「今すぐ使える」という話はよく聞くと思います。

*   [ES6 in Real Life][23]
*   [Using Grunt & the ES6 Module Transpiler][24]
*   [ECMAScript 6 Resources For The Curious JavaScripter][25]

実際に書いてみると、結構普通に使えてしまう感じがして感触は悪く無いです。

`let` や `Generator` 等変換を無理してやって使うべきじゃないものもありますが、ES6はシンタックスシュガー的なものも多いのでその辺は積極的に使いたいなーという感じになりました。(変換したもの自体は機械的に展開してる程度な感じのも多い)

[Functional JavaScriptと合わせた話][26]とか面白い[One Liner][27]ができたりするので、  
[Arrows Function][12]、[Template Strings][13]、[Destructuring][14]、[Default + Rest + Spread][15]とかは一度触ってみると楽しいと思います。

 [1]: http://www.slideshare.net/RameshNair6/es6-next-generation-javascript "ES6 - Next Generation Javascript"
 [2]: http://tagtree.tv/ecmascript-6-episode-1 "ES6 - episode 1"
 [3]: http://blog.tastejs.com/rewriting-a-webapp-with-ecmascript-6 "Rewriting A WebApp With ECMAScript 6 — TasteJS"
 [4]: https://github.com/lukehoban/es6features "lukehoban/es6features"
 [5]: http://wiki.ecmascript.org/doku.php?id=harmony:specification_drafts "harmony:specification_drafts [ES Wiki]"
 [6]: https://onedrive.live.com/view.aspx?resid=704A682DC00D8AAD!59602&app=Excel&authkey=!AAMixsO0TuyPYwc "ES6FeatureSet.xlsx - Microsoft Excel Web App"
 [7]: http://blog.angularjs.org/2014/03/angular-20.html "AngularJS 2.0 | AngularJS"
 [8]: https://github.com/google/traceur-compiler "Traceur"
 [9]: https://github.com/addyosmani/es6-tools "addyosmani/es6-tools"
 [10]: https://github.com/azu/es6features-playground "azu/es6features-playground"
 [11]: https://github.com/azu/es6features-playground/blob/master/out/src/let_const.js "let_const.js"
 [12]: https://github.com/lukehoban/es6features#arrows "Arrows"
 [13]: https://github.com/lukehoban/es6features#template-strings " Template Strings"
 [14]: https://github.com/lukehoban/es6features#template-strings " Destructuring"
 [15]: https://github.com/lukehoban/es6features#template-strings "Default + Rest + Spread"
 [16]: https://github.com/google/traceur-compiler/wiki/LanguageFeatures "LanguageFeatures · google/traceur-compiler Wiki"
 [17]: https://github.com/termi/es6-transpiler "termi/es6-transpiler"
 [18]: https://github.com/twada/power-assert "power-assert"
 [19]: https://github.com/azu/intelli-espower-loader "intelli-espower-loader"
 [20]: https://github.com/twada/espower-loader "espower-loader"
 [21]: https://github.com/azu/es6features-playground#setting-webstorm "azu/es6features-playground"
 [22]: http://www.jetbrains.com/webstorm/nextversion/ "WebStorm 8"
 [23]: http://www.slideshare.net/domenicdenicola/es6-in-real-life "ES6 in Real Life"
 [24]: http://www.thomasboyt.com/2013/06/21/es6-module-transpiler "Using Grunt & the ES6 Module Transpiler"
 [25]: http://addyosmani.com/blog/ecmascript-6-resources-for-the-curious-javascripter/ "ECMAScript 6 Resources For The Curious JavaScripter"
 [26]: https://nicolas.perriault.net/code/2013/functional-javascript-for-crawling-the-web/ "Functional JavaScript"
 [27]: http://h3manth.com/new/blog/2014/es6-one-liners-to-show-off/ "One Liner"
