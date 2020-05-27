---
rank: 5
related_endpoints: []
related_guides:
  - embed/ui-elements
required_guides:
  - embed/ui-elements/installation
related_resources: []
alias_paths:
  - /docs/box-content-picker
  - /docs/content-picker
category_id: embed
subcategory_id: embed/ui-elements
is_index: false
id: embed/ui-elements/picker
type: guide
total_steps: 14
sibling_id: embed/ui-elements
parent_id: embed/ui-elements
next_page_id: embed/ui-elements/preview
previous_page_id: embed/ui-elements/open-with
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/embed/ui-elements/picker.md
---
# Content Picker

Box Content Picker UI Elementを使用すると、開発者は、デスクトップまたはモバイルウェブアプリでBoxからファイルやフォルダを選択するためのサポートを追加できます。ライブラリは、指定されたフォルダに関する情報をBox APIを介して取得し、そのコンテンツをフォルダビューにレンダリングします。ユーザーはファイルまたはフォルダを選択でき、その後、このコンテンツ情報がアプリケーションの別の部分に渡されます。

## インストール

NPMまたはBox CDN経由でBox UI Elementsをインストールする方法は、[こちら](g://embed/ui-elements/installation)を参照してください。

<Message>

# ブラウザのサポート

古いブラウザでは、UI Elementsの[サポートは限定的](g://embed/ui-elements/browser)です。目的のブラウザに合ったpolyfillを必ず追加してください。

</Message>

## 認証

UI Elementsは認証に依存しない方法で設計されているため、Boxアカウントを持つユーザー(管理対象ユーザー)とBox以外のアカウントを持つユーザー(App User)のどちらにUI Elementsを使用するかどうかに関係なく、UI Elementsを使用するのに特別な設定は必要ありません。その理由は、UI Elementsは認証のために「トークン」を受け取ることのみを予期しており、Boxにはトークンの生成方法としてOAuthとJWTの2つがあるからです。

<CTA to="g://authentication/select">
Learn about selecting an authentication method

</CTA>

## サンプルHTML

<!-- markdownlint-disable line-length -->

```html
<!DOCTYPE html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <title>Box File Selection</title>

    <!-- polyfill.io only loads the polyfills your browser needs -->
    <script src="https://cdn.polyfill.io/v2/polyfill.min.js?features=es6,Intl"></script>
    <!-- Alternatively, use polyfill hosted on the Box CDN
    <script src="https://cdn01.boxcdn.net/polyfills/core-js/2.5.3/core.min.js"></script>
    -->

    <!-- Latest version of the picker css for your locale -->
    <link
      rel="stylesheet"
      href="https://cdn01.boxcdn.net/platform/elements/{VERSION}/en-US/picker.css"
    />
  </head>
  <body>
    <div class="container" style="height:600px"></div>
    <!-- Latest version of the picker js for your locale -->
    <script src="https://cdn01.boxcdn.net/platform/elements/{VERSION}/en-US/picker.js"></script>
    <script>
      var folderId = "123";
      var accessToken = "abc";
      var filePicker = new Box.FilePicker();
      filePicker.show(folderId, accessToken, {
        container: ".container"
      });
    </script>
  </body>
</html>
```

## デモ

### ファイル選択のデモ

<iframe height="560" scrolling="no" title="Boxのファイル選択機能" src="//codepen.io/box-platform/embed/PWPxBm/?height=560&theme-id=27216&default-tab=result&embed-version=2&editable=true" frameborder="no" allowtransparency allowfullscreen style="width: 100%;">

</iframe>

### フォルダ選択のデモ

<iframe height="560" scrolling="no" title="Boxのフォルダ選択機能" src="//codepen.io/box-platform/embed/WRQLey/?height=560&theme-id=27216&default-tab=result&embed-version=2&editable=true" frameborder="no" allowtransparency allowfullscreen style="width: 100%;">

</iframe>

### ファイル選択とプレビューのデモ

<iframe height="560" scrolling="no" title="Boxのファイル選択機能とBoxプレビュー" src="//codepen.io/box-platform/embed/oBjJgL/?height=560&theme-id=27216&default-tab=result&embed-version=2&editable=true" frameborder="no" allowtransparency allowfullscreen style="width: 100%;">

</iframe>

### ポップアップ形式でのファイル選択

<iframe height="560" scrolling="no" title="ポップアップ形式のBoxのファイル選択機能とアップローダー" src="//codepen.io/box-platform/embed/oWEKdq/?height=560&theme-id=27216&default-tab=result&embed-version=2&editable=true" frameborder="no" allowtransparency allowfullscreen style="width: 100%;">

</iframe>

<Message>

# アクセストークン

上記のデモは、有効なアクセストークンを指定しなければ、完全に動作しない可能性があります。テスト目的の場合は、一時的な開発者トークンを使用できます。このトークンは、デモにある\[JS]タブで更新する必要があります。

</Message>

<!-- markdownlint-enable line-length -->

## API

```js
const { FilePicker } = Box;
const filePicker = new FilePicker();

/**
 * Shows the file selection.
 *
 * @public
 * @param {String} folderId - The root folder id.
 * @param {String} accessToken - The API access token.
 * @param {Object|void} [options] - Optional options.
 * @return {void}
 */
filePicker.show(folderId, accessToken, options);

/**
 * Hides the file picker, removes all event listeners, and clears out the HTML.
 *
 * @public
 * @return {void}
 */
filePicker.hide();

/**
 * Clears out the internal in-memory cache for the file picker. This forces
 * items to be reloaded from the API.
 *
 * @public
 * @return {void}
 */
filePicker.clearCache();

/**
 * Adds an event listener to the file picker. Listeners should be added before
 * calling show() so no events are missed.
 *
 * @public
 * @param {String} eventName - Name of the event.
 * @param {Function} listener - Callback function.
 * @return {void}
 */
filePicker.addListener(eventName, listener);

/**
 * Removes an event listener from the file picker.
 *
 * @public
 * @param {String} eventName - Name of the event.
 * @param {Function} listener - Callback function.
 * @return {void}
 */
filePicker.removeListener(eventName, listener);

/**
 * Removes all event listeners from the file picker.
 *
 * @public
 * @return {void}
 */
filePicker.removeAllListeners();
```

<!-- markdownlint-disable line-length -->

### パラメータ

| パラメータ         | 型      | 説明                                                                                                                  |
| ------------- | ------ | ------------------------------------------------------------------------------------------------------------------- |
| `folderId`    | String | BoxフォルダのID。選択しようとしているコンテンツが含まれるフォルダのIDです。Boxの\[すべてのファイル]フォルダを使用する場合は、`folderId`として`0`を使用します。                        |
| `accessToken` | String | 使用するBox APIアクセストークン。このトークンには、上記のフォルダに対する読み取り/書き込みアクセス権限が必要です。このトークンのために渡される値は、エクスプローラの表示中は有効期限切れにならないことが前提となっています。  |
| `options`     | Object | 省略可能なオプション。詳細は以下を参照してください。たとえば、`contentExplorer.show(FOLDER_ID, TOKEN, {canDelete: false})`を使用すると、削除オプションが非表示になります。 |

### オプション

| パラメータ                 | 型               | デフォルト                                                  | 説明                                                                                                                                                                                                                                                                                                                             |     |
| --------------------- | --------------- | ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --- |
| `container`           | `String`        | `document.body`                                        | CSS selector of the container in which the content picker should be placed. Calling hide() will clear out this container.                                                                                                                                                                                                      |     |
| `sortBy`              | `String`        | `name`                                                 | The initial sort by option for the content list. Value should be either name or `modified_at`.                                                                                                                                                                                                                                 |     |
| `sortDirection`       | `String`        | `ASC`                                                  | The initial sort direction option for the content list. Value should be either `ASC` or `DESC`.                                                                                                                                                                                                                                |     |
| `logoUrl`             | `String`        | ヘッダーに表示するカスタムロゴのURL。この値が「box」という文字列の場合は、Boxのロゴが表示されます。 |                                                                                                                                                                                                                                                                                                                                |     |
| `extensions`          | `Array<string>` | `[]`                                                   | Array of file extensions to be whitelisted for selection. for example `['doc', 'ppt']`. Applicable only to the File Picker and not the Folder Picker. If any extensions are provided, only those will have the ability to be selected. An empty array signifies all the file extensions are whitelisted for selection.         |     |
| `maxSelectable`       | `Number`        | `Infinity`                                             | Number of files or folders that can be selected. Specify 1 if you want only 1 file or folder selected.                                                                                                                                                                                                                         |     |
| `canUpload`           | `Boolean`       | `true`                                                 | Visually hides the upload option if this is set to `false`. Hiding the option alone will not prevent uploading unless the current folder permissions also set `can_upload` to `false`. This option has no effect when the folder permission `can_upload` is set to `false`.                                                    |     |
| `canSetShareAccess`   | `Boolean`       | `true`                                                 | Visually hides the sharing drop down select if set to `false`. Hiding the select drop down alone will not prevent changing the share access unless the folder item permissions also set `can_set_share_access` to `false`. This option has no effect when the folder item permission `can_set_share_access` is set to `false`. |     |
| `canCreateNewFolder`  | `Boolean`       | `true`                                                 | Visually hides the create new folder option. Hiding the option alone will not prevent creating a new folder unless the folder item permissions also set `can_upload` to `false`. This option has no effect when the folder item permission `can_upload` is set to false.                                                       |     |
| `sharedLink`          | `String`        |                                                        | 共有リンクのURL。フォルダが共有されており、アクセストークンがファイルの所有者またはコラボレータに属していない場合は必須です。                                                                                                                                                                                                                                                               |     |
| `sharedLinkPassword`  | `String`        |                                                        | 共有リンクのパスワード。共有リンクにパスワードが設定されている場合は必須です。                                                                                                                                                                                                                                                                                        |     |
| `modal`               | `Object`        |                                                        | When the modal attribute is specified, then the content pickers will not be created in-place. Instead a button will be created in the container and clicking this button will launch the content picker in a modal popup.                                                                                                      |     |
| `size`                | `String`        | `undefined`                                            | Indicates to the content picker to fit within a `small` or `large` width container. Value can be absent or one of `small` or `large`. If absent the UI Element will adapt to its container and automatically switch between small width or large width mode.                                                                   |     |
| `isTouch`             | Boolean         | デフォルトでは、ブラウザとデバイスのデフォルトのタッチサポートが設定されます。                | Content Explorerがタッチ対応デバイスにレンダリングされることを示します。                                                                                                                                                                                                                                                                                   |     |
| `autoFocus`           | `Boolean`       | `false`                                                | When set to `true`, the item grid will get focus on initial load.                                                                                                                                                                                                                                                              |     |
| `defaultView`         | String          | `files`                                                | Value should either be `files` or `recents`. When set to `recents`, by default you will see recent items instead of the regular file/folder structure.                                                                                                                                                                         |     |
| `chooseButtonLabel`   | `String`        |                                                        | String to re-label the Choose button                                                                                                                                                                                                                                                                                           |     |
| `cancelButtonLabel`   | `String`        |                                                        | String to re-label the Cancel button                                                                                                                                                                                                                                                                                           |     |
| `requestInterceptor`  | `Function`      |                                                        | リクエストをインターセプトする関数。例については、[このCodePen](https://codepen.io/box-platform/pen/jLdxEv)を参照してください。基盤となるXHRライブラリは`axios.js`で、[インターセプタでは同様のアプローチ](https://github.com/axios/axios#interceptors)に従っています。                                                                                                                                   |     |
| `responseInterceptor` | `Function`      |                                                        | 応答をインターセプトする関数。例については、[このCodePen](https://codepen.io/box-platform/pen/jLdxEv)を参照してください。基盤となるXHRライブラリは`axios.js`で、[インターセプタでは同様のアプローチ](https://github.com/axios/axios#interceptors)に従っています。                                                                                                                                      |     |

### モーダルオプション

| パラメータ              | 型        | デフォルト           | 説明                                            |
| ------------------ | -------- | --------------- | --------------------------------------------- |
| `buttonLabel`      | `String` |                 | Label for the button                          |
| `buttonClassName`  | `String` | Box Blue Button | ボタンを装飾するためのCSSクラス                             |
| `modalClassName`   | `String` |                 | CSS class to decorate the modal popup content |
| `overlayClassName` | `String` |                 | CSS class to decorate the modal popup overlay |

### イベント

| イベント名    | イベントデータ                       | 説明                                                                                                                                                                                                  |
| -------- | ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `choose` | `Array<File|Web Link|Folder>` | Will be fired when the Choose button is pressed. Event data will be an array of Folder Object or File Object or Web Link object depending upon whether it was a file selection or folder selection. |
| `cancel` |                               | Will be fired when the Cancel button is pressed                                                                                                                                                     |

<!-- markdownlint-enable line-length -->

## キーボードショートカット

クリックによって手動で、またはJavaScriptや上記の`autoFocus`プロパティによってプログラムで項目グリッドがフォーカスされていると、以下のキーボードショットカットが機能します(対応する操作が適切で許可されている場合)。

| キー                      | 動作                                 |
| ----------------------- | ---------------------------------- |
| `Arrow Up`              | Previous item row                  |
| `Arrow Down`            | Next item row                      |
| `Ctrl/Cmd + Arrow Up`   | First item row                     |
| `Ctrl/Cmd + Arrow Down` | Last item row                      |
| `/`                     | 検索                                 |
| `Shift + X`             | Select an item row                 |
| `Enter`                 | Open the selected item             |
| `g then f`              | Navigates to the root folder       |
| `g then u`              | Upload to the current folder       |
| `g then b`              | Focuses the root folder breadcrumb |
| `g then s`              | Shows the selected items           |
| `g then x`              | Cancel                             |
| `g then c`              | Choose                             |
| `g then r`              | Recent items                       |

## スコープ

アプリケーションで、エンドユーザーがContent Picker機能のサブセットのみにアクセスできるようにする必要がある場合は、[ダウンスコープ][downscope]を使用して、アクセストークンを適切にダウンスコープして必要な権限のセットを含むトークンを生成し、Content Pickerを初期化するエンドユーザークライアントに安全に渡すことができます。

以下は、ダウンスコープと一緒に使用する、UI Element固有の新しいスコープのセットです。こうしたスコープにより、開発者は、ダウンスコープされたトークンに適切なスコープを構成して、Content PickerのUIコントロールを有効/無効にすることができます。詳細については、[Box UI Elementsの専用スコープ][scopes]を参照してください。

<!-- markdownlint-disable line-length -->

| スコープ名         | 付与される権限                                            |
| ------------- | -------------------------------------------------- |
| `base_picker` | ユーザー/ファイル/トークンの権限に基づいて、フォルダツリー内のコンテンツへのアクセスを許可します。 |

### 機能のスコープ

| スコープ名         | 付与される権限                                                                              |
| ------------- | ------------------------------------------------------------------------------------ |
| `item_share`  | Allows sharing of resource specified under "resource" of the Token Exchange request. |
| `item_upload` | Content Pickerでのアップロードを許可します。                                                        |

### サンプルのシナリオ

| シナリオ                                                                                      | スコープ                                         |
| ----------------------------------------------------------------------------------------- | -------------------------------------------- |
| ユーザーが単にフォルダ構造内を移動してファイル/フォルダを選択する                                                         | `base_picker`                                |
| User wants to navigate a folder structure, pick a file / folder and also set access level | `base_picker` + `item_share`                 |
| ユーザーがフォルダ構造内を移動して、ファイル/フォルダを選択し、ファイル/フォルダもアップロードする                                        | `base_picker` + `item_upload`                |
| ユーザーがフォルダ構造内を移動して、ファイル/フォルダを選択し、ファイル/フォルダをアップロードして、ファイル/フォルダのアクセスレベルも設定する                 | `base_picker` + `item_share` + `item_upload` |

<!-- markdownlint-enable line-length -->

[downscope]: guide://authentication/access-tokens/downscope

[scopes]: guide://api-calls/permissions-and-errors/scopes
