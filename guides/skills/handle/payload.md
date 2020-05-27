---
rank: 2
related_endpoints: []
related_guides: []
related_pages: []
required_guides:
  - applications/custom-skills
related_resources:
  - skill_invocation
alias_paths: []
category_id: skills
subcategory_id: skills/handle
is_index: false
id: skills/handle/payload
type: guide
total_steps: 2
sibling_id: skills/handle
parent_id: skills/handle
next_page_id: skills/handle/metadata
previous_page_id: skills/handle
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/skills/handle/payload.md
---
# Skillsペイロードの解析

Skillsアプリが監視するフォルダに新しいファイルがアップロード、コピー、または移動されると、アプリケーションの設定および認証中に指定された呼び出しURLに、Boxからイベントペイロードが送られます。

このイベントペイロードには、アップロードされたファイルのコンテンツを読み込んで機械学習システムなどの処理システムに送信し、処理システムの完了後にファイルにメタデータを書き戻すために必要な情報がすべて含まれています。

## ペイロードの例

<Tabs>

<Tab title="Skills JSONペイロード">

<!-- markdownlint-disable line-length -->

```json
{
  "type": "skill_invocation",
  "skill": {
    "id": "137",
    "type": "skill",
    "name": "Hello World Skill",
    "api_key": "hxel2s12wd2h9r8ne103c4gjbqefofih"
  },
  "token": {
    "write": {
      "access_token": "REDACTED",
      "expires_in": 1540924150,
      "restricted_to": "[{\"scope\":\"gcm\"},{\"scope\":\"item_upload\",\"object_id\":340708371275,\"object_type\":\"file\"},{\"scope\":\"manage_skill_invocations\"}]",
      "token_type": "bearer"
    },
    "read": {
      "access_token": "REDACTED",
      "expires_in": 1540924150,
      "restricted_to": "[{\"scope\":\"gcm\"},{\"scope\":\"item_read\",\"object_id\":340708371275,\"object_type\":\"file\"}]",
      "token_type": "bearer"
    }
  },
  "status": {
    "state": "invoked",
    "message": "",
    "error_code": "",
    "additional_info": ""
  },
  "id": "fd1d2e53-35f5-41fb-9c25-4ba326daf2f9_341016304",
  "created_at": "2018-10-29T11:29:09-07:00",
  "trigger": "FILE_CONTENT",
  "enterprise": {
    "type": "enterprise",
    "id": "94713934",
    "name": "Important Photo"
  },
  "source": {
    "type": "file",
    "id": "340708371275",
    "name": "200386_main.jpg",
    "sequence_id": "0",
    "file_version": {
      "id": "359740326875"
    },
    "owner_enterprise_id": "94713934",
    "parent": {
      "id": "56598494243"
    },
    "old_parent_id": "56598494243",
    "collab_accessible_by": {
      "type": "",
      "id": "",
      "name": "",
      "login": ""
    },
    "size": 150031
  },
  "event": {
    "event_id": "fd1d2e53-35f5-41fb-9c25-4ba326daf2f9",
    "event_type": "ITEM_UPLOAD",
    "created_at": "2018-10-29T11:29:09-07:00",
    "created_by": {
      "type": "user",
      "id": "6122733934",
      "name": "Box Skills",
      "login": "boxskills-dev@boxdemo.com"
    },
    "source": {
      "type": "file",
      "id": "340708371275",
      "name": "200386_main.jpg",
      "sequence_id": "0",
      "file_version": {
        "id": "359740326875"
      },
      "owner_enterprise_id": "94713934",
      "parent": {
        "id": "56598494243"
      },
      "old_parent_id": "56598494243",
      "collab_accessible_by": {
        "type": "",
        "id": "",
        "name": "",
        "login": ""
      }
    }
  },
  "parameters": {}
}
```

<!-- markdownlint-enable line-length -->

</Tab>

</Tabs>

## ペイロードのコンポーネント

Skillsイベントペイロードは、以下の最上位オブジェクトに分類できます。

<!-- markdownlint-disable line-length -->

| Object       | 説明                                                                                                                                                                                                                                                                                                         |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `type`       | The type of event, which will always be `skill_invocation`.                                                                                                                                                                                                                                                |
| `skill`      | Details of the Custom Skills app that sent the event. This information should be used to verify the source of the request.                                                                                                                                                                                 |
| `token`      | Two [downscoped tokens](guide://authentication/access-tokens/downscope) for accessing the uploaded file. The `read` token should be used to download the file for the processing system, and the `write` token should be used to store metadata back to the uploaded file once the processing is complete. |
| `status`     | The status of the event.                                                                                                                                                                                                                                                                                   |
| `id`         | The unique event ID                                                                                                                                                                                                                                                                                        |
| `created_at` | When the event was created.                                                                                                                                                                                                                                                                                |
| `trigger`    | The type of action that triggered the event.                                                                                                                                                                                                                                                               |
| `enterprise` | Information about the Enterprise where the event was triggered from. This information should be used to verify the source of the request.                                                                                                                                                                  |
| `source`     | Information on the file that was uploaded that triggered the event. Use this information to make API requests to download the file and store metadata back to the file.                                                                                                                                    |
| `event`      | The Skills event information.                                                                                                                                                                                                                                                                              |
| `parameters` | Additional details that may be sent with the event.                                                                                                                                                                                                                                                        |

<!-- markdownlint-enable line-length -->

ファイルをダウンロードし、そのファイルのメタデータを更新するには、少なくとも以下の3つの情報が必要です。

* **読み取りトークン**: `token`オブジェクト内の読み取りトークンを使用すると、[ファイルをダウンロード](endpoint://get_files_id)エンドポイントを呼び出し、処理システムに送信するファイルコンテンツをダウンロードできます。
* **書き込みトークン**: ファイル処理の終了後、`token`オブジェクト内の書き込みトークンを使用すると、[ファイルメタデータを作成](e://post_files_id_metadata_id_id)エンドポイントを呼び出すことができます。
* **ファイルID**: `source`オブジェクト内にあります。上記の2つのエンドポイントで、ダウンロードするファイルまたはメタデータを適用するファイルを特定するには、このファイルIDが必要です。
