English | [日本語](README_ja.md)  

# About this repository

## Overview
:smile: :fish: :rooster: :tropical_fish: :cat2: :ox: :pig2: :whale2: :smile:  

Hello. This is my html cording starter kid.
I often use the kit to markup static HTML before using CMS framework.

If your case includes the following, Please use the kid.

- Use Bootstrap as css library.
- Use jQuery
- Use SCSS
- Use BEM style
- Want to manage the design in units such as page layout, parts.

※ If you get good starter kid, I suggest [web starter kid of google](https://github.com/google/web-starter-kit).  
※ This is for creating basic web pages. I have another idea for web App using React, Vue.js. (I want to show the other way on another occasion ) 
※ It is under construction in some places.

:smile: :goat: :rabbit2: :leopard: :octopus: :dog2: :panda_face: :cow2: :smile:  

## Environment Information
This starter has the following structure.
```
Package management:
- Node.js + Yarn

Module bundler / Task runner:
- Webpack (v4)

Javascript transpiler:
- Babel

Style regulation / utilities:
- SCSS
- Autprefixer

Linter:
- eslint
- stylelint

Cording Libraries:
- jQuery
- Bootstrap4
```

# How to Use

## Installation
You can start coding immediately in the following way.
1. Clone this repository: `git clone git@github.com:sumi37/html-starter.git` (or download)
2. Move directory: `cd cd html-starter`
3. Install node modules with yarn: `yarn install`
4. Start server: `yarn start`
5. View the site at `http://localhost:8080/`

※ If you don't have node.js and yarn, Please install on your PC in advance.
※ If you want to stop server, Please use `ctrl+c` on your shell.

## How to add HTML files
Please add html files to under `/public/`.
```
ex:
./public/index.html => http://localhost:8080/
./public/foo.html => http://localhost:8080/foo.html
```
And this directory can be used as an area for saving static files.
so For example, it is recommended to save image files like `/public/images/*`


## How to add Stylesheets
Please add css files as scss to under `/src/scss`.
```
ex:
/src/scss/_foo.scss
```
And import the file to `/src/scss/index.scss`.
You can use both css style and scss style on scss files.


## How to add Javascripts
Please add css files to under `/src/js`.
```
ex:
/src/js/foo.js
```
And import the file to `/src/js/index.js`.
You can use both es5 style and es6 style on javascript files.


## Build and Reading files
If you get static files, There is `yarn run build`.
Please stop server once and enter the command.
`public/css` and `public/js` folder will get bundle files.

You have to load bundle file in HTML.
```
<link rel="stylesheet" href="/css/bundle.css">
<script src="/js/bundle.js"></script>
```
Also, while the server is active, 
the latest state is automatically reflected in the browser without executing the build.


# Design methods
If you start working with this project template, you will notice that it contains several styles and html tags.
I will introduce my coding method from here on.
If you already have good way, Please delete my codes.

I extend the components of Bootstrap a lot.
However contrary to Bootstrap's style based on the basic OOCSS design, 
I am trying to use many BEM design way for new design parts.
We focus on separating styles in component units.
By doing so, I do not care much about this difference.


## Style Contexts
My CSS policy has 7 contexts below.
```
1.Libraries
2.Core
3.Layouts
4.Primitive Parts
5.Complex Parts
6.Pages
(7.User)
```
These are compailed from 1 to 7.
Please use this sort regulation, It will clarify the role of the design.

A description example of endpoint stylesheet is as follows. (index.scss)
Stylesheets are compiled and loaded from top to bottom.
※ `_variables.scss` is only irregular sort position.

```
// Libraries (& Override Core)
@import 'core/variables';
@import '~bootstrap/scss/bootstrap';

// Core
@import 'core/mixins';
@import 'core/utilities';

// Layouts
@import 'layouts/page';
@import 'layouts/header';
@import 'layouts/main';
@import 'layouts/footer';

// Primitive Parts
@import 'parts-primitive/tmp-parts';

// Complex Parts
@import 'parts-complex/tmp-ui';

// Page
@import 'pages/tmp-page';
```


### 1.Libraries
Libraries context is used for external css libraries.
I have added Bootstrap4 to here already.
Please don't touch the libraries code. Also manage the libraries as package as much as possible.

### 2.Core
These stylesheets manage `variables`, `mixins` and `utility classes`.
Also these include some override and extend class of Bootstrap.

#### `_variables.scss`
The file defines global variables. Also, the file includes overrides of Bootstrap.
Definitions of Bootstrap almost use default flag.
Therefore, this file will be prefetched form.

#### `_mixins.scss`
The file defines mixins.

#### `_utilities.scss`
The file includes global utility classes. The classes usually have `!important` flag.
Bootstrap has many utility classes.
I added something not included in Bootstrap utilities file to here.


### 3.Layouts
WEBサイトを構成する最も基礎となるレイアウトコンテナに関するスタイルを記します。
WEBサイト全てのコンテナに共通させたいことを`_page.scss` へ、
サイトヘッダーのレイアウトや内部のパーツのための記述を`_header.scss`へ、
サイトフッターのレイアウトや内部のパーツのための記述を`_footer.scss`へ行います。
また、
ヘッダーとフッターを除く、サイトコンテンツ部分のラップ要素のために`_main.scss`などといった名前のものを用意します。
注意したいことは、これらは「自身のレイアウト」及び「内包する要素共通のデザインレギュレーション」を定義することが主な役割だという点です。
そのため、各コンテナに内包されるデザインパーツたちは後述する4〜7のコンテクストで定義することがが可能です。
ただし、headerやfooterなど他に重複が見られないパーツに関しては、このLayoutsコンテキストで定義してしまうことも可です。

以下は、そのイメージです。
```
[ html ]

<div class="Page">
  <header class="Header">
    <div class="header-foo">...</div>
  </header>
  <main class="Main">
    <div class="foo-parts">...</div>
    <div class="bar-parts">...</div>
  </main>
  <footer class="Footer">
    <div class="site-footer-foo">...</div>
  </footer>
</div>
```

```
[ _page.scss ]

// webサイト全体に関わることを定義する

* { ... }
div { ... }
a { ... }

```

```
[ _header.scss ]

// サイトヘッダー全体に関わることを定義する
Header { ... }
Header * { ... }

// サイトフッター内のブロックに関して定義
// 4〜7のコンテクストで定義しても良いが他に重複を見ない要素のケースが多いのでこちらに記述
header-foo { ... }

```

```
[ _footer.scss ]

// サイトフッター全体に関わることを定義する
Footer { ... }
Footer * { ... }

// サイトフッター内のブロックに関して定義
// 4〜7のコンテクストで定義しても良いが他に重複を見ない要素のケースが多いのでこちらに記述
footer-foo { ... }

```

```
[ _main.scss ]

// メイン（このWEBサイトのコンテンツ）全体に関わることを定義する
Main { ... }
Main * { ... }

// 内包するパーツは4〜7のコンテクストで定義する方針が良い
```

### 4.Primitive Parts
デザイン上、最小単位となるエレメントをパーツ単位にstylesheetにして切り分けます。`_button.scss`, `_table.scss`, `_heading.scss`などが該当します。
Bootstrapが本来持っているcomponentを上書きするケースも頻発します。

### 5.Complex Parts
比較的複合的なパーツを切り分けたい時に利用します。
`_food-image-gallery-carousel.scss`や `_for-new-user-modal.scss`のように「carousel」や「modal」などと具体的なパーツの種類を示してあげるとより明確です。
また、`_breadcrumbs.scss`, `_pagination.scss` など、一般的なWEBサイトにもよく登場する複合的なパーツも
このコンテクストで整理するのが良さそうです。
Complex PartsはPrimitive Partsを含むことが可能です。

### 6.Pages
「ページ」といった単位でデザインを考えたい時に利用します。
しかしながら、ほとんどの場合は、Bootstrapの持つコンポーネント及び、1〜5のコンテクスト、そして、後述するBEM記法によるモディファイア表現により、欲求のほとんどが満たされてしまいます。

しかしながらあえてページ単位にデザインをまとめたい場合、また、デザインの方向性が見えないパーツなどをとりあえず配置するにはうってつけの場所です。HTML5によりDOMエレメントの定義が革新される以前のコーディングを知る方にはこの考え方はむしろ馴染みのあるものではないでしょうか。

### 7.User
まず初めにこのコンテクストはできる限り利用しないことが好ましいです。
このコンテクストは、「このデザイン設計を共有できない状態」が生まれた場合にのみ利用します。

仮に、制作したWEBサイトがローンチした後、クライアント先のWEB担当がサイトを引き継ぎ、管理することを想像してください。
彼は独自の方法でマークアップを行いたいと考えています。また彼の日常業務は非常に軽微なデザインの修正です。
そのため、大きな改修タスクが新たに発生すると、WEBサイトを制作した、あなたの出番がまたおとれます。

例えばこういった場合の、橋渡しに利用されるのがこのコンテクストです。
実際は単純にCSSのシートを分けるというだけの話です。
```
[ HTML ]

<link rel="stylesheet" href="/css/bundle.css">
<link rel="stylesheet" href="/css/user.css">

<div class="foo">The div was red. The div is blue.</div>
<div class="bar" id="bar">The div was big. The div is small.</div>
```

通常全てのコンテクストスタイルシートは1枚のcssファイルにバンドルされますが
Userはそこに含まれません。
```
[ bundle.css ]

.foo { color: red; }
.bar { width: 100px; }
```

```
[ user.css ]

.foo { color: blue }
#bar { width: 500px; }
```

またファイルのロード順でもわかるように、user.cssは、bundle.cssをオーバーライドできる存在です。
このようにuser.cssは「一時的なもの」「緊急の作業」「取り急ぎの処置」などといった目的にも有効です。

さらに、1〜6のコンテクストのサンプルコードの中に、IDを用いたスタイルの指定がないことにも注目してください。
サンプルには使用意図が明確なutilities.scssに含まれるimport構文を用いたクラス指定以外に強力なスタイル指定は存在しません。
これらもuserに確保された強制力の一つです。WEBサイトを一つのサービスと捉えたら1〜6のコンテクストはインフラで7はユーザー設定と言えるでしょう。

※上記では、便宜上スタイルシートを増やすという形でこのuserの概念を提示しましたが、
これも含めて1枚のスタイルシートにバンドルできる環境がある場合はそれがもっとも好ましいです。


## スタイル方法
基本的にCSSのclass名にはBEM記法を利用します。