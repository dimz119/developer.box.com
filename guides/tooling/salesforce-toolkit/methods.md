---
rank: 1
related_endpoints: []
related_guides:
  - tooling/sdks/salesforce
required_guides: []
related_resources: []
alias_paths: []
category_id: tooling
subcategory_id: tooling/salesforce-toolkit
is_index: false
id: tooling/salesforce-toolkit/methods
type: guide
total_steps: 2
sibling_id: tooling/salesforce-toolkit
parent_id: tooling/salesforce-toolkit
next_page_id: tooling/salesforce-toolkit/samples
previous_page_id: tooling/salesforce-toolkit
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/tooling/salesforce-toolkit/methods.md
---
<!-- alex disable failed -->

# メソッドと操作

## ツールキットの詳細

クラス名: `box.Toolkit`

## インスタンス変数

### `mostRecentError`

String to indicate the most recent error that occurred when calling instance
methods.

この文字列が存在しても、操作が失敗したことを意味するわけではありません。そのエラーが回復可能であった可能性もあります。ただし、この文字列に値がない場合は、操作が成功したことを示しています。

### `Enum CollaborationType`

Enum to indicate the [type of collaboration][collab-type].

可能性のある値: `EDITOR`、`VIEWER`、`PREVIEWER`、`UPLOADER`、`COOWNER`、`OWNER`、`PREVIEWERUPLOADER`、`VIEWERUPLOADER`

## 静的メソッド

### `deleteServiceUserAssociation`

Method to clear association between Service Account and Box for Salesforce
integration. This can be used to change the service account if an incorrect one
is being used.

パラメータ:

* なし

戻り値:

* ユーザーのアカウントが存在していたが削除された場合は`true`。
* ユーザーのアカウントが何らかの理由(存在しなかった場合を含む)で削除されなかった場合は`false`。

### `deleteUserAssociation`

<!-- markdownlint-disable line-length -->

| パラメータ    | 型   | 説明                  |
| -------- | --- | ------------------- |
| `userId` | id  | 資格情報がクリアされるユーザーのID。 |

<!-- markdownlint-enable line-length -->

戻り値:

* ユーザーのアカウントが存在していたが削除された場合は`true`。
* ユーザーのアカウントが何らかの理由(存在しなかった場合を含む)で削除されなかった場合は`false`。

## インスタンスメソッド(コンストラクタ/デストラクタ)

### `box.Toolkit()`

パラメータ:

* なし

### `commitChanges`

Treat this method as a destructor for the `box.Toolkit()` method.

<Message type="warning">

このメソッドは重要です。すべてのフォルダ/コラボレーション操作が完了した後、毎回、例外なくこのメソッドを呼び出す必要があります。

</Message>

Salesforceではデータベースの更新/挿入/削除の後の呼び出しは許可されないため、Toolkitクラスではすべての呼び出し操作が完了した後で挿入するオブジェクトのコレクションが保持されます。このメソッドを呼び出さない場合、このようなオブジェクトがデータベースから消去され、ユーザー/レコード/フォルダの関連付けを追跡するテーブルの同期も失われて、高度なデバッグによる修正が必要になります。

パラメータ:

* なし

戻り値:

* `Void`

## Generic Methods

Box for Salesforce Developer Toolkitは、パラメータとして[HttpRequest][sf-httprequest]オブジェクトを受け取り、[HttpResponse][sf-httpresponse]オブジェクトを返すグローバルメソッドを提供します。このメソッドではサービスアカウントの認証の詳細情報を利用してBoxのAPIを呼び出すため、開発者は統合のビジネスロジックに集中して取り組むことができます。

### `sendRequest`

<!-- markdownlint-disable line-length -->

| パラメータ     | 型                             | 説明                                   |
| --------- | ----------------------------- | ------------------------------------ |
| `request` | [HttpRequest][sf-httprequest] | エンドポイントとメソッドが設定されたHttpRequestオブジェクト。 |

<!-- markdownlint-enable line-length -->

戻り値:

* BoxのAPI呼び出しからの応答の詳細情報が含まれた[HttpResponse][sf-httpresponse]オブジェクト。
* HttpRequestのインプットの情報が不足している場合は`Toolkit.BoxApiException`。
* サービスアカウントの認証の詳細情報を取得する際に問題が発生した場合は`null`。この場合は、`mostRecentError`を確認してください。

## ファイル操作

### `createFileFromAttachment`

<Message type="notice">

Available in version 3.46 and above.

Salesforceの文字列長の上限は600万文字です。base64エンコード/デコードプロセスでは文字列が膨張するため、有効なファイルサイズの上限は4.3MBとなっています。

</Message>

<!-- markdownlint-disable line-length -->

| パラメータ              | 型            | 説明                                                                                                                                                                                                                                                          |
| ------------------ | ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `att`              | `Attachment` | The attachment to be converted into a File in Box.                                                                                                                                                                                                          |
| `fileNameOverride` | `string`     | Optional - Name of the new file. If no value is passed in, the name of the attachment is used.                                                                                                                                                              |
| `folderIdOverride` | `string`     | Optional - Box folder id to place this attachment in. If no value is passed in, the file will be placed in the folder associated with the record that is the `parentId` of the attachment. If the record-specific folder doesn't exist, it will be created. |
| `accessToken`      | `string`     | Optional - if `accessToken` is sent, that value is used for the Box API call. Otherwise, the default account credentials are used.                                                                                                                          |

<!-- markdownlint-enable line-length -->

戻り値:

* `string`。作成されたBoxファイルのIDが返されます。
* エラーが発生した場合は`null`。この場合には、`mostRecentError`を確認してください。

### `getObjectFolderByRecordId`

<!-- markdownlint-disable line-length -->

| パラメータ      | 型    | 説明                                                         |
| ---------- | ---- | ---------------------------------------------------------- |
| `recordId` | `id` | Salesforce record id whose root folder id you want to get. |

<!-- markdownlint-enable line-length -->

戻り値:

* `string`。レコードIDが渡されたオブジェクトルートフォルダのBoxフォルダIDが返されます。

## フォルダ操作

### `getRootFolderId`

パラメータ:

* なし

戻り値:

* `string`。SalesforceルートフォルダのBoxフォルダIDが返されます。

### `getObjectFolderByRecordId`

<!-- markdownlint-disable line-length -->

| パラメータ      | 型    | 説明                                                         |
| ---------- | ---- | ---------------------------------------------------------- |
| `recordId` | `id` | Salesforce record id whose root folder id you want to get. |

<!-- markdownlint-enable line-length -->

戻り値:

* `string`。レコードIDが渡されたオブジェクトルートフォルダのBoxフォルダIDが返されます。

### `getFolderUrl`

* This method gets the embed widget URL for a particular record so customers
  can use their own embed logic if desired.
* このメソッドではシームレスなログインの設定が優先されます。このため、シームレスなログインが有効になっている場合、ユーザーはURLに自動的にログインされます。

<!-- markdownlint-disable line-length -->

| パラメータ             | 型         | 説明                                                                          |
| ----------------- | --------- | --------------------------------------------------------------------------- |
| `recordId`        | `id`      | Salesforce record id whose root folder id you want to get.                  |
| `isMobileContext` | `boolean` | Boolean to indicate whether the URL should be mobile (true) or not (false). |

<!-- markdownlint-enable line-length -->

戻り値:

* `string`。渡されたSalesforceレコードIDに関連付けられているフォルダを表すURLが返されます。このURLをBox埋め込みウィジェットで使用して、任意のVisualforceページに埋め込むことができます。

### `createObjectFolderForRecordId`

<!-- markdownlint-disable line-length -->

| パラメータ      | 型    | 説明                                                         |
| ---------- | ---- | ---------------------------------------------------------- |
| `recordId` | `id` | Salesforce record id whose root folder id you want to get. |

<!-- markdownlint-enable line-length -->

戻り値:

* `string`。作成されたルートフォルダのBoxフォルダIDが返されます。
* ルートフォルダがすでに存在していた場合、そのルートフォルダのBoxフォルダIDが返されます。

### `createFolder`

<!-- markdownlint-disable line-length -->

| パラメータ            | 型        | 説明                                                                                                                                          |
| ---------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `folderName`     | `string` | Name of the folder to be created. Folder names are subject to some restrictions. [See here for more details.](endpoint://post_folders)      |
| `parentFolderId` | `string` | Parent Box folder this folder will be created in.                                                                                           |
| `accessToken`    | `string` | Optional - If `accessToken` is sent, that value is used for the Box API call,; otherwise, the default service account credentials are used. |

<!-- markdownlint-enable line-length -->

戻り値:

* `string`。作成されたフォルダのBoxフォルダIDが返されます。
* フォルダが作成されなかった場合は`null`が返されます。この場合、`mostRecentError`で詳細を確認してください。

### `createFolderForRecordId`

<!-- markdownlint-disable line-length -->

| パラメータ                 | 型         | 説明                                                                                                                                                         |
| --------------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `recordId`            | `id`      | Salesforce record id that a Box folder will be created for.                                                                                                |
| `folderNameOverride`  | `string`  | By default, the record's name will be the folder name. If you want to name it something else, send that value here.                                        |
| `optCreateRootFolder` | `boolean` | Boolean to indicate whether to create the object root folder if it doesn't exist. If false is sent and the root folder does not exist, the call will fail. |

<!-- markdownlint-enable line-length -->

戻り値:

* `string`。作成されたフォルダのBoxフォルダIDが返されます。
* フォルダが作成されなかった場合は`null`が返されます。この場合、`mostRecentError`で詳細を確認してください。
* SalesforceレコードがすでにBoxフォルダに関連付けられている場合、既存のBoxフォルダIDが返されます。

### `moveFolder`

<!-- markdownlint-disable line-length -->

| パラメータ               | 型        | 説明                                                                                                                                         |
| ------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `folderId`          | `string` | Box folder Id of the folder to be moved.                                                                                                   |
| `newParentFolderId` | `string` | Box folder Id of the folder that will be the new parent folder.                                                                            |
| `accessToken`       | `string` | Optional - If `accessToken` is sent, that value is used for the Box API call. Otherwise, the default service account credentials are used. |

<!-- markdownlint-enable line-length -->

戻り値:

* フォルダが正常に移動された場合は`true`。
* フォルダが正常に移動されなかった場合は`false`。`mostRecentError`で詳細を確認してください。

## フォルダ関連付けメソッド

### `getFolderAssociationsByRecordId`

<!-- markdownlint-disable line-length -->

| パラメータ      | 型    | 説明                                                                            |
| ---------- | ---- | ----------------------------------------------------------------------------- |
| `recordId` | `id` | Salesforce record id that the folder mapping entries returned are related to. |

<!-- markdownlint-enable line-length -->

戻り値:

* 返されるリストは、このレコードに関連付けられているすべてのフォルダマッピングエントリのコレクションです。
* 一般に、フォルダマッピングエントリが存在しない場合は空のリストになりますが、状況によって`null`になる場合があります。

### `getFolderIdByRecordId`

<!-- markdownlint-disable line-length -->

| パラメータ      | 型    | 説明                                                    |
| ---------- | ---- | ----------------------------------------------------- |
| `recordId` | `id` | Salesforce record id whose folder id you wish to get. |

<!-- markdownlint-enable line-length -->

戻り値:

* `string`。渡されたSalesforceレコードIDに関連付けられたBoxフォルダIDが返されます。

### `getRecordIdByFolderId`

<!-- markdownlint-disable line-length -->

| パラメータ      | 型        | 説明             |
| ---------- | -------- | -------------- |
| `folderId` | `string` | Box folder id. |

<!-- markdownlint-enable line-length -->

戻り値:

* `id`。渡されたBoxフォルダIDに関連付けられたSalesforceレコードIDが返されます。

### `createFolderAssociation`

<!-- markdownlint-disable line-length -->

| パラメータ      | 型        | 説明                                                               |
| ---------- | -------- | ---------------------------------------------------------------- |
| `recordId` | `id`     | Salesforce record Id that is being associated with a box folder. |
| `folderId` | `string` | Box folder Id being associated with a Salesforce record.         |

<!-- markdownlint-enable line-length -->

戻り値:

* `boxFRUPc`オブジェクト - エラーが発生した場合(`mostRecentError`を確認)、返されるFRUPオブジェクトは`null`になります。このFRUPエントリは、`commitChanges`メソッドの呼び出し時にデータベースに挿入されます。このメソッドでは、同じフォルダの複数レコードへの関連付けやその逆の関連付けが許可されないため、他のフォルダの関連付けとの一貫性が保証されます。

## コラボレーションメソッド

<Message type="warning">

Box for Salesforce Developer Toolkitによって作成されたコラボレーションは、コラボレータにコラボレーションメールを送信しません。Box for Salesforce統合に使用されるサービスアカウントのみがコラボレーションメールを受け取ります。

</Message>

### `createCollaboration`

<!-- markdownlint-disable line-length -->

| パラメータ          | 型                               | 説明                                                                                                                                   |
| -------------- | ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| `folderId`     | `string`                        | Box folder id to create a collaboration on.                                                                                          |
| `boxUserId`    | `string`                        | Box user id to be collaborated (either `boxUserId` or `emailAddress` is required but not both).                                      |
| `emailAddress` | `box.Toolkit.CollaborationType` | Email address of the box user to be.                                                                                                 |
| `collabType`   | `string`                        | Type of collaboration (see the `CollaborationType` enum definition).                                                                 |
| `accessToken`  | `string`                        | Optional - If sent, this value is used for authentication for the box API call; if `null`, the service account credentials are used. |

<!-- markdownlint-enable line-length -->

戻り値:

* `string`。作成されたBoxコラボレーションのIDが返されます。
* エラーが発生した場合は`null`が返されます。その場合、`mostRecentError`を確認してください。

### `createCollaborationOnRecord`

<!-- markdownlint-disable line-length -->

| パラメータ             | 型                               | 説明                                                                                                                                                                                                                                                                     |
| ----------------- | ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `userId`          | `id`                            | Salesforce user id to be collaborated.                                                                                                                                                                                                                                 |
| `recordId`        | `id`                            | Salesforce record id of the record folder to be collaborated on.                                                                                                                                                                                                       |
| `collabType`      | `box.Toolkit.CollaborationType` | Type of collaboration (see the `CollaborationType` enum definition).                                                                                                                                                                                                   |
| `optCreateFolder` | `boolean`                       | Boolean to indicate whether to create the Box folder associated for the Salesforce record id if it does not already exist. This also creates the root folder if it did not already exist. If set to `false` and the folder does not already exist, the call will fail. |

<!-- markdownlint-enable line-length -->

戻り値:

* `string`。作成されたBoxコラボレーションのIDが返されます。
* エラーが発生した場合は`null`が返されます。その場合、`mostRecentError`を確認してください。

[collab-type]: https://community.box.com/t5/Collaborate-By-Inviting-Others/Understanding-Collaborator-Permission-Levels/ta-p/144

[sf-httprequest]: https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_classes_restful_http_httprequest.htm

[sf-httpresponse]: https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_classes_restful_http_httpresponse.htm#apex_classes_restful_http_httpresponse
