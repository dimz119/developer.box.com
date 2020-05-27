---
rank: 2
related_endpoints:
  - get_events
  - options_events
related_guides:
  - events/for-user
  - events/polling
  - events/pagination
required_guides: []
alias_paths: []
category_id: events
subcategory_id: null
is_index: false
id: events/for-enterprise
type: guide
total_steps: 4
sibling_id: events
parent_id: events
next_page_id: events/polling
previous_page_id: events/for-user
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/events/for-enterprise.md
---
# エンタープライズイベントの取得

エンタープライズイベントを取得するには、管理者権限を持つユーザーを認証し、`stream_type`を`admin_logs`に設定して[`GET /events`](e://get_events) APIを呼び出します。

<Samples id="get_events">

</Samples>

<Message>

このAPIを使用するには、ユーザーは**新規レポートの実行と既存レポートへのアクセス**のための権限を持つ、会社の管理者または共同管理者である必要があります。

</Message>

## イベントタイプによるフィルタ

エンタープライズイベントフィードでは、イベントタイプによるフィルタがサポートされています。

```curl
curl https://api.box.com/2.0/events?event_type=LOGIN,FAILED_LOGIN \
  -H "authorization: Bearer ACCESS_TOKEN"
```

イベントタイプの完全なリストについては、以下を参照してください。

## 制限

管理者イベントフィードでは、ロングポーリングがサポートされません。イベントに対するロングポーリングでは、ユーザーイベントフィードを使用します。

Boxでのイベントの保存は無期限ではありません。

ユーザーイベントは2週間から2か月間保存され、その後、保存されたユーザーイベントは削除されます。エンタープライズイベントには、APIを介した場合は1年間、Box管理コンソールのエクスポートされたレポート経由の場合は7年間アクセスできます。

このフィードでは、レイテンシよりも完全性を重視しています。つまり、Boxでは管理者イベントをユーザーフィードよりも高いレイテンシで配信することがあります。ユーザーイベントストリームとは違い、管理者イベントストリームでは、特定のイベントに対するフィルタが可能ですが、ロングポーリングはサポートされていません。

## イベントタイプ

エンタープライズに対して、以下のイベントがトリガーされます。

<!-- markdownlint-disable line-length -->

| イベント名                                          | 説明                                                                                              |
| ---------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| `GROUP_ADD_USER`                               | Added user to group                                                                             |
| `NEW_USER`                                     | Created user                                                                                    |
| `GROUP_CREATION`                               | Created new group                                                                               |
| `GROUP_DELETION`                               | Deleted group                                                                                   |
| `DELETE_USER`                                  | Deleted user                                                                                    |
| `GROUP_EDITED`                                 | Edited group                                                                                    |
| `EDIT_USER`                                    | Edited user                                                                                     |
| `GROUP_REMOVE_USER`                            | Removed user from group                                                                         |
| `ADMIN_LOGIN`                                  | Admin login                                                                                     |
| `ADD_DEVICE_ASSOCIATION`                       | Added device association                                                                        |
| `CHANGE_FOLDER_PERMISSION`                     | Edit the permissions on a folder                                                                |
| `FAILED_LOGIN`                                 | Failed login                                                                                    |
| `LOGIN`                                        | Login                                                                                           |
| `REMOVE_DEVICE_ASSOCIATION`                    | Removed device association                                                                      |
| `DEVICE_TRUST_CHECK_FAILED`                    | Device Trust check failed                                                                       |
| `TERMS_OF_SERVICE_ACCEPT`                      | Accepted terms                                                                                  |
| `TERMS_OF_SERVICE_REJECT`                      | Rejected terms                                                                                  |
| `FILE_MARKED_MALICIOUS`                        | Virus found on a file. Event is only received by enterprises that have opted in to be notified. |
| `COPY`                                         | Copied                                                                                          |
| `DELETE`                                       | Deleted                                                                                         |
| `DOWNLOAD`                                     | Downloaded                                                                                      |
| `EDIT`                                         | Edited                                                                                          |
| `LOCK`                                         | Locked                                                                                          |
| `MOVE`                                         | Moved                                                                                           |
| `PREVIEW`                                      | Previewed                                                                                       |
| `RENAME`                                       | A file or folder name or description is changed.                                                |
| `STORAGE_EXPIRATION`                           | Set file auto-delete                                                                            |
| `UNDELETE`                                     | Restored                                                                                        |
| `UNLOCK`                                       | Unlocked                                                                                        |
| `UPLOAD`                                       | Uploaded                                                                                        |
| `SHARE`                                        | Enabled shared links                                                                            |
| `ITEM_SHARED_UPDATE`                           | Share links settings updated                                                                    |
| `UPDATE_SHARE_EXPIRATION`                      | Extend shared link expiration                                                                   |
| `SHARE_EXPIRATION`                             | Set shared link expiration                                                                      |
| `UNSHARE`                                      | Shared link removed                                                                             |
| `COLLABORATION_ACCEPT`                         | Accepted invites                                                                                |
| `COLLABORATION_ROLE_CHANGE`                    | Changed user roles                                                                              |
| `UPDATE_COLLABORATION_EXPIRATION`              | Extend collaborator expiration                                                                  |
| `COLLABORATION_REMOVE`                         | Removed collaborators                                                                           |
| `COLLABORATION_INVITE`                         | Invited                                                                                         |
| `COLLABORATION_EXPIRATION`                     | Set collaborator expiration                                                                     |
| `EXTERNAL_COLLAB_SECURITY_SETTINGS`            | Changes in external collaboration security settings                                             |
| `ITEM_SYNC`                                    | Synced folder                                                                                   |
| `ITEM_UNSYNC`                                  | Unmarked folder for synced                                                                      |
| `ADD_LOGIN_ACTIVITY_DEVICE`                    | A user is logging in from a device we haven’t seen before                                       |
| `REMOVE_LOGIN_ACTIVITY_DEVICE`                 | We invalidated a user session associated with an app                                            |
| `USER_AUTHENTICATE_OAUTH2_ACCESS_TOKEN_CREATE` | An OAuth 2.0 access token has been created                                                      |
| `OAUTH2_ACCESS_TOKEN_REVOKE`                   | An OAuth 2.0 access token has been revoked                                                      |
| `CHANGE_ADMIN_ROLE`                            | When an admin role changes for a user                                                           |
| `CONTENT_WORKFLOW_UPLOAD_POLICY_VIOLATION`     | A collaborator violated an admin-set upload policy                                              |
| `METADATA_INSTANCE_CREATE`                     | Creation of metadata instance.                                                                  |
| `METADATA_INSTANCE_UPDATE`                     | Update of metadata instance.                                                                    |
| `METADATA_INSTANCE_DELETE`                     | Deletion of metadata instance.                                                                  |
| `SHIELD_ALERT`                                 | Shield detected an anomalous  download, session, or location based on enterprise Shield Rules.  |
| `TASK_ASSIGNMENT_UPDATE`                       | Update of a task assignment.                                                                    |
| `TASK_ASSIGNMENT_CREATE`                       | A task assignment is created.                                                                   |
| `TASK_ASSIGNMENT_DELETE`                       | A task assignment is deleted.                                                                   |
| `TASK_CREATE`                                  | A task is created.                                                                              |
| `TASK_UPDATE`                                  | A task's comment was edited.                                                                    |
| `COMMENT_CREATE`                               | A comment is created on a file.                                                                 |
| `COMMENT_DELETE`                               | A comment is deleted on a file.                                                                 |
| `GROUP_ADD_ITEM`                               | An item is added to a group.                                                                    |
| `DATA_RETENTION_REMOVE_RETENTION`              | Retention is removed.                                                                           |
| `DATA_RETENTION_CREATE_RETENTION`              | Retention is created.                                                                           |
| `RETENTION_POLICY_ASSIGNMENT_ADD`              | A retention policy assignment is added.                                                         |
| `LEGAL_HOLD_ASSIGNMENT_CREATE`                 | A legal hold assignment is created.                                                             |
| `LEGAL_HOLD_ASSIGNMENT_DELETE`                 | A legal hold assignment is deleted.                                                             |
| `LEGAL_HOLD_POLICY_CREATE`                     | A legal hold policy is created.                                                                 |
| `LEGAL_HOLD_POLICY_UPDATE`                     | A legal hold policy is updated.                                                                 |
| `LEGAL_HOLD_POLICY_DELETE`                     | A legal hold policy is deleted.                                                                 |
| `CONTENT_WORKFLOW_SHARING_POLICY_VIOLATION`    | There is a sharing policy violation.                                                            |
| `APPLICATION_PUBLIC_KEY_ADDED`                 | An application public key is added.                                                             |
| `APPLICATION_PUBLIC_KEY_DELETED`               | An application public key is deleted.                                                           |
| `APPLICATION_CREATED`                          | A new application was created in the Box developer console.                                     |
| `CONTENT_WORKFLOW_POLICY_ADD`                  | A content policy is added.                                                                      |
| `CONTENT_WORKFLOW_AUTOMATION_ADD`              | An automation is added.                                                                         |
| `CONTENT_WORKFLOW_AUTOMATION_DELETE`           | An automation is deleted.                                                                       |
| `EMAIL_ALIAS_CONFIRM`                          | A user email alias is confirmed.                                                                |
| `EMAIL_ALIAS_REMOVE`                           | A user email alias is removed.                                                                  |
| `WATERMARK_LABEL_CREATE`                       | A watermark is added to a file.                                                                 |
| `WATERMARK_LABEL_DELETE`                       | A watermark is removed from a file.                                                             |
| `ACCESS_GRANTED`                               | A user has granted Box access to their account.                                                 |
| `ACCESS_REVOKED`                               | A user has revoked Box access to their account.                                                 |
| `METADATA_TEMPLATE_CREATE`                     | Creation of metadata template instance.                                                         |
| `METADATA_TEMPLATE_UPDATE`                     | Update of metadata template instance.                                                           |
| `METADATA_TEMPLATE_DELETE`                     | Deletion of metadata template instance.                                                         |
| `ITEM_OPEN`                                    | Item was opened.                                                                                |
| `ITEM_MODIFY`                                  | Item was modified.                                                                              |
| `CONTENT_WORKFLOW_ABNORMAL_DOWNLOAD_ACTIVITY`  | When a policy set in the Admin console is triggered.                                            |
| `GROUP_REMOVE_ITEM`                            | Folders were removed from a group in the Admin console.                                         |
| `GROUP_ADD_ITEM`                               | Folders were added to a group in the Admin console.                                             |
| `FILE_WATERMARKED_DOWNLOAD`                    | A watermarked file was downloaded.                                                              |
| `ENTERPRISE_APP_AUTHORIZATION_UPDATE`          | When a JWT application has been authorized or reauthorized                                      |

<!-- markdownlint-enable line-length -->

## 匿名ユーザー

場合によっては、イベントフィードには、IDが`2`のユーザーが表示される可能性があります。これは、匿名ユーザーを表すBoxの内部識別子です。

匿名ユーザーは、ログインしていないユーザーです。この状況は、ユーザーがコンテンツを操作し、最初にログインを求められない場合にいつでも発生する可能性があります。たとえば、ユーザーが、公開共有リンクを使用してファイルをダウンロードするときなどです。
