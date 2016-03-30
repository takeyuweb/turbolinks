# Turbolinks

> Turbolinks makes navigating your web application faster. Get the performance benefits of a single-page application without the added complexity of a client-side JavaScript framework. Use HTML to render your views on the server side and link to pages as usual. When you follow a link, Turbolinks automatically fetches the page, swaps in its `<body>`, and merges its `<head>`, all without incurring the cost of a full page load.

**TurbolinksはあなたのWebアプリケーションの閲覧を高速化します。**複雑なクライアントサイドJavaScriptフレームワークを追加することなしにシングルページアプリケーションのパフォーマンス上の恩恵を得ることができます。サーバーサイドのビューにHTMLを使って、いつものようにページにリンクを掛けます。リンクを辿るとき、Turbolinksは自動的にページを取得、`<body>`の置き換え、`<head>`のマージ、すべてページ全体の読み込みにかかるコストを招くことなくできます。

<img src="https://camo.githubusercontent.com/a9098e08c266f53bec1998d2cb1d11815c11145c/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f747572626f6c696e6b732d646f63732f696d616765732f747572626f6c696e6b732e676966">

### 機能

> - Optimizes navigation automatically. No need to annotate links or specify which parts of the page should change.
> - No server-side cooperation necessary. Respond with full HTML pages, not partial page fragments or JSON.
> - Respects the web. The Back and Reload buttons work just as you’d expect. Search engine-friendly by design.
> - Supports mobile apps. Adapters for iOS and Android let you build hybrid applications using native navigation controls.

- **ナビゲーションの最適化。**リンクに注釈（アノテーション）を付けたり、ページのどの部分を変更するか指定するする必要はありません。
- **サーバーサイドの協調は不要。**フラグメントやJSONではなく、完全なHTMLページを返します。
- **Webの尊重。**戻るボタンや再読込ボタンは期待通り動作します。サーチエンジンフレンドリーな設計な設計です。
- **モバイルアプリ対応。**iOS/Androidアダプタでネイティブなナビゲーションコントロールを使ったハイブリッドアプリケーションを構築しましょう。

### サポートするブラウザ

> Turbolinks works in all modern desktop and mobile browsers. It depends on the HTML5 History API and Window.requestAnimationFrame. In unsupported browsers, Turbolinks gracefully degrades to standard navigation.

すべてのモダンブラウザとモバイルブラウザで動作します。[HTML5 History API](https://developer.mozilla.org/en-US/docs/Web/API/History)と[Window.requestAnimationFrame](http://caniuse.com/#search=requestAnimationFrame)に依存します。サポートしないブラウザでは、標準的なナビゲーションに戻します。（Grafeful Degration）

### インストール

> Include dist/turbolinks.js in your application’s JavaScript bundle.

`dist/turbolinks.js`をアプリケーションのJavaScriptセットに含めて下さい。

#### Railsインテグレーション

> The Turbolinks gem is packaged as a Rails engine and integrates seamlessly with the Rails asset pipeline. To install:

Turbolinks gemはRailsエンジンとしてパッケージ化されていて、Rails asset pipelineとシームレスに統合されています。

> 1. Add the turbolinks gem, version 5, to your Gemfile: gem 'turbolinks', '~> 5.0.0.beta'
> 2. Run bundle install.
> 3. Add //= require turbolinks to your JavaScript manifest file (usually found at app/assets/javascripts/application.js).

1. Gemfileに`turbolinks`gem バージョン5を追加。: `gem 'turbolinks', '~> 5.0.0.beta'`
2. `bundle install`実行。
3. `//= require turbolinks`をJavascriptマニフェストファイルに追加。（たいていは`app/assets/javascripts/application.js`）

## Navigating with Turbolinks

> Turbolinks intercepts all clicks on `<a href>` links to the same domain. When you click an eligible link, Turbolinks prevents the browser from following it. Instead, Turbolinks changes the browser’s URL using the History API, requests the new page using XMLHttpRequest, and then renders the HTML response.

Turbolinksは同じドメインの`<a href>`リンクのすべてのクリックを横取りします。適当なリンクをクリックすると、Turbolinksはそれをブラウザが辿るのを防ぎます。その代わりに[History API](https://developer.mozilla.org/en-US/docs/Web/API/History)を使ってブラウザのURLを変え、[XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)を使って新しいページを要求し、それからHTMLレスポンスを描画します。

> During rendering, Turbolinks replaces the current `<body>` element outright and merges the contents of the `<head>` element. The JavaScript window and document objects, and the HTML `<html>` element, persist from one rendering to the next.

レンダリングの間に、Turbolinksは現在の`<body>`を完全に置き換え、`<head>`のコンテンツをマージします。`window`と`document`オブジェクト、それに`<html>`要素は維持します。

### ナビゲーションはVisit

> Turbolinks models navigation as a visit to a location (URL) with an action.

Turbolinksは*action*をロケーション（URL）への*visit*のナビゲーションとしてモデル化します。（？）

> Visits represent the entire navigation lifecycle from click to render. That includes changing browser history, issuing the network request, restoring a copy of the page from cache, rendering the final response, and updating the scroll position.

*Visit*はクリックから描画まで全体のナビゲーションライフサイクルに相当します。それには、ブラウザ履歴変更、ネットワークリクエスト発行、キャッシュからのページコピーの復元、最終的な応答の描画とスクロール位置の更新が含まれます。

`visit`には２種類あります: *進展（advance）*または*置換（replace）*のアクションをもつ*application visit*、それから*復元（restore）*のアクションをもつ*restoration visit*


### Application Visits

> Application visits are initiated by clicking a Turbolinks-enabled link, or programmatically by calling Turbolinks.visit(location).

Application VisitsはTurbolinksが有効なリンクのクリックまたは、`Turbolinks.visit(location)`を呼ぶことにより初期化されます。

> An application visit always issues a network request. When the response arrives, Turbolinks renders its HTML and completes the visit.

Application Visitsはいつもネットワークリクエストを発生させます。応答が返ると、TurbolinksはそのHTMLを描画し、visitを完了します。

> If possible, Turbolinks will render a preview of the page from cache immediately after the visit starts. This improves the perceived speed of frequent navigation between the same pages.

もし可能なら、Turbolinksはvisitが始まった後即座にキャッシュからプレビューを描画します。これは同じページへ頻繁に遷移するときの体感速度を改善します。

> If the visit’s location includes an anchor, Turbolinks will attempt to scroll to the anchored element. Otherwise, it will scroll to the top of the page.

もしアンカーが含まれていたら、Turbolinksはアンカー先の要素へのスクロールを試みます。それがなければページの上部へスクロールします。

> Application visits result in a change to the browser’s history; the visit’s action determines how.

Application Visitsはブラウザの履歴の変化をもたらします。そのvisitのアクションが、どのようにするかを決めます。

<img src="https://camo.githubusercontent.com/17c33fd7fe4e7a747d505b926f1a429efcf67be5/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f747572626f6c696e6b732d646f63732f696d616765732f616476616e63652e737667">

> The default visit action is advance. During an advance visit, Turbolinks pushes a new entry onto the browser’s history stack using history.pushState.

標準のvisitアクションは*advance（進展）*です。advance visitの間、Turbolinksは[history.pushState](https://developer.mozilla.org/en-US/docs/Web/API/History/pushState)を使ってブラウザの履歴スタックに新しいエントリを追加します。

> Applications using the Turbolinks iOS adapter typically handle advance visits by pushing a new view controller onto the navigation stack. Similarly, applications using the Android adapter typically push a new activity onto the back stack.

[iOSアダプタ](https://github.com/turbolinks/turbolinks-ios)を使うアプリケーションは一般的にナビゲーションスタックに新しいViewControllerを追加することでadvance visitを扱います。同様に[Androidアダプタ](https://github.com/turbolinks/turbolinks-android)を使うアプリケーションは例によってバックスタックに新しいActivityを追加します。

<img src="https://camo.githubusercontent.com/9962ffa357e8cb009b46dd706b277d338952ae96/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f747572626f6c696e6b732d646f63732f696d616765732f7265706c6163652e737667">

> You may wish to visit a location without pushing a new history entry onto the stack. The replace visit action uses history.replaceState to discard the topmost history entry and replace it with the new location.

スタックに新しい履歴を追加することなしにvisitを処理したい場合。*replace（置換）* visitアクションは一番上の履歴を破棄して新しいlocationで置き換えるのに[history.replaceState](https://developer.mozilla.org/en-US/docs/Web/API/History/pushState)を使います。

> To specify that following a link should trigger a replace visit, annotate the link with data-turbolinks-action="replace":

replace visitを作動さするようリンクを指定するには、`data-turbolinks-action="replace"`とアノテーションを入れます。

```html
<a href="/edit" data-turbolinks-action="replace">Edit</a>
```

> To programmatically visit a location with the replace action, pass the action: "replace" option to Turbolinks.visit:

relaceアクションをプログラムから使うには、`action: "replace"`オプションを`Turbolinks.visit`に渡します：

```javascript
Turbolinks.visit("/edit", { action: "replace" })
```

> Applications using the Turbolinks iOS adapter typically handle replace visits by dismissing the topmost view controller and pushing a new view controller onto the navigation stack without animation.

[iOSアダプタ](https://github.com/turbolinks/turbolinks-ios)を使うアプリケーションでは、一番上のViewControllerを破棄して新しいViewControllerをアニメーションなしにナビゲーションスタックに追加することで、replace visitsを扱います。

## Restoration Visits

> Turbolinks automatically initiates a restoration visit when you navigate with the browser’s Back or Forward buttons. Applications using the [iOS](https://github.com/turbolinks/turbolinks-ios) or [Android](https://github.com/turbolinks/turbolinks-android) adapters initiate a restoration visit when moving backward in the navigation stack.

Turbolinksはブラウザの戻るや進むボタンを押したとき、自動的に復元のvisitを開始します。iOSアダプタを使うアプリケーションはナビゲーションスタックを後退したときです。

![Restore visit action](https://s3.amazonaws.com/turbolinks-docs/images/restore.svg)

> If possible, Turbolinks will render a copy of the page from cache without making a request. Otherwise, it will retrieve a fresh copy of the page over the network. See [Understanding Caching](#understanding-caching) for more details.

可能ならTurbolinksはネットワークリクエストを発生させずにキャッシュのページコピーをレンダリングしようとします。そうでなければあたらいコピーをネットワーク越しに取得します。詳しくは[キャッシュを理解する](#understanding-caching)をお読み下さい。

> Turbolinks saves the scroll position of each page before navigating away and automatically returns to this saved position on restoration visits.

Turbolinkは移動の前にページのスクロール位置を記録しておき、復元のvisitのとき自動的にその位置に戻します。

> Restoration visits have an action of _restore_ and Turbolinks reserves them for internal use. You should not attempt to annotate links or invoke [`Turbolinks.visit`](#turbolinksvisit) with an action of `restore`.

復元のvisitは_restore_アクションを持っていて、内部的な利用のために予約しています（？）。`restore`アクションで[`Turbolinks.visit`](#turbolinksvisit)を試みてはいけません。

## Canceling Visits Before They Start

> Application visits can be canceled before they start, regardless of whether they were initiated by a link click or a call to [`Turbolinks.visit`](#turbolinksvisit).

Application visitはリンクをクリックしたか[`Turbolinks.visit`]を呼んだかに関係なく、それを開始前にキャンセルできます。

> Listen for the `turbolinks:before-visit` event to be notified when a visit is about to start, and use `event.data.url` (or `$event.originalEvent.data.url`, when using jQuery) to check the visit’s location. Then cancel the visit by calling `event.preventDefault()`.

visitが開始されたときにイベントを通知されるように`turbolinks:before-visit`イベントをListenして、visitのlocationを`event.data.url`（またはjQueryを使う場合`$event.originalEvent.data.url`）で調べて、`event.preventDefault()`を呼び出すことでそのvisitをキャンセルします。

> Restoration visits cannot be canceled and do not fire `turbolinks:before-visit`. Turbolinks issues restoration visits in response to history navigation that has *already taken place*, typically via the browser’s Back or Forward buttons.

Restration visitはキャンセルできず、`turbolinks:before-visit`も発火しません。Turbolinksは応答に*すでに来た場所*のナビゲーション履歴に応答して、restration visitを発行します。一般的にはブラウザの戻るや進むボタンを介します。

## Disabling Turbolinks on Specific Links 特定のリンクでTurbolinksを無効化する

> Turbolinks can be disabled on a per-link basis by annotating a link or any of its ancestors with `data-turbolinks="false"`.

Turbolinksはリンクかその祖先に`data-turbolinks="false"`とアノテーションを付けることで、リンクごとに無効化できます。

```html
<a href="/" data-turbolinks="false">Disabled</a>

<div data-turbolinks="false">
  <a href="/">Disabled</a>
</div>
```

> To reenable when an ancestor has opted out, use `data-turbolinks="true"`:

祖先から独立して有効化するには、`data-turbolinks="true"`を使います。

```html
<div data-turbolinks="false">
  <a href="/" data-turbolinks="true">Enabled</a>
</div>
```

> Links with Turbolinks disabled will be handled normally by the browser.

Turbolinksが無効化されたリンクは普通にブラウザに扱われます。

# Building Your Turbolinks Application

> Turbolinks is fast because it doesn’t reload the page when you follow a link. Instead, your application becomes a persistent, long-running process in the browser. This requires you to rethink the way you structure your JavaScript.

Turbolinksはリンクを辿るときページを再読込しないので速いです。代わりに、アプリケーションは永続化し、ブラウザの中で長く続くプロセスになります。このことはJavaScriptの組み方を再考する必要があります。

> In particular, you can no longer depend on a full page load to reset your environment every time you navigate. The JavaScript `window` and `document` objects retain their state across page changes, and any other objects you leave in memory will stay in memory.

とりわけ、もはや閲覧の度に環境がリセットされるページ全体の読み込みに依存できません。JavaScriptの`window`と`document`オブジェクトはページ遷移を超えて維持しますし、あなたがメモリ内に残した他のオブジェクトもメモリに残り続けます。

> With awareness and a little extra care, you can design your application to gracefully handle this constraint without tightly coupling it to Turbolinks.


意識して少し注意を払えば、Turbolinksと密結合させずにこの問題をうまく扱えるようアプリケーションを設計できます。

## Running JavaScript When a Page Loads ページ読み込みのときのJavaScript実行

> You may be used to installing JavaScript behavior in response to the `window.onload`, `DOMContentLoaded`, or jQuery `ready` events. With Turbolinks, these events will fire only in response to the initial page load—not after any subsequent page changes.

`window.onload`、`DOMContentLoaded`またはJQueryの`ready`イベントに応じたJavaScriptの振る舞いの設置は可能です。Turbolinksでこれらのイベントは最初のページ読み込みでだけ応答し、その後に続くページ遷移では応答しません。

> In many cases, you can simply adjust your code to listen for the `turbolinks:load` event, which fires once on the initial page load and again after every Turbolinks visit.

多くの場合、最初のページ読み込みの一回に加えてTurbolinksのvisitの後毎回発火する`turbolinks:load`イベントをListenするようにコードをかんたんに調整できます。

```js
document.addEventListener("turbolinks:load", function() {
  // ...
})
```

> When possible, avoid using the `turbolinks:load` event to add event listeners directly to elements on the page body. Instead, consider using [event delegation](https://learn.jquery.com/events/event-delegation/) to register event listeners once on `document` or `window`.

可能なら、イベントリスナー直接ページbodyの要素に`turbolinks:load`イベントを設定することは避けてください。かわりに、`document`か`window`のどちらかのイベントリスナーに登録して[イベントの委譲](https://learn.jquery.com/events/event-delegation/)を使うことを検討してください。

## Understanding Caching キャッシュを理解する

> Turbolinks maintains a cache of recently visited pages. This cache serves two purposes: to display pages without accessing the network during restoration visits, and to improve perceived performance by showing temporary previews during application visits.

Turbolinksは最近訪れたページのキャッシュを保持します。このキャッシュは2つの目的で動きます：復元のvisitの際ネットワークアクセスなしにページを表示すること、及びapplication visitsの際に一時プレビューを表示し体感速度を改善することです。

> When navigating by history (via [Restoration Visits](#restoration-visits)), Turbolinks will restore the page from cache without loading a fresh copy from the network, if possible.

履歴による遷移（[Restration Visits](#restration-visits)参照）のとき、Turbolinksは可能ならばネットワークから新しいコピーを読み込むことなしに、キャッシュからページを復元しようとします。

> Otherwise, during standard navigation (via [Application Visits](#application-visits)), Turbolinks will immediately restore the page from cache and display it as a preview while simultaneously loading a fresh copy from the network. This gives the illusion of instantaneous page loads for frequently accessed locations.

もしくは、一般的な遷移（[Application Visits](#application-visits)参照）のとき、Turbolinksはネットワークから新しいコピーを読み込むのと平行して、キャッシュからページをすぐに復元してプレビューとして表示しようとします。これは頻繁にアクセスされる場所で瞬時に読み込むような錯覚を与えます。

> Turbolinks saves a copy of the current page to its cache just before rendering a new page. Note that Turbolinks copies the page using [`cloneNode(true)`](https://developer.mozilla.org/en-US/docs/Web/API/Node/cloneNode), which means any attached event listeners and associated data are discarded.

Turbolinksは新しいページをレンダリングする直前、キャッシュに現在のページのコピーを保存します。Turbolinksがページをコピーするのに[`cloneNode(true)`](https://developer.mozilla.org/en-US/docs/Web/API/Node/cloneNode)を使うことに注意して下さい。これはアタッチされたイベントや関連付けられたデータはすべて破棄されることを意味します。

### Preparing the Page to be Cached キャッシュの前準備

> Listen for the `turbolinks:before-cache` event if you need to prepare the document before Turbolinks caches it. You can use this event to reset forms, collapse expanded UI elements, or tear down any third-party widgets so the page is ready to be displayed again.

もしTurbolinksがキャッシュする前にドキュメントの準備が必要なら`turbolinks:before-cache`イベントをListenしてください。このイベントはフォームのリセット、拡張されたUI要素の折りたたみ、あるいはページが再度表示されたとき準備できているようにするのにサードパーティーのウィジェットの破棄処理などに使えます。

```js
document.addEventListener("turbolinks:before-cache", function() {
  // ...
})
```

### Detecting When a Preview is Visible プレビューが表示されているときの検出

> Turbolinks adds a `data-turbolinks-preview` attribute to the `<html>` element when it displays a preview from cache. You can check for the presence of this attribute to selectively enable or disable behavior when a preview is visible.

Turbolinksはキャッシュからプレビューを表示するとき`data-turbolinks-preview`属性を`<html>`要素に追加します。プレビューが表示されているときの、選択的な有効または無効な振る舞いのためにこの属性が与えられていることを確認できます。


```js
if (document.documentElement.hasAttribute("data-turbolinks-preview")) {
  // Turbolinks is displaying a preview
}
```

### Opting Out of Caching キャッシュの除外

> You can control caching behavior on a per-page basis by including a `<meta name="turbolinks-cache-control">` element in your page’s `<head>` and declaring a caching directive.

ページの`<head>`内に`<meta name="turbolinks-cache-control">`要素を含めてキャッシュの指定を宣言することで、キャッシュの振る舞いを制御できます。

> Use the `no-preview` directive to specify that a cached version of the page should not be shown as a preview during an application visit. Pages marked no-preview will only be used for restoration visits.

application visitのときキャッシュ版のページがプレビューとして表示されないようにするために`no-preview`ディレクティブを使います。

> To specify that a page should not be cached at all, use the `no-cache` directive. Pages marked no-cache will always be fetched over the network, including during restoration visits.

そのページは全くキャッシュしないことを指定するには、`no-cache`ディレクティブを使います。no-cacheにマークされたページは、復元のvisitも含めていつもネットワーク越しに取得します。

```html
<head>
  ...
  <meta name="turbolinks-cache-control" content="no-cache">
</head>
```

> To completely disable caching in your application, ensure every page contains a no-cache directive.

アプリケーションで完全にキャッシュを無効化するには、すべてのページがno-cacheディレクティブを含むようにします。

## Making Transformations Idempotent Transformationsの冪等性づくり

> Often you’ll want to perform client-side transformations to HTML received from the server. For example, you might want to use the browser’s knowledge of the user’s current time zone to group a collection of elements by date.

サーバから受け取ったHTMLにクライアントサイドで加工したくなることはよくあります。たとえば、日付で要素のコレクションをグループわけするのにブラウザが認識したユーザーのタイムゾーンを使った方がいいでしょう。

> Suppose you have annotated a set of elements with `data-timestamp` attributes indicating the elements’ creation times in UTC. You have a JavaScript function that queries the document for all such elements, converts the timestamps to local time, and inserts date headers before each element that occurs on a new day.

要素のUTC表記での作成日時を示す`data-timestamp`属性を持つ要素のセットを持っているとします。その全部の要素を文書から照会して、タイムスタンプをローカル日時に変換し、日付ヘッダーを新しい日の要素の前に挿入するJavaScript関数を持っています。

> Consider what happens if you’ve configured this function to run on `turbolinks:load`. When you navigate to the page, your function inserts date headers. Navigate away, and Turbolinks saves a copy of the transformed page to its cache. Now press the Back button—Turbolinks restores the page, fires `turbolinks:load` again, and your function inserts a second set of date headers.

`turbolinks:load`でこの関数を実行するように設定したら、何が起こるか考えてみて下さい。ページ遷移のとき、あなたの関数は日付ヘッダーを挿入します。ページを離れると、Turbolinksはキャッシュに加工済みページのコピーを保存します。今度は戻るぼたんを押すと･･･Turbolinksはページを復元し、`turbolinks:load`をもう一度実行します。そして関数は2度目の日付ヘッダーを挿入します。

> To avoid this problem, make your transformation function _idempotent_. An idempotent transformation is safe to apply multiple times without changing the result beyond its initial application.

この問題を回避するには、冪等な加工関数を作って下さい。冪等な加工は、最初の適用の後に複数回実行しても結果が変わらず安全です。

> One technique for making a transformation idempotent is to keep track of whether you’ve already performed it by setting a `data` attribute on each processed element. When Turbolinks restores your page from cache, these attributes will still be present. Detect these attributes in your transformation function to determine which elements have already been processed.

冪等な加工を作る1つのテクニックは、`data`属性を加工されたそれぞれの要素にセットし、実行済みかどうか追跡することです。Turbolinksがキャッシュからページを復元したとき、これらの属性は依然として存在します。要素がすでに実行済みかどうかを判断するために、あなたの加工関数でこれらの属性を検出します。

> A more robust technique is simply to detect the transformation itself. In the date grouping example above, that means checking for the presence of a date divider before inserting a new one. This approach gracefully handles newly inserted elements that weren’t processed by the original transformation.

より堅牢なテクニックは単純に加工自体で検出することです。上記の日付グループ分けの例では、新しいものを挿入する前に日付の区切りがあることを調べることを意味します。このアプローチは元の加工によって処理されなかった新しく挿入された要素をうまく扱います。

## Responding to Page Updates ページ変更の応答

> Turbolinks may not be the only source of page updates in your application. New HTML can appear at any time from Ajax requests, WebSocket connections, or other client-side rendering operations, and this content will need to be initialized as if it came from a fresh page load.

Turbolinksはあなたのアプリケーションの更新の唯一の情報源ではないかもしれません。新しいHTMLはAjaxリクエスト、WebSocketコネクション、あるいは他のクライアントサイド描画処理からいつでも表示することができ、そしてこの内容は新しいページ読み込みのように初期化する必要があります。

> You can handle all of these updates, including updates from Turbolinks page loads, in a single place with the precise lifecycle callbacks provided by [`MutationObserver`](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver) and [Custom Elements](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Custom_Elements).

[`MutationObserver`](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver)と[Custom Elements](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Custom_Elements)により提供される明確なライフサイクルコールバックの１つで、Turbolinksページ読み込みからの更新を含め、これらの更新のすべてを扱うことができます。

> In particular, these APIs give you callbacks when elements are attached to and removed from the document. You can use these callbacks to perform transformations and register or tear down behavior as soon as matching elements appear on the page, regardless of how they were added.

とりわけ、これらのAPIはドキュメントから要素が追加されたり削除されるときのコールバックを提供します。一致したページ要素が現れると、すぐに加工を実行したり振る舞いの登録や取り外したりするのにこれらのコールバックを使え、それらがどうやって追加されたか気にしません。

> By taking advantage of `MutationObserver`, Custom Elements, and [idempotent transformations](#making-transformations-idempotent), there’s little need to couple your application to Turbolinks’ events.

`MutationObserver`、Custom Elements、および[冪等な加工](#making-transformations-idempotent)を利用することで、あなたのアプリケーションとTurbolinksのイベントとを結びつける必要はほとんどありません。

## Persisting Elements Across Page Loads ページ読み込みを超えて存在する要素

> Turbolinks allows you to mark certain elements as _permanent_. Permanent elements persist across page loads, so that any changes you make to those elements do not need to be reapplied after navigation.

永続するように、Turbolinksは特定の要素をマークできます。永続化要素はページの読み込みを超えて存在し、そのためこれらの要素にするどんな変更も、遷移後に再適用する必要ありません。

> Consider a Turbolinks application with a shopping cart. At the top of each page is an icon with the number of items currently in the cart. This counter is updated dynamically with JavaScript as items are added and removed.

ショッピングカートのTurbolinksアプリケーションを考えます。各ページの上部に現在カートに入っている商品数のアイコンがあります。
このカウンターはJavaScriptで動的に更新されます。たとえば商品の追加と削除です。

> Now imagine a user who has navigated to several pages in this application. She adds an item to her cart, then presses the Back button in her browser. Upon navigation, Turbolinks restores the previous page’s state from cache, and the cart item count erroneously changes from 1 to 0.

次にユーザーがこのアプリケーションでいくつかのページを遷移したことをイメージします。彼女はカートに商品を入れ、そしてブラウザの戻るボタンを押します。遷移で、Turbolinksはキャッシュから前のページの状態を復元し、カートの商品数が誤って1から0に変わります。

> You can avoid this problem by marking the counter element as permanent. Designate permanent elements by giving them an HTML `id` and annotating them with `data-turbolinks-permanent`.

カウンター要素を永続化することでこの問題を回避できます。それらのHTMLに`id`を与えて`data-turbolinks-permanent`を付記することで、永続化要素に指定します。

```html
<div id="cart-counter" data-turbolinks-permanent>1 item</div>
```

> Before each render, Turbolinks matches all permanent elements by `id` and transfers them from the original page to the new page, preserving their data and event listeners.

描画前、Turbolinksは`id`が一致するすべての永続化要素を元のページから新しいページに移動し、それらのデータとイベントリスナーを保持します。

# Advanced Usage 高度な使い方

## Displaying Progress 進捗の表示

> During Turbolinks navigation, the browser will not display its native progress indicator. Turbolinks installs a CSS-based progress bar to provide feedback while issuing a request.

Turbolinksの遷移の間、ブラウザネイティブな進捗インディケータは表示されません。リクエストを発行している間、Turbolinksは応答を提供する為にCSSベースのプログレスバーを取り付けます。

> The progress bar is enabled by default. It appears automatically for any page that takes longer than 500ms to load.

プログレスバーはデフォルトで有効です。読み込みに500ms以上かかるページに自動で表示します。

> The progress bar is a `<div>` element with the class name `turbolinks-progress-bar`. Its default styles appear first in the document and can be overridden by rules that come later.

プログレスバーは`turbolinks-progress-bar`クラスの`<div>`要素です。デフォルトのスタイルでドキュメントの先頭に表示されて、次のルールで上書きできます。

> For example, the following CSS will result in a thick green progress bar:

たとえば、次のCSSで厚みのある緑のプログレスバーになります：

```css
.turbolinks-progress-bar {
  height: 5px;
  background-color: green;
}
```

> To disable the progress bar entirely, set its `visibility` style to `hidden`:

完全に非表示にするには、`visibility`スタイルを`hidden`にします：

```css
.turbolinks-progress-bar {
  visibility: hidden;
}
```

## Reloading When Assets Change アセット変更時の再読込

> Turbolinks can track the URLs of asset elements in `<head>` from one page to the next, and automatically issue a full reload if they change. This ensures that users always have the latest versions of your application’s scripts and styles.

Turbolinksは、あるページから次のページへの`<head>`内アセット要素のURLを追跡できます。

> Annotate asset elements with `data-turbolinks-track="reload"` and include a version identifier in your asset URLs. The identifier could be a number, a last-modified timestamp, or better, a digest of the asset’s contents, as in the following example.

アセット要素に`data-turbolinks-track="reload"`と注記し、アセットURLにバージョン識別子を含めて下さい。識別子は、数値、最終更新日時、その他適当なもの、次の例のようなアセットコンテンツのdigestにできるでしょう。


```html
<head>
  ...
  <link rel="stylesheet" href="/application-258e88d.css" data-turbolinks-track="reload">
  <script src="/application-cbd3cd4.js" data-turbolinks-track="reload"></script>
</head>
```

> Note that Turbolinks will only consider tracked assets in `<head>` and not elsewhere on the page.

Turbolinksは`<head>`内のアセットの追跡のみ考慮していて、ページの他の場所はそうでないことに注意して下さい。

## Setting a Root Location ルートロケーションの設定

> By default, Turbolinks only loads URLs with the same origin—i.e. the same protocol, domain name, and port—as the current document. A visit to any other URL falls back to a full page load.

標準でTurbolinksは現在のドキュメントと同じオリジン-すなわち同じプロトコル、ドメイン名、ポート-のURLのみ読み込みます。

> In some cases, you may want to further scope Turbolinks to a path on the same origin. For example, if your Turbolinks application lives at `/app`, and the non-Turbolinks help site lives at `/help`, links from the app to the help site shouldn’t use Turbolinks.

場合によっては、同じオリジンの１つのパスにTurbolinksを働かせたいことがあるでしょう。例えば、あなたのTurbolinksアプリケーションが`/app`にあり、Turbolinksでないヘルプサイトが`/help`にあって、アプリからヘルプサイトのリンクにTurbolinksを使ってはいけないときです。

> Include a `<meta name="turbolinks-root">` element in your pages’ `<head>` to scope Turbolinks to a particular root location. Turbolinks will only load same-origin URLs that are prefixed with this path.

特定のルートロケーションにTurbolinksを働かせるには`<meta name="turbolinks-root">`要素をページの`<head>`に含めて下さい。

```html
<head>
  ...
  <meta name="turbolinks-root" content="/app">
</head>
```

## Following Redirects リダイレクトの追従

> When you visit location `/one` and the server redirects you to location `/two`, you expect the browser’s address bar to display the redirected URL.

`/one`を訪れて`/two`にサーバーがリダイレクトしたとき、ブラウザのアドレスバーにリダイレクト先URLを表示するのを期待します。

> However, Turbolinks makes requests using `XMLHttpRequest`, which transparently follows redirects. There’s no way for Turbolinks to tell whether a request resulted in a redirect without additional cooperation from the server.

しかしながら、Turbolinksはリダイレクトを透過的に追従し`XMLHttpRequest`を使ってリクエストを発行します。サーバからの特別な協力なしに要求がリダイレクトの結果に終わったかどうかTurbolinksに伝える方法はありません。

> To work around this problem, send the `Turbolinks-Location` header in response to a visit that was redirected, and Turbolinks will replace the browser’s topmost history entry with the value you provide.

この問題の回避策として、リダイレクトされた先の応答で`Turbolinks-Location`ヘッダを送出して、Turbolinksはブラウザの一番上の履歴とあなたが提供する値と置き換えます。

> The Turbolinks Rails engine sets `Turbolinks-Location` automatically when using `redirect_to` in response to a Turbolinks visit.

Turbolinks RailsエンジンはTurbolinksのvisitの応答の中で`redirect_to`を使ったとき、自動的に`Turbolinks-Location`を設定します。

## Redirecting After a Form Submission フォーム送信後のリダイレクト

> Submitting an HTML form to the server and redirecting in response is a common pattern in web applications. Standard form submission is similar to navigation, resulting in a full page load. Using Turbolinks you can improve the performance of form submission without complicating your server-side code.

HTMLフォームをサーバに送信した応答でリダイレクトすることは、Webアプリケーションでよくあるパターンです。標準的なフォーム送信はナビゲーションと同じように、完全なページ読み込みが生じます。Turbolinksを使ってサーバサイドのコードを複雑にせずにフォーム応答のパフォーマンスを改善できます。

> Instead of submitting forms normally, submit them with XHR. In response to an XHR submit on the server, return JavaScript that performs a [`Turbolinks.visit`](#turbolinksvisit) to be evaluated by the browser.

普通のフォーム送信の代わりに、XHRで送信します。サーバからの応答で、ブラウザによって評価され[`Turbolinks.visit`](#turbolinksvisit)を実行するためのJavaScriptを返します。

> If form submission results in a state change on the server that affects cached pages, consider clearing Turbolinks’ cache with [`Turbolinks.clearCache()`](#turbolinksclearcache).

もしフォーム送信がキャッシュされたページに影響を与えるサーバの状態変化を返したら、[`Turbolinks.clearCache()`](#turbolinksclearcache)でTurbolinksのキャッシュを削除することを考慮します。

> The Turbolinks Rails engine performs this optimization automatically for non-GET XHR requests that redirect with the `redirect_to` helper.

Turbolinks RailsエンジンはGET以外のHXRリクエストで`redirect_to`を使ってリダイレクトすれば、この最適化を自動的に行います。

## Setting Custom HTTP Headers カスタムHTTPヘッダーの設定

> You can observe the `turbolinks:request-start` event to set custom headers on Turbolinks requests. Access the request’s XMLHttpRequest object via `event.data.xhr`, then call the [`setRequestHeader`](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) method as many times as you wish.

Turbolinksリクエストのカスタムヘッダーを設定するのに`turbolinks:request-start`イベントを監視できます。`event.data.xhr`を介してリクエストのXMLHttpRequestオブジェクトにアクセスして、
何度でも[`setRequestHeader`](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)を呼び出せます。

> For example, you might want to include a request ID with every Turbolinks link click and programmatic visit.

例えば、Turbolinksリンクのクリックやプログラムされたvisitの度にリクエストIDを含めたいかもしれません。

```javascript
document.addEventListener("turbolinks:request-start", function(event) {
  var xhr = event.data.xhr
  xhr.setRequestHeader("X-Request-Id", "123...")
})
```

# API Reference APIリファレンス

## Turbolinks.visit

Usage:
```js
Turbolinks.visit(location)
Turbolinks.visit(location, { action: action })
```

> Performs an [Application Visit](#application-visits) to the given _location_ (a string containing a URL or path) with the specified _action_ (a string, either `"advance"` or `"replace"`).

> If _location_ is a cross-origin URL, or falls outside of the specified root (see [Setting a Root Location](#setting-a-root-location)), or if the value of [`Turbolinks.supported`](#turbolinkssupported) is `false`, Turbolinks performs a full page load by setting `window.location`.

> If _action_ is unspecified, Turbolinks assumes a value of `"advance"`.

> Before performing the visit, Turbolinks fires a `turbolinks:before-visit` event on `document`. Your application can listen for this event and cancel the visit with `event.preventDefault()` (see [Canceling Visits Before They Start](#canceling-visits-before-they-start)).

## Turbolinks.clearCache

> Usage:
```js
Turbolinks.clearCache()
```

> Removes all entries from the Turbolinks page cache. Call this when state has changed on the server that may affect cached pages.

## Turbolinks.supported

> Usage:
```js
if (Turbolinks.supported) {
  // ...
}
```

> Detects whether Turbolinks is supported in the current browser (see [Supported Browsers](#supported-browsers)).

## Full List of Events

> Turbolinks emits events that allow you to track the navigation lifecycle and respond to page loading. Except where noted, Turbolinks fires events on the `document` object.

> * `turbolinks:click` fires when you click a Turbolinks-enabled link. The clicked element is the event target. Access the requested location with `event.data.url`. Cancel this event to let the click fall through to the browser as normal navigation.

> * `turbolinks:before-visit` fires before visiting a location, except when navigating by history. Access the requested location with `event.data.url`. Cancel this event to prevent navigation.

> * `turbolinks:visit` fires immediately after a visit starts.

> * `turbolinks:request-start` fires before Turbolinks issues a network request to fetch the page. Access the XMLHttpRequest object with `event.data.xhr`.

> * `turbolinks:request-end` fires after the network request completes. Access the XMLHttpRequest object with `event.data.xhr`.

> * `turbolinks:before-cache` fires before Turbolinks saves the current page to cache.

> * `turbolinks:before-render` fires before rendering the page. Access the new `<body>` element with `event.data.newBody`.

> * `turbolinks:render` fires after Turbolinks renders the page. This event fires twice during an application visit to a cached location: once after rendering the cached version, and again after rendering the fresh version.

> * `turbolinks:load` fires once after the initial page load, and again after every Turbolinks visit. Access visit timing metrics with the `event.data.timing` object.

# Contributing to Turbolinks

> Turbolinks is open-source software, freely distributable under the terms of an [MIT-style license](LICENSE). The [source code is hosted on GitHub](https://github.com/turbolinks/turbolinks).
Development is sponsored by [Basecamp](https://basecamp.com/).

> We welcome contributions in the form of bug reports, pull requests, or thoughtful discussions in the [GitHub issue tracker](https://github.com/turbolinks/turbolinks/issues).

> Please note that this project is released with a [Contributor Code of Conduct](CONDUCT.md). By participating in this project you agree to abide by its terms.

## Building From Source

> Turbolinks is written in [CoffeeScript](https://github.com/jashkenas/coffee-script) and compiled to JavaScript with [Blade](https://github.com/javan/blade). To build from source you’ll need a recent version of Ruby. From the root of your Turbolinks directory, issue the following commands to build the distributable files in `dist/`:

```
$ gem install bundler
$ bundle install
$ bin/blade build
```

## Running Tests

> Follow the instructions for _Building From Source_ above. Then run `bin/blade runner` and visit the displayed URL in your browser. The Turbolinks test suite will start automatically.
