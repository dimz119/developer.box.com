---
alias_paths:
  - /docs/api-changelog
centered: true
rank: 0
category_id: changelog
subcategory_id: null
is_index: true
id: changelog
type: page
total_steps: 3
sibling_id: pages
parent_id: pages
next_page_id: changelog/2018
previous_page_id: ''
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/pages/changelog/index.md
---
<!-- alex disable postman-postwoman -->

# 変更ログ

過去の変更ログについては、[2019](page://changelog/2019)および[2018](page://changelog/2018)のリリースノートを参照してください。

## 2020年4月23日/ファイルの新しい`classification`フィールド

`Field`オブジェクト内で、新しいオプションフィールドである`classification`が使用可能になりました。このフィールドは、現在ファイルに適用されている分類を表します。

分類は、[ファイル情報の取得](endpoint://get-files-id)など、ファイルを返す任意のエンドポイントを介してリクエストでき、[追加`fields`のリクエスト](g://api-calls/request-extra-fields)をサポートします。

```js
{
  "id": "123456789",
  "type": "file",
  "etag": "1",
  "classification": {     
    "name": "Top Secret",     
    "definition": "Content that should not be shared outside the company."
  },
  ...
}
```

分類は、APIまたはBox Shieldを介して設定することも、ユーザーがウェブアプリケーションを使用して設定することもできます。

## 2020年4月23日/ファイルおよびファイルバージョンに新しい`uploader_display_name`を追加

新しいフィールド`uploader_display_name`がファイルリソースとファイルバージョンリソースの両方に追加されました。このフィールドでは、アップロード時にユーザーの名前を指定します。

```json
{
  ...
  "uploader_display_name": "Ellis Wiggins"
}
```

このフィールドをファイルおよびファイルバージョンのエンドポイントのいずれかでリクエストするには、`fields`クエリパラメータを指定します。以下に例を示します。

```bash
curl -X GET https://api.box.com/2.0/files/12345?fields=uploader_display_name \
     -H 'Authorization: Bearer <ACCESS_TOKEN>'
```

ログアウトした匿名ユーザーがファイルをアップロードすると、そのユーザーのメールアドレスが返されます。使用可能なメールアドレスがない場合、このフィールドはデフォルトで`Someone`というテキストが設定されます。

## 2020年4月21日/新しいメタデータクエリAPIが使用可能になりました

新しいAPIが使用可能になりました。開発者はこのAPIを使用して、ファイルやフォルダに追加されているメタデータによってそのファイルやフォルダに対してクエリを実行できます。Boxは、こちらの[メタデータクエリAPI][e_mdq_api]のリファレンスドキュメントを更新したほか、新しい[ガイド][g_mdq_api]と[メタデータのクイックスタートガイド][qs_mdq_api]の更新をリリースしました。

[e_mdq_api]: e://post_metadata_queries_execute_read

[g_mdq_api]: g://metadata/queries

[qs_mdq_api]: g://metadata/quick-start

## 2020年3月30日/メタデータの`date`フィールドの形式に影響する可能性のある変更

Boxのメタデータインフラストラクチャに対する継続的な改善の一環として、メタデータテンプレートの`date`フィールドの形式に大きく影響する可能性のある3つの変更をリリースする予定です。これらの変更により、Box APIで返される形式は、API呼び出し間でより一貫性のあるものになります。

最初の変更は、日付におけるタイムゾーンオフセットの使用に影響します。これまでは、タイムゾーンオフセットを含めるように日付を設定した場合でも、APIによって返される日付には一貫性がなく、タイムゾーンオフセットが含まれるものもあれば、含まれないものもありました。今後は、すべての日付がUTC (協定世界時)に変換され、タイムゾーンオフセットは削除されます。

例:

* 日付が`2020-02-20T12:00:00.000-01:00`に設定されたと想定します。
* これまでは、APIでは`2020-02-20T12:00:00.000-01:00` (元の値)または`2020-02-20T13:00:00.000Z` (UTCに調整された値)が返されました。
* 今後は、常に`2020-02-20T13:00:00.000Z` (UTCに調整された値)が返されます。

2つ目の変更は、メタデータAPIによって返される日付の秒未満の精度に影響します。これまでは、メタデータAPIで返される値の秒未満の精度は0～3桁でした。今後、Boxから返されるメタデータの日時値は、常にミリ秒まで正確になります。

例:

* これまで、APIでは`2020-02-20T12:00:00Z`、`2020-02-20T12:00:00.0Z`、`2020-02-20T12:00:00.00Z`または`2020-02-20T12:00:00.000Z`が返される可能性がありました。
* 今後は、常に`2020-02-20T12:00:00.000Z`が返されます。

最後の変更は、メタデータインスタンスの更新時の[`test`](g/metadata/instances/update/#Test-a-value)操作の使用に影響します。これまでのテストでは、リテラル文字列値を使用して日時の値が比較されていました。今回の更新後は、UNIXのタイムスタンプ(ミリ秒単位)を使用して比較されます。

例:

* これまでは、`2020-01-21T19:20:00.123-08:00`は`2020-01-22T03:20:00.123Z`と同じではありませんでした。
* 今後、`2020-01-21T19:20:00.123-08:00`は`2020-01-22T03:20:00.123Z`と同じになります。

### 今回の変更がアプリケーションに及ぼす影響

`RFC3339`に準拠した日時の解析を実装するアプリケーションでは、これらはすべて同じ日付を表す有効な`RFC3339`値であるため、何も実行する必要はありません。`RFC3339`に準拠した日時の解析を実装していないアプリケーションは、解析を実行できるように更新する必要があります。

すべてのBox公式SDKでは、`RFC3339`に準拠した日時の解析をサポートしているため、Box公式SDKの最新バージョンを使用しているアプリケーションの場合、更新は不要です。

### 今回の変更がアプリケーションに影響を及ぼした場合の対応

今回の変更は、今後数週間かけて段階的にリリースする予定です。メタデータチームは潜在的な影響を監視しますが、ご自身が影響を受けていることに気付いた場合は、カスタマーサクセスマネージャまたは当社のサポートチャネルまでお問い合わせください。

## 2020年3月30日/メタデータインスタンスのバージョンに対する変更

メタデータインフラストラクチャに対する継続的な改善の一環として、メタデータインスタンスの[`version`](r:/metadata/#param-$version)値に対する小規模な変更をリリースします。

これまで、メタデータインスタンスに関連付けられたバージョン番号は、関連付けられたメタデータテンプレートのフィールドが削除された場合または関連付けられたメタデータテンプレートで`enum`フィールドのオプションが削除された場合に増加していました。

今後は、こうした変更により、メタデータインスタンスのバージョン番号が増加することはありません。

バージョン番号の増加は常に暗黙的に行われてきたため、Boxでは、メタデータインスタンスのバージョンから得られる価値はほとんどないと認識しています。そのため、この変更がお客様に影響を及ぼすとは考えておりません。それでも、ご自身が影響を受けていることに気付いた場合は、カスタマーサクセスマネージャまたは当社のサポートチャネルまでお問い合わせください。

## 2020年2月3日/Preview SDK `v2.34.0`のリリース

Preview SDKのバージョン`2.34.0`がリリースされ、新しいJavaScriptとCSSのPreviewファイルが使用可能になりました。新しい変更を導入するには、Content Preview用のリンクを[UI Elementsの手動によるインストール][ui-elements-manual-install]に関する記事で確認してください。

機能の変更点の全一覧については、`v2.34.0`の[リリースノート][preview-2.34-release-notes]を参照してください。

## 2020年1月22日/Preview SDK `v2.33.1`のリリース

Preview SDKのバージョン`2.33.1`がリリースされ、新しいJavaScriptとCSSのPreviewファイルが使用可能になりました。新しい変更を導入するには、Content Preview用のリンクを[UI Elementsの手動によるインストール][ui-elements-manual-install]に関する記事で確認してください。

機能の変更点の全一覧については、`v2.33.1`の[リリースノート][preview-2.33-release-notes]を参照してください。

## 2020年1月20日/Postmanコレクションとクイックスタートを更新

Box Postmanコレクションは、新機能と、統合されたクイックスタートガイドによって更新されました。主な機能は以下のとおりです。

* エンドツーエンドの[Postmanクイックスタートガイド][postman-quick-start-guide]。これは、ユーザーがPostmanのインストール、Boxアプリの設定、PostmanへのAPI資格情報の読み込みを行う際に役立ちます。
* Box API用に[再編成されたPostmanコレクション][postman-collection]。これにより、API資格情報の期限切れが自動的に検出され、必要に応じてこれらの資格情報を更新するための統合ソリューションが提供されます。

しばらくの間は[従来のPostmanコレクション][legacy-postman-collection]も引き続きご利用いただけます。

[postman-quick-start-guide]: g://tooling/postman/quick-start

[postman-collection]: g://tooling/postman/install

[legacy-postman-collection]: g://tooling/postman/legacy

[ui-elements-manual-install]: g://embed/ui-elements/installation/#manual-installation

[preview-2.34-release-notes]: https://github.com/box/box-content-preview/releases/tag/v2.34.0

[preview-2.33-release-notes]: https://github.com/box/box-content-preview/releases/tag/v2.33.1
