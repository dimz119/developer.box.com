---
category_id: metadata
subcategory_id: metadata/3-fields
is_index: true
id: metadata/fields
rank: 3
type: guide
total_steps: 5
sibling_id: metadata
parent_id: metadata
next_page_id: metadata/fields/enum
previous_page_id: metadata/fields/float
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/metadata/3-fields/0-index.md
---
# メタデータフィールド

[メタデータテンプレートフィールド][r_field]には、メタデータテンプレート内の特定のデータが記載されています。たとえば、請求書のIDを`invoiceData`テンプレートの`id`フィールドとして表すことができます。

すべてのメタデータテンプレートには`field`オブジェクトのリストが含まれており、各フィールドは次のいずれかのタイプになります。

<!-- markdownlint-disable line-length -->

|                                                   |                                               |
| ------------------------------------------------- | --------------------------------------------- |
| [`string`](g://metadata/fields/string)            | A text field                                  |
| [`float`](g://metadata/fields/float)              | A numeric input field                         |
| [`date`](g://metadata/fields/date)                | A date picker field                           |
| [`enum`](g://metadata/fields/enum)                | A dropdown list for selecting 1 value         |
| [`multiSelect`](g://metadata/fields/multi-select) | A dropdown list for selecting multiple values |

<!-- markdownlint-enable line-length -->

## 基本的なフィールドタイプ

基本的なフィールドタイプは、テキストフィールドの場合は`string`、数値フィールドの場合は`float`、日時選択フィールドの場合`date`です。

## リストのフィールドタイプ

さらに、メタデータテンプレートでは、ドロップダウンリストを表す2つのフィールドタイプをサポートしています。`enum`フィールドは、ユーザーが選択できるあらかじめ定義された項目のリストを表すのに対し、`multiSelect`フィールドは、ユーザーが複数の値を選択できる項目のリストを表します。

[r_field]: r://metadata_field
