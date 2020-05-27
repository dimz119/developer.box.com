---
related_endpoints:
  - post_metadata_queries_execute_read
category_id: metadata
subcategory_id: metadata/5-queries
is_index: false
id: metadata/queries/syntax
rank: 2
type: guide
total_steps: 7
sibling_id: metadata/queries
parent_id: metadata/queries
next_page_id: metadata/queries/pagination
previous_page_id: metadata/queries/create
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/metadata/5-queries/2-syntax.md
---
# クエリ構文

メタデータクエリAPIのクエリ構文はSQLデータベースのクエリ構文と似ています。契約金額が100ドルを超える契約メタデータテンプレートに一致するすべてのファイルとフォルダに対してクエリを実行するには、以下のメタデータクエリを作成します。

```json
{
  "from": "enterprise_123456.contractTemplate",
  "query": "amount >= :value",
  "query_params": {
    "value": 100
  },
  "ancestor_folder_id": "5555"
}
```

この場合、`from`値はメタデータテンプレートの`scope`と`templateKey`を表し、`ancestor_folder_id`はサブフォルダを含む検索範囲となるフォルダIDを表します。

## `query`パラメータ

`query`パラメータは、選択したメタデータインスタンスに対して実行する、SQLに似たクエリを表します。このパラメータは省略可能で、このパラメータを指定しない場合、APIはこのテンプレートに対してすべてのファイルとフォルダを返します。

左側の各フィールド名(`amount`など)は、関連付けられたメタデータテンプレートのフィールドの`key`に一致する必要があります。つまり、関連付けられたメタデータインスタンスに実際に存在するフィールドだけを検索できます。その他のフィールド名を指定するとエラーが発生し、エラーが返されます。

### `query_params`パラメータ

クエリ文字列への動的な値の埋め込みをわかりやすくするために、`:value`のように、コロン構文を使用して引数を定義できます。たとえば、次のように指定された各引数では、`query_params`オブジェクトにそのキーを使用した後続の値が必要です。

```json
{
  ...,
  "query": "amount >= :amount AND country = :country",
  "query_params": {
    "amount": 100,
    "country": "US"
  },
  ...
}
```

### 論理演算子

クエリでは、以下の論理演算子がサポートされます。

<!-- markdownlint-disable line-length -->

| 演算子        |                                                                                                                                                                                                                                                      |     |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- |
| `AND`      | Matches when all the conditions separated by `AND` are `TRUE`                                                                                                                                                                                        |     |
| `OR`       | Matches when any of the conditions separated by `OR` is `TRUE`                                                                                                                                                                                       |     |
| `NOT`      | Matches when the preceding condition(s) is **not** `TRUE`                                                                                                                                                                                            |     |
| `LIKE`     | Matches when the template field value matches a pattern. Only supported for string values. See [pattern matching](#pattern-matching) for more details                                                                                                |     |
| `NOT LIKE` | Matches when the template field value does **not** match a pattern. Only supported for string values. See [pattern matching](#pattern-matching) for more details                                                                                     |     |
| `ILIKE`    | Identical to `LIKE` but case insensitive                                                                                                                                                                                                             |     |
| `NOT LIKE` | Identical to `NOT LIKE` but case insensitive                                                                                                                                                                                                         |     |
| `IN`       | Matches when the template field value is equal to any one of a list of arguments provided. The format for this requires each item in the list to be an explicitly defined `query_params` argument, for example `amount NOT IN (:arg1, :arg2, :arg3)` |     |
| `NOT IN`   | Similar to `IN` but when the template field value matches none of the arguments provided in the list                                                                                                                                                 |     |
| `IS NULL`  | Matches when the template field value is `null`                                                                                                                                                                                                      |     |
| `IS NOT`   | Matches when the template field value is not `null`                                                                                                                                                                                                  |     |

<!-- markdownlint-enable line-length -->

<Message notice>

`ILIKE`演算子を使用した場合を除き、`string`または`enum`フィールドでの一致は、どれも大文字小文字が区別されます。

</Message>

### 比較演算子

クエリでは、以下の比較演算子がサポートされます。

<!-- markdownlint-disable line-length -->

| 演算子  |                                                                                      |
| ---- | ------------------------------------------------------------------------------------ |
| `=`  | Ensures a template field value is **equal to** the a specified value                 |
| `>`  | Ensures a template field value is **greater than to** the a specified value          |
| `<`  | Ensures a template field value is **greater than to** the a specified value          |
| `>=` | Ensures a template field value is **greater than or equal to** the a specified value |
| `<=` | Ensures a template field value is **less than or equal to** the a specified value    |
| `<>` | Ensures a template field value is **not equal to** to the a specified value          |

<!-- markdownlint-enable line-length -->

<Message warning>

ビット単位演算子および算術演算子は、メタデータクエリAPIではサポートされていません。

</Message>

### パターン一致

`LIKE`、`NOT LIKE`、`ILIKE`および`NOT ILIKE`演算子は、パターンに対して文字列が一致するかどうかを照合します。このパターンでは、以下の予約文字をサポートします。

* `%` パーセント記号は0個、1個または複数個の文字を表します。たとえば、`%Contract`の場合、`Contract`、`Sales Contract`は一致しますが、`Contract (Sales)`は一致しません。
* `_` アンダースコアは1文字を表します。たとえば、`Bo_`の場合、`Box`、`Bot`は一致しますが、`Bots`は一致しません。

上記の文字はどちらも、他の文字の前後または文字の間に使用できます。パターンには、複数の予約文字を含めることができます。たとえば、`Box% (____)`の場合は`Box Contract (2020)`が一致します。

<Message notice>

`%`または`_`文字を、その文字として照合する必要がある場合は、エスケープするためにバックスラッシュ文字`\`を使用できます。たとえば、`20\%`の場合は、リテラル値`20%`が一致します。

</Message>
