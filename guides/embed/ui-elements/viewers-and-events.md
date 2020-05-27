---
rank: 13
hide_in_page_nav: true
related_endpoints: []
related_guides:
  - embed/ui-elements
required_guides:
  - embed/ui-elements/installation
related_resources: []
alias_paths:
  - /docs/file-types-events
category_id: embed
subcategory_id: embed/ui-elements
is_index: false
id: embed/ui-elements/viewers-and-events
type: guide
total_steps: 14
sibling_id: embed/ui-elements
parent_id: embed/ui-elements
next_page_id: embed/ui-elements/custom-domains
previous_page_id: embed/ui-elements/scopes
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/embed/ui-elements/viewers-and-events.md
---
<!-- alex disable black -->

# プレビュー - ビューアーとイベント

<!-- markdownlint-disable no-duplicate-header -->

このページでは、ファイルタイプごとのプレビュー機能について説明します。また、ビューアーの種類別にリッスンできるイベントのリストも示します。

## テキストビューアー

テキストビューアーでは、テキストファイルのプレビューがレンダリングされます。`py`や`rb`のようなコードファイルの場合は、構文の強調表示を追加するために[`highlight.js`](https://github.com/isagalaev/highlight.js)が使用されます。

### 動作

テキストビューアーには、ファイル内のテキストのうち最初の192KBが表示されます。その他のテキストは省略され、通知とダウンロードボタンがプレビューの最下部に追加されます。

ビューアーウィンドウのサイズを変更すると、使用可能なスペースに収まるようテキストの再流し込みが行われます。また、拡大ボタンと縮小ボタンにより、フォントサイズがそれぞれ縮小または拡大されます。

このビューアーでは印刷がサポートされており、`print()`が呼び出されるか印刷ボタンが押されると、適切な構文を強調表示した状態で印刷が行われます。サイズの大きなファイルを印刷すると、一部のブラウザでは数秒間動作が停止する場合があります。

### コントロール

* 拡大
* 縮小
* 全画面: Escキーを押すと終了可能

### サポートされているファイル拡張子

`as`, `as3as`, `asmas`, `batas`, `cas`, `ccas`, `cmakeas`, `cppas`, `csas`,
`cssas`, `cxxas`, `diffas`, `erbas`, `groovyas`, `has`, `hamlas`, `hhas`,
`htmas`, `htmlas`, `javaas`, `jsas`, `lessas`, `mas`, `makeas`, `mdas`, `mlas`,
`mmas`, `phpas`, `plas`, `plistas`, `propertiesas`, `pyas`, `rbas`, `rstas`,
`sassas`, `scalaas`, `scriptas`, `scmas`, `smlas`, `sqlas`, `shas`, `vias`,
`vimas`, `webdocas`, `xhtmlas`, `yaml`,

### イベント

テキストビューアーでは、以下のイベントがトリガーされます。

<!-- markdownlint-disable line-length -->

| イベント名          | 説明                                         | イベントデータ                                                |
| -------------- | ------------------------------------------ | ------------------------------------------------------ |
| `destroy`      | The preview is intentionally destroyed     |                                                        |
| `load`         | The preview loads                          | 1. `{string}` **`error`** (省略可): エラーメッセージ              |
|                |                                            | 2. `{object}` **`file`**: 現在のファイル                      |
|                |                                            | 3. `{object}` **`metrics`**: ロガーからの情報                  |
|                |                                            | 4. `{object}` **`viewer`**: 現在のビューアー                   |
| `notification` | A notification is displayed                |                                                        |
| `navigate`     | The preview is shown for a given index     | `{object}`ファイル                                         |
| `reload`       | The preview reloads                        |                                                        |
| `resize`       | The preview resizes                        | 1. `{number}` **`height`**: ウィンドウの高さ                   |
|                |                                            | 2. `{number}` **`width`**: ウィンドウの幅                     |
| `zoom`         | The preview zooms in or out                | 1. `{number}` **`zoom`**: 新しい拡大/縮小値                    |
|                |                                            | 2. `{boolean}` **`canZoomIn`**: trueにするとビューアーをさらに拡大可能  |
|                |                                            | 3. `{boolean}` **`canZoomOut`**: trueにするとビューアーをさらに縮小可能 |
| `printsuccess` | An attempt to print triggered successfully |                                                        |

<!-- markdownlint-enable line-length -->

## 360度動画ビューアー

360度動画ビューアーでは、正距円筒図法を使用して保存された動画(ほとんどの場合に360度カメラで撮影)のプレビューがレンダリングされます。

### 動作

このビューアーでは、360度動画がインタラクティブに表示されます。

### コントロール

* マウスの左ボタンで表示方向を変更します(タッチ対応デバイスでは1回タッチ)。

### VRボタン

WebVRをサポートするブラウザの使用時に、対応するVRデバイスがコンピュータに接続されると、VRボタンを使用して、VRモードの開始と終了を切り替えることができます。

### 制限

現在、このプレビューアーを使用するには、ファイルの名前を付ける際にファイル拡張子の前に`.360`を付ける必要があります。こうすることで、Preview SDKは、標準の動画プレビューではなく、このビューアーを実行することを認識します。

### サポートされているファイル拡張子

`360.3g2`, `360.3gp`, `360.avi`, `360.m2v`, `360.m2ts`, `360.m4v`, `360.mkv`,
`360.mov`, `360.mp4`, `360.mpeg`, `360.mpg`, `360.mts`, `360.qt`, `360.wmv`

### イベント

360度動画ビューアーでは、以下のイベントがトリガーされます。

<!-- markdownlint-disable line-length -->

| イベント名          | 説明                                     | イベントデータ                                   |
| -------------- | -------------------------------------- | ----------------------------------------- |
| `destroy`      | The preview is intentionally destroyed |                                           |
| `load`         | The preview loads                      | 1. `{string}` **`error`** (省略可): エラーメッセージ |
|                |                                        | 2. `{object}` **`file`**: 現在のファイル         |
|                |                                        | 3. `{object}` **`metrics`**: ロガーからの情報     |
|                |                                        | 4. `{object}` **`viewer`**: 現在のビューアー      |
| `notification` | A notification is displayed            |                                           |
| `navigate`     | The preview is shown for a given index | `{object}`ファイル                            |
| `reload`       | The preview reloads                    |                                           |
| `resize`       | The preview resizes                    | 1. `{number}` **`height`**: ウィンドウの高さ      |
|                |                                        | 2. `{number}` **`width`**: ウィンドウの幅        |

<!-- markdownlint-enable line-length -->

## SWFビューアー

SWFビューアーは、[`SWFObject`](https://github.com/swfobject/swfobject)を使用してSWFファイルを埋め込みます。

### 動作

ユーザーがAdobe Flash Playerプラグインを使用している場合、`SWFObject`はSWFファイルを埋め込み、このプラグインで関連するコンテンツをレンダリングできるようにします。

Flashコンテンツによるネットワークリクエストはすべて、セキュリティの制約によってブロックされるため、ネットワーク接続を必要とするコンテンツはレンダリングされないことに注意してください。

### サポートされているファイル拡張子

* `swf`

### イベント

SWFビューアーでは、以下のイベントがトリガーされます。

<!-- markdownlint-disable line-length -->

| イベント名          | 説明                                     | イベントデータ                                   |
| -------------- | -------------------------------------- | ----------------------------------------- |
| `destroy`      | The preview is intentionally destroyed |                                           |
| `load`         | The preview loads                      | 1. `{string}` **`error`** (省略可): エラーメッセージ |
|                |                                        | 2. `{object}` **`file`**: 現在のファイル         |
|                |                                        | 3. `{object}` **`metrics`**: ロガーからの情報     |
|                |                                        | 4. `{object}` **`viewer`**: 現在のビューアー      |
| `notification` | A notification is displayed            |                                           |
| `navigate`     | The preview is shown for a given index | `{object}`ファイル                            |
| `reload`       | The preview reloads                    |                                           |
| `resize`       | The preview resizes                    | 1. `{number}` **`height`**: ウィンドウの高さ      |
|                |                                        | 2. `{number}` **`width`**: ウィンドウの幅        |

<!-- markdownlint-enable line-length -->

## プレゼンテーションビューアー

プレゼンテーションビューアーでは、PowerPointファイルのプレビューがレンダリングされます。

### 動作

プレゼンテーションビューアーには、プレビューを閉じるときに表示していたスライドが記憶されます。次回そのファイルを開くと、すぐにそのスライドが表示されます。マウスで上下にスクロールするか、モバイルデバイスで上下にスワイプすると、スライド間を移動します。ビューアーを拡大または縮小すると、スライドのサイズもそれぞれ拡大または縮小されます。ズームレベルが原因でコンテンツがはみ出す場合は、マウスをスクロールすると、スライドをスクロールできます。通常のスクロール動作に戻すには、はみ出さなくなるまで縮小する必要があります。

### コントロール

* 拡大
* 縮小
* ページの設定: 上矢印と下矢印を使用するか、ページ番号をクリックしてテキストを入力します。
* 全画面: Escキーを押すと終了可能

### サポートされているファイル拡張子

`ppt`, `pptx`, `odp`

### オプション

<!-- markdownlint-disable line-length -->

| オプション         | 型       | 説明                                      |
| ------------- | ------- | --------------------------------------- |
| `annotations` | boolean | 省略可。コンテンツの注釈を表示するかどうか。デフォルト値は`false`です。 |

<!-- markdownlint-enable line-length -->

### イベント

プレゼンテーションビューアーでは、以下のイベントがトリガーされます。

<!-- markdownlint-disable line-length -->

| イベント名          | 説明                                     | イベントデータ                                               |
| -------------- | -------------------------------------- | ----------------------------------------------------- |
| `destroy`      | The preview is intentionally destroyed |                                                       |
| `load`         | The preview loads                      | 1. `string` **`error`** (省略可): エラーメッセージ               |
|                |                                        | 2. `object` **`file`**: 現在のファイル                       |
|                |                                        | 3. `object` **`metrics`**: ロガーからの情報                   |
|                |                                        | 4. `object` **`viewer`**: 現在のビューアー                    |
| `notification` | A notification is displayed            |                                                       |
| `navigate`     | The preview is shown for a given index | `object`ファイル                                          |
| `reload`       | The preview reloads                    |                                                       |
| `resize`       | The preview resizes                    | 1. `number` **`height`**: ウィンドウの高さ                    |
|                |                                        | 2. `number` **`width`**: ウィンドウの幅                      |
| `zoom`         | The preview zooms in or out            | 1. `number` **`zoom`**: 新しい拡大/縮小値                     |
|                |                                        | 2. `boolean` **`canZoomIn`**: trueにするとビューアーをさらに拡大可能   |
|                |                                        | 3. `boolean` **`canZoomOut`**: trueにするとビューアーをさらに縮小可能  |
| `pagerender`   | A page renders                         | `number` レンダリングされたページのページ番号                           |
| `pagefocus`    | A page is visible                      | `number` フォーカスされたページのページ番号                            |
| `scrollstart`  | The viewer starts to scroll            | 1. `number` **`scrollTop`**: ビューポートの上部からスクロールしたピクセル数  |
|                |                                        | 2. `number` **`scrollLeft`**: ビューポートの左側からスクロールしたピクセル数 |
| `scrollend`    | The viewer stops scrolling             | 1. `number` **`scrollTop`**: ビューポートの上部からスクロールしたピクセル数  |
|                |                                        | 2. `number` **`scrollLeft`**: ビューポートの左側からスクロールしたピクセル数 |

<!-- markdownlint-enable line-length -->

## MP4ビューアー

MP4ビューアーでは、動画ファイルのプレビューがレンダリングされます。

### 動作

MP4ビューアーは、黒い背景を使用して見やすくしています。音量は、音量アイコンをクリックしてミュートまたはミュート解除したり、音量スクラバをドラッグして変更したりできます。動画の位置は、再生スクラバをクリックまたはドラッグして変更できます。

### コントロール

* 再生/一時停止
* 音量
* 設定
* 全画面(Escキーを押すと終了可能)

### 設定

設定は、プレビューツールバーにある歯車アイコンから使用できます。

* 動画速度の値: `0.25`、`0.5`、標準(`1`)、`1.25`、`1.5`、`2.0`

### サポートされているファイル拡張子

`3g2`, `3gp`, `avi`, `m2v`, `m2ts`, `m4v`, `mkv`, `mov`, `mp4`, `mpeg`, `mpg`,
`ogg`, `mts`, `qt`, `wmv`

### イベント

MP4ビューアーでは、以下のイベントがトリガーされます。

<!-- markdownlint-disable line-length -->

| イベント名          | 説明                                     | イベントデータ                                 |
| -------------- | -------------------------------------- | --------------------------------------- |
| `destroy`      | The preview is intentionally destroyed |                                         |
| `load`         | The preview loads                      | 1. `string` **`error`** (省略可): エラーメッセージ |
|                |                                        | 2. `object` **`file`**: 現在のファイル         |
|                |                                        | 3. `object` **`metrics`**: ロガーからの情報     |
|                |                                        | 4. `object` **`viewer`**: 現在のビューアー      |
| `notification` | A notification is displayed            |                                         |
| `navigate`     | The preview is shown for a given index | `object`ファイル                            |
| `reload`       | The preview reloads                    |                                         |
| `resize`       | The preview resizes                    | 1. `number` **`height`**: ウィンドウの高さ      |
|                |                                        | 2. `number` **`width`**: ウィンドウの幅        |
| `speedchange`  | Media speed changes                    | `string` 再生速度                           |
| `play`         | The video plays                        |                                         |
| `pause`        | The video pauses                       |                                         |
| `seek`         | The video skips to a time              | `number` 時刻                             |

<!-- markdownlint-enable line-length -->

## MP3ビューアー

MP3ビューアーでは、オーディオファイルのプレビューが表示されます。

### 動作

音量は、音量アイコンをクリックしてミュートまたはミュート解除したり、音量スクラバをドラッグして変更したりできます。音声の位置は、再生スクラバをクリックまたはドラッグして変更できます。

### コントロール

* 再生/一時停止
* 音量
* 設定

### 設定

設定は、プレビューツールバーにある歯車アイコンから使用できます。

* 音声速度: `0.25`、`0.5`、標準(`1`)、`1.25`、`1.5`、`2.0`

### サポートされているファイル拡張子

`aac`, `aif`, `aifc`, `aiff`, `amr`, `au`, `flac`, `m4a`, `mp3`, `ra`, `wav`, `wma`

### イベント

MP3ビューアーでは、以下のイベントがトリガーされます。

<!-- markdownlint-disable line-length -->

| イベント名          | 説明                                     | イベントデータ                                 |
| -------------- | -------------------------------------- | --------------------------------------- |
| `destroy`      | The preview is intentionally destroyed |                                         |
| `load`         | The preview loads                      | 1. `string` **`error`** (省略可): エラーメッセージ |
|                |                                        | 2. `object` **`file`**: 現在のファイル         |
|                |                                        | 3. `object` **`metrics`**: ロガーからの情報     |
|                |                                        | 4. `object` **`viewer`**: 現在のビューアー      |
| `notification` | A notification is displayed            |                                         |
| `navigate`     | The preview is shown for a given index | `object`ファイル                            |
| `reload`       | The preview reloads                    |                                         |
| `resize`       | The preview resizes                    | 1. `number` **`height`**: ウィンドウの高さ      |
|                |                                        | 2. `number` **`width`**: ウィンドウの幅        |
| `speedchange`  | Media speed changes                    | `string` 再生速度                           |
| `play`         | The video plays                        |                                         |
| `pause`        | The video pauses                       |                                         |
| `seek`         | The video skips to a time              | `number` 時刻                             |

<!-- markdownlint-enable line-length -->

## Officeビューアー

Officeビューアーでは、MicrosoftのOffice Onlineビューアーの`<iframe>`を埋め込むことで、Microsoft Officeドキュメントのプレビューをレンダリングします。

### 動作

現在、Officeビューアーでは、Boxウェブアプリ内からMicrosoft Office Onlineを使用したExcelファイルのプレビューがサポートされています。プラットフォームのユースケースおよび他のOfficeファイル形式のサポートは開発中です。

現時点では、次のように、いくつかの制限があります。

* ファイルはダウンロード可能にする必要があります。
* ファイルのサイズは5MB未満にする必要があります。
* ファイルは、パスワード付きのBox共有リンクで共有できません(パスワードのない共有リンクは問題ありません)。

### サポートされているファイル拡張子

`xlsx`

### イベント

Officeビューアーでは、以下のイベントがトリガーされます。

<!-- markdownlint-disable line-length -->

| イベント名          | 説明                                     | イベントデータ                                   |
| -------------- | -------------------------------------- | ----------------------------------------- |
| `destroy`      | The preview is intentionally destroyed |                                           |
| `load`         | The preview loads                      | 1. `{string}` **`error`** (省略可): エラーメッセージ |
|                |                                        | 2. `{object}` **`file`**: 現在のファイル         |
|                |                                        | 3. `{object}` **`metrics`**: ロガーからの情報     |
|                |                                        | 4. `{object}` **`viewer`**: 現在のビューアー      |
| `notification` | A notification is displayed            |                                           |
| `navigate`     | The preview is shown for a given index | `{object}`ファイル                            |
| `reload`       | The preview reloads                    |                                           |
| `resize`       | The preview resizes                    | 1. `{number}` **`height`**: ウィンドウの高さ      |
|                |                                        | 2. `{number}` **`width`**: ウィンドウの幅        |

<!-- markdownlint-enable line-length -->

## Markdownビューアー

Markdownビューアーでは、[`Remarkable`](https://github.com/jonschlinkert/remarkable)を使用してマークダウンファイルと[`highlight.js`](https://github.com/isagalaev/highlight.js)を解析し、ビューアーに表示されるコードブロックに構文の強調表示を追加します。

### 動作

Markdownビューアーでは、ファイル内の未加工のマークダウンのうち最初の192KBを解析し、GitHubのMarkdownスタイルを使用してレンダリングします。その多のコンテンツは省略され、ダウンロードボタンとともに通知がプレビューの最下部に追加されます。

このビューアーは、テーブル、構文の強調表示、自動URLリンクなど、[GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/)をサポートします。

ビューアーウィンドウのサイズを変更すると、使用可能なスペースに収まるようマークダウンの再流し込みが行われます。また、このビューアーでは印刷がサポートされており、解析されたマークダウンが印刷されます。さらに、`print()`が呼び出されるか印刷ボタンが押されると、コード上で構文が強調表示されます。サイズの大きなファイルを印刷すると、一部のブラウザでは数秒間動作が停止する場合があります。

### コントロール

* 全画面(Escキーを押すと終了可能)

### サポートされているファイル拡張子

`md`

### イベント

Markdownビューアーでは、以下のイベントがトリガーされます。

<!-- markdownlint-disable line-length -->

| イベント名          | 説明                                         | イベントデータ                                   |
| -------------- | ------------------------------------------ | ----------------------------------------- |
| `destroy`      | The preview is intentionally destroyed     |                                           |
| `load`         | The preview loads                          | 1. `{string}` **`error`** (省略可): エラーメッセージ |
|                |                                            | 2. `{object}` **`file`**: 現在のファイル         |
|                |                                            | 3. `{object}` **`metrics`**: ロガーからの情報     |
|                |                                            | 4. `{object}` **`viewer`**: 現在のビューアー      |
| `notification` | A notification is displayed                |                                           |
| `navigate`     | The preview is shown for a given index     | `{object}`ファイル                            |
| `reload`       | The preview reloads                        |                                           |
| `resize`       | The preview resizes                        | 1. `{number}` **`height`**: ウィンドウの高さ      |
|                |                                            | 2. `{number}` **`width`**: ウィンドウの幅        |
| `printsuccess` | An attempt to print triggered successfully |                                           |

<!-- markdownlint-enable line-length -->

## `Model3D`ビューアー

`Model3D`ビューアーでは、3Dモデルファイルのプレビューがレンダリングされるほか、各種レンダリングモードを有効にして、ワイヤフレームやテクスチャ座標など、モデルのさまざまな側面を調査できます。また、アニメーションデータは、それを含むファイル(`box3d`、`fbx`、`dae`など)でサポートされています。

### 動作

`Model3D`ビューアーでは、モデルがインタラクティブに表示されます。マウスの左ボタンを使用すると、モデルを回転させることができます(タッチ対応デバイスでは1回タッチ)。モデルの任意の場所をクリックすると、軌道のフォーカスを変更できます。

### コントロール

* マウスホイールを使用(またはタッチ対応デバイスの場合は2本指でスクロール)して拡大/縮小(モデルまでの距離を変更)。
* マウスの右ボタンを使用(またはタッチ対応デバイスの場合は3本指でスワイプ)して平行移動(横方向の移動)。
* アニメーションの選択: 表示中のモデルにアニメーションが含まれている場合、ツールバーには2つのアニメーションボタンが表示されます。1つはアニメーションを再生/一時停止できるボタン、もう1つは現在のアニメーションを選択できるボタンです。
* VRボタン: WebVRをサポートするブラウザを使用していて、対応するVRデバイスがコンピュータに接続された場合、VRボタンを使用すると、VRモードの開始と終了を切り替えることができます。

### 設定

設定は、プレビューツールバーにある歯車アイコンから使用できます。

* レンダリングモード: ライティングあり、ライティングなし、標準、シェイプ、UVオーバーレイ
* ワイヤフレームの切り替え
* スケルトンの切り替え
* カメラ投影: 透視投影、平行投影
* レンダリング品質: 自動、詳細
* モデルの回転: X、Y、Z

## `Box3D`パッケージ

プレビューを使用すると、ユーザーはBox内に1つのファイルを表示できるため、デフォルトでは、モデルでテクスチャを表示できません。ただし、Boxウェブアプリを使用すると、ユーザーは、Box3Dパッケージを作成できます。このパッケージでは、依存するすべてのファイルが、共有およびプレビュー可能な1つファイルに統合されます。このパッケージを作成するには、Box内でモデルファイルを右クリックし、\[3Dパッケージの作成]を選択します。Box内にある参照ファイルはすべて、作成されるパッケージに含まれます。

### サポートされているファイル拡張子

`box3d`, `fbx`, `dae`, `3ds`, `obj`, `stl`, `ply`

### イベント

`Model3D`ビューアーでは、以下のイベントがトリガーされます。

<!-- markdownlint-disable line-length -->

| イベント名          | 説明                                     | イベントデータ                                   |
| -------------- | -------------------------------------- | ----------------------------------------- |
| `destroy`      | The preview is intentionally destroyed |                                           |
| `load`         | The preview loads                      | 1. `{string}` **`error`** (省略可): エラーメッセージ |
|                |                                        | 2. `{object}` **`file`**: 現在のファイル         |
|                |                                        | 3. `{object}` **`metrics`**: ロガーからの情報     |
|                |                                        | 4. `{object}` **`viewer`**: 現在のビューアー      |
| `notification` | A notification is displayed            |                                           |
| `navigate`     | The preview is shown for a given index | `{object}`ファイル                            |
| `reload`       | The preview reloads                    |                                           |
| `resize`       | The preview resizes                    | 1. `{number}` **`height`**: ウィンドウの高さ      |
|                |                                        | 2. `{number}` **`width`**: ウィンドウの幅        |

<!-- markdownlint-enable line-length -->

## 360度画像ビューアー

360度画像ビューアーでは、正距円筒図法を使用して保存された画像(ほとんどの場合に360度カメラで撮影)のプレビューがレンダリングされます。

### 動作

このビューアーでは、360度画像がインタラクティブに表示されます。まず、低解像度バージョンの画像が読み込まれ、フル解像度の画像の読み込みが終了するまで簡易表示されます。マウスの左ボタンでクリックしてドラッグすると、表示方向が変更されます(タッチ対応デバイスでは1回タッチしてドラッグ)。

### コントロール

* 全画面(Escキーを押すと終了可能)
* VRボタン: WebVRをサポートするブラウザの使用時に、対応するVRデバイスがコンピュータに接続されると、VRボタンを使用して、VRモードの開始と終了を切り替えることができます。

### 制限

現在、このプレビューアーを使用するには、ファイルの名前を付ける際にファイル拡張子の前に「.360」を付ける必要があります。こうすることで、Preview SDKは、標準の画像ビューアーではなく、このビューアーを実行することを認識します。

### サポートされているファイル拡張子

`360.jpg`, `360.jpeg`, `360.png`, `360.ai`, `360.bmp`, `360.dcm`, `360.eps`,
`360.gif`, `360.ps`, `360.psd`, `360.svg`, `360.svs`, `360.tga`, `360.tif`,
`360.tiff`

### イベント

360度画像ビューアーでは、以下のイベントがトリガーされます。

<!-- markdownlint-disable line-length -->

| イベント名          | 説明                                     | イベントデータ                                   |
| -------------- | -------------------------------------- | ----------------------------------------- |
| `destroy`      | The preview is intentionally destroyed |                                           |
| `load`         | The preview loads                      | 1. `{string}` **`error`** (省略可): エラーメッセージ |
|                |                                        | 2. `{object}` **`file`**: 現在のファイル         |
|                |                                        | 3. `{object}` **`metrics`**: ロガーからの情報     |
|                |                                        | 4. `{object}` **`viewer`**: 現在のビューアー      |
| `notification` | A notification is displayed            |                                           |
| `navigate`     | The preview is shown for a given index | `{object}`ファイル                            |
| `reload`       | The preview reloads                    |                                           |
| `resize`       | The preview resizes                    | 1. `{number}` **`height`**: ウィンドウの高さ      |
|                |                                        | 2. `{number}` **`width`**: ウィンドウの幅        |

<!-- markdownlint-enable line-length -->

## 画像ビューアー

画像ビューアーでは、画像ファイルのプレビューがレンダリングされます。

### 動作

ビューアーを回転させると、画像が右へ90度回転します。デフォルトのズームレベルでは、画像をクリックすると1回拡大されます。拡大されたドキュメントをクリックすると、デフォルトのズームレベルに戻ります。縮小されたドキュメントをクリックすると、元のズームレベルになるまで拡大されます。

### コントロール

* 拡大
* 縮小
* 回転
* 全画面: Escキーを押すと終了可能

### サポートされているファイル拡張子

`ai`, `bmp`, `dcm`, `eps`, `gif`, `png`, `ps`, `psd`, `svs`, `tga`

### オプション

<!-- markdownlint-disable line-length -->

| オプション         | 型       | 説明                                      |
| ------------- | ------- | --------------------------------------- |
| `annotations` | boolean | 省略可。コンテンツの注釈を表示するかどうか。デフォルト値は`false`です。 |

<!-- markdownlint-enable line-length -->

### イベント

画像ビューアーでは、以下のイベントがトリガーされます。

<!-- markdownlint-disable line-length -->

| イベント名          | 説明                                         | イベントデータ                                                |
| -------------- | ------------------------------------------ | ------------------------------------------------------ |
| `destroy`      | The preview is intentionally destroyed     |                                                        |
| `load`         | The preview loads                          | 1. `{string}` **`error`** (省略可): エラーメッセージ              |
|                |                                            | 2. `{object}` **`file`**: 現在のファイル                      |
|                |                                            | 3. `{object}` **`metrics`**: ロガーからの情報                  |
|                |                                            | 4. `{object}` **`viewer`**: 現在のビューアー                   |
| `notification` | A notification is displayed                |                                                        |
| `navigate`     | The preview is shown for a given index     | `{object}`ファイル                                         |
| `reload`       | The preview reloads                        |                                                        |
| `resize`       | The preview resizes                        | 1. `{number}` **`height`**: ウィンドウの高さ                   |
|                |                                            | 2. `{number}` **`width`**: ウィンドウの幅                     |
| `zoom`         | The preview zooms in or out                | 1. `{number}` **`zoom`**: 新しい拡大/縮小値                    |
|                |                                            | 2. `{boolean}` **`canZoomIn`**: trueにするとビューアーをさらに拡大可能  |
|                |                                            | 3. `{boolean}` **`canZoomOut`**: trueにするとビューアーをさらに縮小可能 |
| `pan`          | The preview is panning                     |                                                        |
| `panstart`     | Panning starts                             |                                                        |
| `panend`       | Panning ends                               |                                                        |
| `rotate`       | The image rotates                          |                                                        |
| `printsuccess` | An attempt to print triggered successfully |                                                        |

<!-- markdownlint-enable line-length -->

## 複数画像ビューアー

複数画像ビューアーでは、複数画像のファイル(`tiff`、`tif`)のプレビューがレンダリングされます。

### 動作

デフォルトのズームレベルでは、画像をクリックすると1回拡大されます。拡大されたドキュメントをクリックすると、デフォルトのズームレベルに戻ります。縮小されたドキュメントをクリックすると、元のズームレベルになるまで拡大されます。

### コントロール

* 拡大
* 縮小
* 全画面: Escキーを押すと終了可能
* ページの設定: 上矢印と下矢印を使用するか、ページ番号をクリックしてテキストを入力します。

### サポートされているファイル拡張子

`tif`, `tiff`

### オプション

<!-- markdownlint-disable line-length -->

| オプション         | 型       | 説明                                      |
| ------------- | ------- | --------------------------------------- |
| `annotations` | boolean | 省略可。コンテンツの注釈を表示するかどうか。デフォルト値は`false`です。 |

<!-- markdownlint-enable line-length -->

### イベント

画像ビューアーでは、以下のイベントがトリガーされます。

<!-- markdownlint-disable line-length -->

| イベント名          | 説明                                     | イベントデータ                                                |
| -------------- | -------------------------------------- | ------------------------------------------------------ |
| `destroy`      | The preview is intentionally destroyed |                                                        |
| `load`         | The preview loads                      | 1. `{string}` **`error`** (省略可): エラーメッセージ              |
|                |                                        | 2. `{object}` **`file`**: 現在のファイル                      |
|                |                                        | 3. `{object}` **`metrics`**: ロガーからの情報                  |
|                |                                        | 4. `{object}` **`viewer`**: 現在のビューアー                   |
| `notification` | A notification is displayed            |                                                        |
| `navigate`     | The preview is shown for a given index | `{object}`ファイル                                         |
| `reload`       | The preview reloads                    |                                                        |
| `resize`       | The preview resizes                    | 1. `{number}` **`height`**: ウィンドウの高さ                   |
|                |                                        | 2. `{number}` **`width`**: ウィンドウの幅                     |
| `zoom`         | The preview zooms in or out            | 1. `{number}` **`zoom`**: 新しい拡大/縮小値                    |
|                |                                        | 2. `{boolean}` **`canZoomIn`**: trueにするとビューアーをさらに拡大可能  |
|                |                                        | 3. `{boolean}` **`canZoomOut`**: trueにするとビューアーをさらに縮小可能 |
| `pagefocus`    | `{number}` フォーカスされたページのページ番号           |                                                        |
| `pan`          | The preview is panning                 |                                                        |
| `panstart`     | Panning starts                         |                                                        |
| `panend`       | Panning ends                           |                                                        |

<!-- markdownlint-enable line-length -->

### メソッド

複数ページの画像ビューアーでは以下のメソッドを使用できます。

<!-- markdownlint-disable line-length -->

| メソッド名              | 説明                             | メソッドのパラメータ                       |
| ------------------ | ------------------------------ | -------------------------------- |
| `zoom`             | Zooms the image                | `{string}` 「in」、「out」、または「reset」 |
| `previousPage`     | Navigates to the previous page |                                  |
| `nextPage`         | Navigates to the next page     |                                  |
| `setPage`          | Navigates to a given page      | `{number}` ページ番号                 |
| `toggleFullscreen` | Toggles full screen mode       |                                  |

<!-- markdownlint-disable line-length -->

## ドキュメントビューアー

ドキュメントビューアーでは、さまざまなドキュメントタイプのプレビューがレンダリングされます。

### 動作

ドキュメントビューアーには、プレビューを閉じるときに表示していたページが記憶されます。次回そのファイルを開くと、すぐにそのページが表示されます。ビューアーウィンドウのサイズを変更すると、ドキュメントのサイズが変更されます。

### コントロール

* 拡大
* 縮小
* ページの設定: 上矢印と下矢印を使用するか、ページ番号をクリックしてテキストを入力します。
* 全画面(Escキーを押すと終了可能)

### サポートされているファイル拡張子

`as`, `as3`, `asm`, `bat`, `c`, `cc`, `cmake`, `cpp`, `cs`, `css`, `csv`, `cxx`,
`diff`, `doc`, `docx`, `erb`, `gdoc`, `groovy`, `gsheet`, `h`, `haml`, `hh`,
`htm`, `html`, `java`, `js`, `less`, `log`, `m`, `make`, `md`, `ml`, `mm`,
`msg`, `odp`, `ods`, `odt`, `pdf`, `php`, `pl`, `plist`, `ppt`, `pptx`,
`properties`, `py`, `rb`, `rst`, `rtf`, `sass`, `scala`, `scm`, `script`, `sh`,
`sml`, `sql`, `tsv`, `txt`, `vi`, `vim`, `webdoc`, `wpd`, `xhtml`, `xls`,
`xlsm`, `xlsx`, `xml`, `xsd`, `xsl`, `yaml`

### オプション

<!-- markdownlint-disable line-length -->

| オプション         | 型       | 説明                                      |
| ------------- | ------- | --------------------------------------- |
| `annotations` | boolean | 省略可。コンテンツの注釈を表示するかどうか。デフォルト値は`false`です。 |

<!-- markdownlint-enable line-length -->

### イベント

ドキュメントビューアーでは、以下のイベントがトリガーされます。

<!-- markdownlint-disable line-length -->

| イベント名          | 説明                                         | イベントデータ                                                 |
| -------------- | ------------------------------------------ | ------------------------------------------------------- |
| `destroy`      | The preview is intentionally destroyed     |                                                         |
| `load`         | The preview loads                          | 1. `{string}` **`error`** (省略可): エラーメッセージ               |
|                |                                            | 2. `{object}` **`file`**: 現在のファイル                       |
|                |                                            | 3. `{object}` **`metrics`**: ロガーからの情報                   |
|                |                                            | 4. `{object}` **`viewer`**: 現在のビューアー                    |
| `notification` | A notification is displayed                |                                                         |
| `navigate`     | The preview is shown for a given index     | `{object}`ファイル                                          |
| `reload`       | The preview reloads                        |                                                         |
| `resize`       | The preview resizes                        | 1. `{number}` **`height`**: ウィンドウの高さ                    |
|                |                                            | 2. `{number}` **`width`**: ウィンドウの幅                      |
| `zoom`         | The preview zooms in or out                | 1. `{number}` **`newScale`**: 新しい拡大/縮小値                 |
|                |                                            | 2. `{boolean}` **`canZoomIn`**: trueにするとビューアーをさらに拡大可能   |
|                |                                            | 3. `{boolean}` **`canZoomOut`**: trueにするとビューアーをさらに縮小可能  |
| `pagerender`   | A page is rendered                         | `{number}` レンダリングされたページのページ番号                           |
| `pagefocus`    | A page is visible                          | `{number}` フォーカスされたページのページ番号                            |
| `scrollstart`  | The viewer starts to scroll                | 1. `{number}` **`scrollTop`**: ビューポートの上部からスクロールしたピクセル数  |
|                |                                            | 2. `{number}` **`scrollLeft`**: ビューポートの左側からスクロールしたピクセル数 |
| `scrollend`    | The viewer stops scrolling                 | 1. `{number}` **`scrollTop`**: ビューポートの上部からスクロールしたピクセル数  |
|                |                                            | 2. `{number}` **`scrollLeft`**: ビューポートの左側からスクロールしたピクセル数 |
| `printsuccess` | An attempt to print triggered successfully |                                                         |
| `printsuccess` | An attempt to print failed                 |                                                         |

<!-- markdownlint-enable line-length -->

### 注釈

<!-- markdownlint-disable line-length -->

| イベント名                      | 説明                                                                                                                                         | イベントデータ |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ------- |
| `annotationdraw`           | Text is highlighted                                                                                                                        |         |
| `annotationcommentpending` | User hovers back into a dialog that has a pending comment                                                                                  |         |
| `annotationcreate`         | Annotation is created                                                                                                                      |         |
| `annotationcancel`         | An annotation was started but then abandoned before it was created                                                                         |         |
| `annotationdelete`         | Annotation with provided `AnnotationID` is deleted. If no ID is provided, deletes the first (and only remaining) annotation in the thread. |         |

<!-- markdownlint-enable line-length -->

## `<iframe>`ビューアー

`<iframe>`ビューアーは、外部ソースからレンダリングされたコンテンツを表示するためのフレームを埋め込みます。

### 動作

`<iframe>`ビューアーは、Box NotesおよびBox DICOMファイルのプレビューに使用されるため、これらのプレビューは現在Boxウェブアプリ内からのみ動作します。プラットフォームのお客様は、[Box DICOM Viewer](https://boxdicom.com/#viewer)を使用してAPI経由でDICOMスタディをプレビューしてください。

Box NotesとBox DICOMの両方で、メインのBoxウェブアプリ内にフル機能搭載のビューアーを備えています。ただし、このようなフル機能搭載のビューアーは、NotesやDICOMのファイルと同じディレクトリ内にある他のファイルのプレビューから移動しても初期化されません。この場合、`<iframe>`ビューアーは、Box NoteまたはBox DICOMファイルの表示専用のレンダリング機能を埋め込みます。

### サポートされているファイル拡張子

`boxnote`, `boxdicom`

### イベント

`<iframe>`ビューアーでは、以下のイベントがトリガーされます。

<!-- markdownlint-disable line-length -->

| イベント名          | 説明                                     | イベントデータ                                   |
| -------------- | -------------------------------------- | ----------------------------------------- |
| `destroy`      | The preview is intentionally destroyed |                                           |
| `load`         | The preview loads                      | 1. `{string}` **`error`** (省略可): エラーメッセージ |
|                |                                        | 2. `{object}` **`file`**: 現在のファイル         |
|                |                                        | 3. `{object}` **`metrics`**: ロガーからの情報     |
|                |                                        | 4. `{object}` **`viewer`**: 現在のビューアー      |
| `notification` | A notification is displayed            |                                           |
| `navigate`     | The preview is shown for a given index | `{object}`ファイル                            |
| `reload`       | The preview reloads                    |                                           |
| `resize`       | The preview resizes                    | 1. `{number}` **`height`**: ウィンドウの高さ      |
|                |                                        | 2. `{number}` **`width`**: ウィンドウの幅        |

<!-- markdownlint-enable line-length -->

## DASHビューアー

DASHビューアーでは、[`Shaka Player`][shaka]を使用して、動画ファイルのプレビューがレンダリングされます。

### 動作

DASHビューアーは、黒い背景を使用して、より自然な表示を実現しています。動画は、自動的に決まる初期の品質で、数バイトのチャンクに分割されてストリーミングされます。音量は、音量アイコンをクリックしてミュートまたはミュート解除したり、音量スクラバをドラッグして変更したりできます。動画の位置は、再生スクラバをクリックまたはドラッグして変更できます。

### コントロール

* 再生/一時停止
* 音量
* 設定
* 全画面(Escキーを押すと終了可能)

### 設定

設定は、プレビューツールバーにある歯車アイコンから使用できます。

* 動画速度: `0.25`、`0.5`、標準(`1`)、`1.25`、`1.5`、`2.0`
* 動画品質: `480p`、`1080p`、`auto`

### サポートされているファイル拡張子

`3g2`, `3gp`, `avi`, `m2v`, `m2ts`, `m4v`, `mkv`, `mov`, `mp4`, `mpeg`, `mpg`,
`ogg`, `mts`, `qt`, `wmv`

### イベント

DASHビューアーでは、以下のイベントがトリガーされます。

<!-- markdownlint-disable line-length -->

| イベント名              | 説明                                                            | イベントデータ                                   |
| ------------------ | ------------------------------------------------------------- | ----------------------------------------- |
| `destroy`          | The preview is intentionally destroyed                        |                                           |
| `load`             | The preview loads                                             | 1. `{string}` **`error`** (省略可): エラーメッセージ |
|                    |                                                               | 2. `{object}` **`file`**: 現在のファイル         |
|                    |                                                               | 3. `{object}` **`metrics`**: ロガーからの情報     |
|                    |                                                               | 4. `{object}` **`viewer`**: 現在のビューアー      |
| `notification`     | A notification is displayed                                   |                                           |
| `navigate`         | The preview is shown for a given index                        | `{object}`ファイル                            |
| `reload`           | The preview reloads                                           |                                           |
| `resize`           | The preview resizes                                           | 1. `{number}` **`height`**: ウィンドウの高さ      |
|                    |                                                               | 2. `{number}` **`width`**: ウィンドウの幅        |
| `speedchange`      | The media speed changes                                       | `{string}` 再生速度                           |
| `qualitychange`    | The video quality changes                                     | `{string}` メディア品質                         |
| `bandwidthhistory` | Gives bandwidth history when the preview is destroyed         | `{array}` 帯域幅情報                           |
| `switchhistory`    | Gives quality switching history when the preview is destroyed | `{array}` 品質切り替えオブジェクト                    |
| `adaptation`       | Quality adapts to a change in bandwidth                       | `{number}` 帯域幅                            |
| `play`             | The video plays                                               |                                           |
| `pause`            | The video pauses                                              |                                           |
| `seek`             | The video skips to a time                                     | `{number}` 時刻                             |

<!-- markdownlint-enable line-length -->

## CSVビューアー

CSVビューアーでは、[`PapaParse`](https://github.com/mholt/PapaParse)を使用してCSVおよびTSVファイルを解析し、[`React Virtualized`][reactv]を使用して解析後のデータを表形式で表示します。

### 動作

ビューアーウィンドウのサイズを変更すると、表のサイズが変更されます。拡大ボタンと縮小ボタンにより、フォントサイズがそれぞれ縮小または拡大されます。現在、列と行のサイズは固定されているため、はみ出すテキストは省略されます。

このビューアーは、印刷をサポートしていません。

### コントロール

* 拡大
* 縮小
* 全画面(Escキーを押すと終了可能)

### サポートされているファイル拡張子

`csv`, `tsv`

### イベント

CSVビューアーでは、以下のイベントがトリガーされます。

<!-- markdownlint-disable line-length -->

| イベント名          | 説明                                     | イベントデータ                                                |
| -------------- | -------------------------------------- | ------------------------------------------------------ |
| `destroy`      | The preview is intentionally destroyed |                                                        |
| `load`         | The preview loads                      | 1. `{string}` **`error`** (省略可): エラーメッセージ              |
|                |                                        | 2. `{object}` **`file`**: 現在のファイル                      |
|                |                                        | 3. `{object}` **`metrics`**: ロガーからの情報                  |
|                |                                        | 4. `{object}` **`viewer`**: 現在のビューアー                   |
| `notification` | A notification is displayed            |                                                        |
| `navigate`     | The preview is shown for a given index | `{object}`ファイル                                         |
| `reload`       | The preview reloads                    |                                                        |
| `resize`       | The preview resizes                    | 1. `{number}` **`height`**: ウィンドウの高さ                   |
|                |                                        | 2. `{number}` **`width`**: ウィンドウの幅                     |
| `zoom`         | The preview zooms in or out            | 1. `{number}` **`zoom`**: 新しい拡大/縮小値                    |
|                |                                        | 2. `{boolean}` **`canZoomIn`**: trueにするとビューアーをさらに拡大可能  |
|                |                                        | 3. `{boolean}` **`canZoomOut`**: trueにするとビューアーをさらに縮小可能 |

<!-- markdownlint-enable line-length -->

<!-- markdownlint-enable no-duplicate-header -->

[reactv]: https://github.com/bvaughn/react-virtualized

[shaka]: https://github.com/google/shaka-player
