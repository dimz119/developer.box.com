---
category_id: metadata
subcategory_id: metadata/1-quick-start
is_index: false
id: metadata/quick-start/create-query
rank: 6
type: guide
total_steps: 7
sibling_id: metadata/quick-start
parent_id: metadata/quick-start
next_page_id: metadata/quick-start/next-steps
previous_page_id: metadata/quick-start/update-template
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/metadata/1-quick-start/6-create-query.md
---
# メタデータクエリの作成

最後の手順として、ファイル/フォルダに追加されているメタデータに基づいてそのファイルおよびフォルダを見つけるためのクエリを作成する方法を見てみましょう。

[メタデータクエリ][query]を使用すると、ファイルやフォルダに追加されているメタデータを検索して、そのファイルやフォルダを見つけることができます。この検索構文はSQLに似ており、強力な検索を実行するためにブール演算と比較演算子をサポートしています。

<CTA to="g://metadata/queries">
Learn more about creating queries

</CTA>

この例では、`customerInfo`メタデータテンプレートのインスタンスが適用されているファイルまたはファイルを検索するクエリを作成します。このリストを、預金総額が200,000ドルを超える顧客に属しているファイルに絞り込みます。

<Tabs>

<Tab title="cURL">

```curl
curl -X POST https://api.box.com/2.0/metadata_queries/execute_read \
     -H 'Authorization: Bearer <ACCESS_TOKEN>" '
     -H 'Content-Type: application/json'
     -d '{
       "from": "enterprise_123456.customerInfo",
       "query": "tav >= :value",
       "query_params": {
         "value": 200000
       },
       "ancestor_folder_id": "0"
     }'
```

</Tab>

</Tabs>

このAPIにより、一致したファイルとフォルダのリストのほか、そのファイルのクエリに一致したメタデータが返されます。

```json
{
  "entries":[
    {
      "item":{
        "type":"file",
        "id":"11111",
        "file_version":{
          "type":"file_version",
          "id":"222222",
          "sha1":"69888bb1bff455d1b2f8afea75ed1ff0b4879bf6"
        },
        "sequence_id":"0",
        "etag":"0",
        "sha1":"69888bb1bff455d1b2f8afea75ed1ff0b4879bf6",
        "name":"Contract.docx",
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
          "login":"admin@example.com"
        },
        "modified_by":{
          "type":"user",
          "id":"193973366",
          "name":"Box Admin",
          "login":"admin@example.com"
        },
        "owned_by":{
          "type":"user",
          "id":"193973366",
          "name":"Box Admin",
          "login":"admin@example.com"
        },
        "shared_link":null,
        "parent":{
          "type":"folder",
          "id":"0",
          "sequence_id":"0",
          "etag":"0",
          "name":"All Files"
        },
        "item_status":"active"
      },
      "metadata":{
        "enterprise_123456":{
          "someTemplate":{
            "name": "Box",
            "industry": "Technology",
            "tav": 1000000,
            "$id": "01234500-12f1-1234-aa12-b1d234cb567e",
            "$parent": "folder_12345,",
            "$scope": "enterprise_34567",
            "$template": "customerInfo",
            "$type": "customerInfo-6bcba49f-ca6d-4d2a-a758-57fe6edf44d0",
            "$typeVersion": 2,
            "$version": 1,
            "$canEdit": true
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

<CTA to="g://metadata/queries">
Learn more about metadata queries

</CTA>

<Next>

メタデータを使用してファイルにクエリを実行しました

</Next>

[query]: g://metadata/queries
