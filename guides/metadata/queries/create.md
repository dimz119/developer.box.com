---
related_endpoints:
  - post_metadata_queries_execute_read
category_id: metadata
subcategory_id: metadata/5-queries
is_index: false
id: metadata/queries/create
rank: 1
type: guide
total_steps: 7
sibling_id: metadata/queries
parent_id: metadata/queries
next_page_id: metadata/queries/syntax
previous_page_id: ''
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/metadata/5-queries/1-create.md
---
# クエリの作成

メタデータクエリとは、`/metadata_queries/execute_read`エンドポイントに対する`POST`リクエストで、その本文にはメタデータクエリのすべてのパーツが含まれています。ここで最も重要なのは、検索対象のテンプレートを指定する`from`属性、検索するフォルダを指定する`ancestor_folder_id`、検索に使用するすべてのテンプレートフィールドを決定する`query`です。

```curl
curl -X POST https://api.box.com/2.0/metadata_queries/execute_read \
     -H 'Authorization: Bearer <ACCESS_TOKEN>" '
     -H 'Content-Type: application/json'
     -d '{
       "from": "enterprise_123456.contractTemplate",
       "query": "amount >= :value",
       "query_params": {
         "value": 100
       },
       "ancestor_folder_id": "5555",
       "use_index": "amountAsc",
       "order_by": [
         {
           "field_key": "amount",
           "direction": "asc"
         }
       ],
       "limit": 100
     }'
```

使用可能なすべてのパラメータの詳細については、Boxの他の[メタデータクエリガイド](g://metadata/queries)または関連する[エンドポイントリファレンス](e://post_metadata_queries_execute_read)を参照してください。

<CTA to="g://metadata/queries/syntax">
Learn more about the query syntax

</CTA>

## レスポンス

クエリに一致するすべてのファイルまたはフォルダがあれば、API応答で返されます。応答の本文はJSONオブジェクトで、各ファイルまたはフォルダの`entries`のリストと、次の検索結果ページを見つけるための`next_marker`値が含まれています。各エントリには、ファイルまたはフォルダを表す`item`のほか、ファイルまたはフォルダに追加されているメタデータを含む`metadata`インスタンスが含まれます。

```json
{
  "entries":[
    {
      "item":{
        "type":"file",
        "id":"1617554169109",
        "file_version":{
          "type":"file_version",
          "id":"1451884469385",
          "sha1":"69888bb1bff455d1b2f8afea75ed1ff0b4879bf6"
        },
        "sequence_id":"0",
        "etag":"0",
        "sha1":"69888bb1bff455d1b2f8afea75ed1ff0b4879bf6",
        "name":"My Contract.docx",
        "description":"",
        "size":25600,
        "path_collection":{
          "total_count":4,
          "entries":[
            {
              "type":"folder",
              "id":"0",
              "sequence_id":null,
              "etag":null,
              "name":"All Files"
            },
            {
              "type":"folder",
              "id":"15017998644",
              "sequence_id":"0",
              "etag":"0",
              "name":"Contracts"
            },
            {
              "type":"folder",
              "id":"15286891196",
              "sequence_id":"1",
              "etag":"1",
              "name":"North America"
            },
            {
              "type":"folder",
              "id":"16125613433",
              "sequence_id":"0",
              "etag":"0",
              "name":"2017"
            }
          ]
        },
        "created_at":"2017-04-20T12:55:27-07:00",
        "modified_at":"2017-04-20T12:55:27-07:00",
        "trashed_at":null,
        "purged_at":null,
        "content_created_at":"2017-01-06T17:59:01-08:00",
        "content_modified_at":"2017-01-06T17:59:01-08:00",
        "created_by":{
          "type":"user",
          "id":"193973366",
          "name":"Box Admin",
          "login":"admin@company.com"
        },
        "modified_by":{
          "type":"user",
          "id":"193973366",
          "name":"Box Admin",
          "login":"admin@company.com"
        },
        "owned_by":{
          "type":"user",
          "id":"193973366",
          "name":"Box Admin",
          "login":"admin@company.com"
        },
        "shared_link":null,
        "parent":{
          "type":"folder",
          "id":"16125613433",
          "sequence_id":"0",
          "etag":"0",
          "name":"2017"
        },
        "item_status":"active"
      },
      "metadata":{
        "enterprise_123456":{
          "someTemplate":{
            "$parent":"file_161753469109",
            "$version":0,
            "customerName":"Phoenix Corp",
            "$type":"someTemplate-3d5fcaca-f496-4bb6-9046-d25c37bc5594",
            "$typeVersion":0,
            "$id":"ba52e2cc-371d-4659-8d53-50f1ac642e35",
            "amount":100,
            "claimDate":"2016-04-10T00:00:00Z",
            "region":"West",
            "$typeScope":"enterprise_123456"
          }
        }
      }
    }
  ],
  "limit": 20,
  "next_marker":"AAAAAmVYB1FWec8GH6yWu2nwmanfMh07IyYInaa7DZDYjgO1H4KoLW29vPlLY173OKsci6h6xGh61gG73gnaxoS+o0BbI1/h6le6cikjlupVhASwJ2Cj0tOD9wlnrUMHHw3/ISf+uuACzrOMhN6d5fYrbidPzS6MdhJOejuYlvsg4tcBYzjauP3+VU51p77HFAIuObnJT0ff"
}
```

このAPIはデフォルトで、ページあたり`20`個の項目を返しますが、マーカーベースのページ割りを使用すると、さらに多くの項目をリクエストできます。

<CTA to="g://metadata/queries/pagination">
Learn more about paginating query results

</CTA>
