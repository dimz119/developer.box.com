---
rank: 1
related_endpoints:
  - get_events
  - options_events
related_guides:
  - events/for-enterprise
  - events/polling
  - events/pagination
required_guides: []
alias_paths: []
category_id: events
subcategory_id: null
is_index: false
id: events/for-user
type: guide
total_steps: 4
sibling_id: events
parent_id: events
next_page_id: events/for-enterprise
previous_page_id: ''
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/events/for-user.md
---
# ユーザーイベントの取得

ユーザーイベントを取得するには、任意のユーザーとして認証し、[`GET /events`](e://get_events) APIを呼び出します。

<Samples id="get_events">

</Samples>

<Message>

返されるイベントは、APIの作成に使用したアクセストークンを所有するユーザーのみを対象とします。別のユーザーのイベントフィードを取得するには、`as-user`ヘッダーか、そのユーザーの実際のアクセストークンを使用します。

</Message>

## ロングポーリング

ユーザーイベントストリームでは、[`OPTIONS /events` APIを介して][longpoll]Long pollingがサポートされます。

## ストリームタイプ

ユーザーイベントストリームでは、3つのタイプのストリームがサポートされます。

<!-- markdownlint-disable line-length -->

| ストリームタイプ  |                                                                                         |
| --------- | --------------------------------------------------------------------------------------- |
| `all`     | Returns everything for a user (default)                                                 |
| `changes` | Returns events that may cause file tree changes such as file updates or collaborations. |
| `sync`    | Is similar to changes but only applies to synced folders                                |

<!-- markdownlint-enable line-length -->

## 制限

Boxでのイベントの保存は無期限ではありません。

ユーザーイベントは2週間から2か月間保存され、その後、保存されたユーザーイベントは削除されます。エンタープライズイベントには、APIを介した場合は1年間、Box管理コンソールのエクスポートされたレポート経由の場合は7年間アクセスできます。

このフィードでは、完全な結果を迅速に返すことを重視しています。つまり、Boxではイベントを複数回または異なる順序で返す可能性があります。重複するイベントは、イベントIDによって識別できます。

## イベントタイプ

ユーザーに対して、以下のイベントがトリガーされます。

<!-- markdownlint-disable line-length -->

以下のイベントは、すべてのフィードで使用できます。

| イベント名                        | 説明                                                                              |
| ---------------------------- | ------------------------------------------------------------------------------- |
| `ITEM_CREATE`                | A folder or File was created                                                    |
| `ITEM_UPLOAD`                | A folder or File was uploaded                                                   |
| `ITEM_MOVE`                  | A file or folder was moved                                                      |
| `ITEM_COPY`                  | A file or folder was copied                                                     |
| `LOCK_CREATE`                | A file was locked                                                               |
| `LOCK_DESTROY`               | A file was unlocked. If a locked file is deleted, the source file will be null. |
| `ITEM_TRASH`                 | A file or folder was marked as deleted                                          |
| `ITEM_UNDELETE_VIA_TRASH`    | A file or folder was recovered out of the trash                                 |
| `COLLAB_ADD_COLLABORATOR`    | A collaborator was added to a folder                                            |
| `COLLAB_ROLE_CHANGE`         | A collaborator had their role changed                                           |
| `COLLAB_INVITE_COLLABORATOR` | A collaborator was invited on a folder                                          |
| `COLLAB_REMOVE_COLLABORATOR` | A collaborator was removed from a folder                                        |
| `ITEM_SYNC`                  | A folder was marked for sync                                                    |
| `ITEM_UNSYNC`                | A folder was unmarked for sync                                                  |
| `ITEM_RENAME`                | A file or folder was renamed                                                    |
| `ITEM_MAKE_CURRENT_VERSION`  | A previous version of a file was promoted to the current version                |
| `GROUP_ADD_USER`             | Added user to group                                                             |
| `GROUP_REMOVE_USER`          | Removed user from group                                                         |

以下のイベントは、`all`フィードでのみ使用できます。

| イベント名                    | 説明                                                        |
| ------------------------ | --------------------------------------------------------- |
| `COMMENT_CREATE`         | A comment was created on a folder, file, or other comment |
| `COMMENT_DELETE`         | A comment was deleted on folder, file, or other comment   |
| `ITEM_DOWNLOAD`          | A file or folder was downloaded                           |
| `ITEM_PREVIEW`           | A file was previewed                                      |
| `TASK_ASSIGNMENT_CREATE` | A task was assigned                                       |
| `TASK_CREATE`            | A task was created                                        |
| `ITEM_SHARED_CREATE`     | A file or folder was enabled for sharing                  |
| `ITEM_SHARED_UNSHARE`    | A file or folder was disabled for sharing                 |
| `ITEM_SHARED`            | A folder was shared                                       |
| `TAG_ITEM_CREATE`        | A Tag was added to a file or folder                       |
| `ENABLE_TWO_FACTOR_AUTH` | 2 factor authentication enabled by user.                  |
| `MASTER_INVITE_ACCEPT`   | Free user accepts invitation to become a managed user.    |
| `MASTER_INVITE_REJECT`   | Free user rejects invitation to become a managed user.    |
| `ACCESS_GRANTED`         | Granted Box access to account.                            |
| `ACCESS_REVOKED`         | Revoke Box access to account.                             |

<!-- markdownlint-enable line-length -->

## 匿名ユーザー

場合によっては、イベントフィードには、IDが`2`のユーザーが表示される可能性があります。これは、匿名ユーザーを表すBoxの内部識別子です。

匿名ユーザーは、ログインしていないユーザーです。この状況は、ユーザーがコンテンツを操作し、最初にログインを求められない場合にいつでも発生する可能性があります。たとえば、ユーザーが、公開共有リンクを使用してファイルをダウンロードするときなどです。

[longpoll]: g://events/polling
