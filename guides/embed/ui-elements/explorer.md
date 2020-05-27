---
rank: 3
related_endpoints: []
related_guides:
  - embed/ui-elements
required_guides:
  - embed/ui-elements/installation
related_resources: []
alias_paths:
  - /docs/box-content-explorer
  - /docs/content-explorer
category_id: embed
subcategory_id: embed/ui-elements
is_index: false
id: embed/ui-elements/explorer
type: guide
total_steps: 14
sibling_id: embed/ui-elements
parent_id: embed/ui-elements
next_page_id: embed/ui-elements/open-with
previous_page_id: embed/ui-elements/browser
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/embed/ui-elements/explorer.md
---
# Content Explorer

Box Content Explorer UI Elementを使用すると、開発者は、Boxに保存されているコンテンツのフォルダビューをデスクトップまたはモバイルウェブアプリに埋め込むことができます。ライブラリはBox APIを介して指定されたフォルダに関する情報を取得した後、メインのBoxウェブアプリと同様にそのコンテンツをフォルダビューにレンダリングします。ユーザーは、そのフォルダ階層内を移動し、名前の変更、削除、共有などのファイル操作を実行できます。

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
    <title>Box Content Explorer Demo</title>

    <!-- polyfill.io only loads the polyfills your browser needs -->
    <script src="https://cdn.polyfill.io/v2/polyfill.min.js?features=es6,Intl"></script>
    <!-- Alternatively, use polyfill hosted on the Box CDN
    <script src="https://cdn01.boxcdn.net/polyfills/core-js/2.5.3/core.min.js"></script>
    -->

    <!-- Latest version of the explorer css for your locale -->
    <link
      rel="stylesheet"
      href="https://cdn01.boxcdn.net/platform/elements/{VERSION}/en-US/explorer.css"
    />
  </head>
  <body>
    <div class="container" style="height:600px"></div>
    <!-- Latest version of the explorer js for your locale -->
    <script src="https://cdn01.boxcdn.net/platform/elements/{VERSION}/en-US/explorer.js"></script>
    <script>
      var folderId = "123";
      var accessToken = "abc";
      var contentExplorer = new Box.ContentExplorer();
      contentExplorer.show(folderId, accessToken, {
        container: ".container"
      });
    </script>
  </body>
</html>
```

## デモ

<iframe height="560" scrolling="no" title="Box Content Explorer" src="//codepen.io/box-platform/embed/wdWWdN/?height=560&theme-id=27216&default-tab=result&embed-version=2&editable=true" frameborder="no" allowtransparency allowfullscreen style="width: 100%;">

</iframe>

<!-- markdownlint-enable line-length -->

## API

```js
const { ContentExplorer } = Box;
const contentExplorer = new ContentExplorer();

/**
 * Shows the content explorer.
 *
 * @param {string} folderId - The root folder id
 * @param {string} accessToken - Box API access token
 * @param {Object} [options] - Options
 * @return {void}
 */
contentExplorer.show(folderId, accessToken, options);

/**
 * Hides the content explorer, removes all event listeners, and clears out the
 * HTML.
 *
 * @return {void}
 */
contentExplorer.hide();

/**
 * Clears out the internal in-memory
 * cache for the content explorer forcing
 * re-load of items via the API.
 *
 * @public
 * @return {void}
 */
contentExplorer.clearCache();

/**
 * Adds an event listener to the content explorer. Listeners should be added
 * before calling show() so no events are missed.
 *
 * @param {string} eventName - Name of the event
 * @param {Function} listener - Callback function
 * @return {void}
 */
contentExplorer.addListener(eventName, listener);

/**
 * Removes an event listener from the content explorer.
 *
 * @param {string} eventName - Name of the event
 * @param {Function} listener - Callback function
 * @return {void}
 */
contentExplorer.removeListener(eventName, listener);

/**
 * Removes all event listeners from the content explorer.
 *
 * @return {void}
 */
contentExplorer.removeAllListeners();
```

<!-- markdownlint-disable line-length -->

### パラメータ

| パラメータ         | 型      | 説明                                                                                                                  |
| ------------- | ------ | ------------------------------------------------------------------------------------------------------------------- |
| `folderId`    | String | BoxフォルダのID。中を移動するフォルダのIDになります。Boxの\[すべてのファイル]フォルダを使用する場合は、`folderId`として`0`を使用します。                                   |
| `accessToken` | String | 使用するBox APIアクセストークン。このトークンには、上記のフォルダに対する読み取り/書き込みアクセス権限が必要です。このトークンのために渡される値は、エクスプローラの表示中は有効期限切れにならないことが前提となっています。  |
| `options`     | Object | 省略可能なオプション。詳細は以下を参照してください。たとえば、`contentExplorer.show(FOLDER_ID, TOKEN, {canDelete: false})`を使用すると、削除オプションが非表示になります。 |

### オプション

| パラメータ                  | 型        | デフォルト                                   | 説明                                                                                                                                                                                                                                                                                                                                                  |
| ---------------------- | -------- | --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `container`            | String   | `document.body`                         | CSS selector of the container in which the content explorer should be placed. Calling hide() will clear out this container.                                                                                                                                                                                                                         |
| `sortBy`               | String   | `name`                                  | The initial sort by option for the content list. Value can be either `id`, `name`, `date` or `size`.                                                                                                                                                                                                                                                |
| `sortDirection`        | String   | `ASC`                                   | The initial sort direction option for the content list. Value should be either `ASC` or `DESC`.                                                                                                                                                                                                                                                     |
| `logoUrl`              | String   |                                         | ヘッダーに表示するカスタムロゴのURL。この値が「box」という文字列の場合は、Boxのロゴが表示されます。                                                                                                                                                                                                                                                                                              |
| `canPreview`           | Boolean  | `true`                                  | If this option is set to `true` AND `can_preview` permission on the file is `true`, files on the content explorer will be clickable. Clicking on a file will launch preview of that file. This option has no effect when the file permission `can_preview` is set to `false`. This is only applicable to files that can be previewed.               |
| `canDownload`          | Boolean  | `true`                                  | Visually hides the download option if this is set to `false`. Hiding the option alone will not prevent deleting unless the file permissions also set `can_download` to `false`. This option has no effect when the file permission `can_download` is set to `false`. This is only applicable to files.                                              |
| `canDelete`            | Boolean  | `true`                                  | Visually hides the delete option if this is set to `false`. Hiding the option alone will not prevent deleting unless the item permissions also set `can_delete` to `false`. This option has no effect when the item permission `can_delete` is set to `false`.                                                                                      |
| `canRename`            | Boolean  | `true`                                  | Visually hides the rename option if this is set to `false`. Hiding the option alone will not prevent uploading unless the item permissions also set `can_rename` to `false`.                                                                                                                                                                        |
| `canUpload`            | Boolean  | `true`                                  | Visually hides the upload option if this is set to `false`. Hiding the option alone will not prevent uploading unless the current folder permissions also set `can_upload` to `false`. This option has no effect when the folder permission `can_upload` is set to `false`.                                                                         |
| `canCreateNewFolder`   | Boolean  | `true`                                  | Visually hides the create new folder option. Hiding the option alone will not prevent creating a new folder unless the folder item permissions also set `can_upload` to `false`. This option has no effect when the folder item permission `can_upload` is set to `false`.                                                                          |
| `canShare`             | Boolean  | `true`                                  | Visually hides the share button if set to `false`. Hiding the button alone will not prevent sharing unless the item `permissions` also set `can_share` to false. This option has no effect when the item permission `can_share` is set to `false`.                                                                                                  |
| `canSetShareAccess`    | Boolean  | `true`                                  | Visually hides the sharing drop down select that allows changing share access, if set to `false`. Hiding the select drop down alone will not prevent changing the share access unless the item permissions also set `can_set_share_access` to `false`. This option has no effect when the item permission `can_set_share_access` is set to `false`. |
| `sharedLink`           | String   |                                         | 共有リンクのURL。フォルダが共有されており、アクセストークンがファイルの所有者またはコラボレータに属していない場合は必須です。                                                                                                                                                                                                                                                                                    |
| `sharedLinkPassword`   | String   |                                         | 共有リンクのパスワード。共有リンクにパスワードが設定されている場合は必須です。                                                                                                                                                                                                                                                                                                             |
| `size`                 | String   | `undefined`                             | Indicates to the content explorer to fit within a small or large width container. Value can be absent or one of `small` or `large`. If absent the UI Element will adapt to its container and automatically switch between `small` width or `large` width mode.                                                                                      |
| `isTouch`              | Boolean  | デフォルトでは、ブラウザとデバイスのデフォルトのタッチサポートが設定されます。 | Content Explorerがタッチ対応デバイスにレンダリングされることを示します。                                                                                                                                                                                                                                                                                                        |
| `autoFocus`            | Boolean  | `false`                                 | When set to `true`, the item grid will get focus on initial load.                                                                                                                                                                                                                                                                                   |
| `defaultView`          | String   | `files`                                 | Value should either be `files` or `recents`. When set to `recents`, by default you will see recent items instead of the regular file/folder structure.                                                                                                                                                                                              |
| `requestInterceptor`   | Function |                                         | リクエストをインターセプトする関数。例については、[このCodePen](https://codepen.io/box-platform/pen/jLdxEv)を参照してください。基盤となるXHRライブラリは`axios.js`で、[インターセプタでは同様のアプローチ](https://github.com/axios/axios#interceptors)に従っています。                                                                                                                                                        |
| `responseInterceptor`  | Function |                                         | 応答をインターセプトする関数。例については、[このCodePen](https://codepen.io/box-platform/pen/jLdxEv)を参照してください。基盤となるXHRライブラリは`axios.js`で、[インターセプタでは同様のアプローチ](https://github.com/axios/axios#interceptors)に従っています。                                                                                                                                                           |
| `ContentOpenWithProps` | Object   | `{ show: false }`                       | Allows you to show the Open With Element when previewing via the explorer.                                                                                                                                                                                                                                                                          |

### イベント

| イベント名      | イベントデータ                       | 説明                                          |
| ---------- | ----------------------------- | ------------------------------------------- |
| `select`   | `Array<File|Web Link|Folder>` | Will be fired when item rows are selected.  |
| `rename`   | `File|Web Link|Folder`        | Will be fired when an item is renamed.      |
| `preview`  | `File`                        | Will be fired when a file is previewed.     |
| `download` | `Array<File>`                 | Will be fired when items are downloaded.    |
| `delete`   | `Array<File>`                 | Will be fired when items are deleted.       |
| `upload`   | `Array<File>`                 | Will be fired when items are uploaded.      |
| `navigate` | `Folder`                      | Will be fired when navigating into folders. |
| `create`   | フォルダ                          | 新しいフォルダが作成されたときに発生します。                      |

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
| `Delete`                | Delete the selected item           |
| `Enter`                 | Open the selected item             |
| `Shift + R`             | Rename the selected item           |
| `Shift + S`             | Share the selected item            |
| `Shift + D`             | Download the selected item         |
| `g then f`              | Navigates to the root folder       |
| `g then u`              | Upload to the current folder       |
| `g then b`              | Focuses the root folder breadcrumb |
| `g then r`              | Recent items                       |

## スコープ

アプリケーションで、エンドユーザーがContent Explorer機能のサブセットのみにアクセスできるようにする必要がある場合は、[ダウンスコープ][downscope]を使用して、アクセストークンを適切にダウンスコープして必要な権限のセットを含むトークンを生成し、Content Explorerを初期化するエンドユーザークライアントに安全に渡すことができます。

以下は、ダウンスコープと一緒に使用する、UI Element固有の新しいスコープのセットです。こうしたスコープにより、開発者は、ダウンスコープされたトークンに適切なスコープを構成して、Content ExplorerのUIコントロールを有効/無効にすることができます。詳細については、[Box UI Elementsの専用スコープ][scopes]を参照してください。

### 基本スコープ

<!-- markdownlint-disable line-length -->

| スコープ名           | 付与される権限                                                                           |
| --------------- | --------------------------------------------------------------------------------- |
| `base_explorer` | Allows access to content in the folder tree based on user/file/token permissions. |

### 機能のスコープ

| スコープ名           | 付与される権限                                                                                                   |
| --------------- | --------------------------------------------------------------------------------------------------------- |
| `item_preview`  | Automatically enables preview of the file, upon user click (requires Preview UI Element to be referenced) |
| `item_download` | Allows files/folders contents to be downloaded                                                            |
| `item_rename`   | Allows files/folders to be renamed                                                                        |
| `item_share`    | Allows sharing of resource specified under "resource" of the downscope request.                           |
| `item_delete`   | Allows file/folders to be deleted                                                                         |

### サンプルのシナリオ

| シナリオ                                                     | スコープ                                                                                                              |
| -------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| ユーザーがフォルダ構造内を移動する(基本機能)                                  | `base_explorer`                                                                                                   |
| User want basic functionality + preview                  | `base_explorer` + `item_preview`                                                                                  |
| ユーザーが基本機能、プレビュー、およびダウンロードを必要とする                          | `base_explorer` + `item_preview` + `item_download`                                                                |
| ユーザーが基本機能、プレビュー、ダウンロード、およびファイル/フォルダ名の変更を必要とする            | `base_explorer` + `item_preview` + `item_download` + `item_rename`                                                |
| ユーザーがすべての機能(基本、プレビュー、ダウンロード、名前の変更、共有、アップロード、および削除)を必要とする | `base_explorer` + `item_preview` + `item_download` + `item_rename` + `item_delete` + `item_share` + `item_upload` |

<!-- markdownlint-enable line-length -->

[downscope]: guide://authentication/access-tokens/downscope

[scopes]: guide://api-calls/permissions-and-errors/scopes
