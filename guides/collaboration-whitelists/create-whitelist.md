---
rank: 1
related_endpoints:
  - post_collaboration_whitelist_entries
related_guides:
  - collaborations
  - collaboration-whitelists/list-whitelists
  - collaboration-whitelists/delete-whitelist
related_pages: []
required_guides: []
related_resources: []
alias_paths: []
category_id: collaboration-whitelists
subcategory_id: null
is_index: false
id: collaboration-whitelists/create-whitelist
type: guide
total_steps: 3
sibling_id: collaboration-whitelists
parent_id: collaboration-whitelists
next_page_id: collaboration-whitelists/list-whitelists
previous_page_id: ''
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/collaboration-whitelists/create-whitelist.md
---
<!-- alex disable whitelist -->

# コラボレーションホワイトリストの作成

コラボレーションホワイトリストを作成すると、社内でコラボレーションを作成できる新しいドメイン(`example.com`など)が設定されます。

<Samples id="post_collaboration_whitelist_entries">

</Samples>

[コラボレーションホワイトリストの作成エンドポイント](endpoint://post_collaboration_whitelist_entries)には、コラボレーションを許可する`domain`と、以下のように設定できる`direction`が必要です。

* `inbound`: 外部ユーザーが社内のコンテンツでコラボレーションできるかどうか。
* `outbound`: 社内の管理対象ユーザーが外部の会社内で所有されているコンテンツでコラボレーションできるかどうか。
* `both`: 上記の両方。

新しいコラボレーションホワイトリストを設定する場合はbothパラメータを指定します。

<Samples id="post_collaboration_whitelist_entries">

</Samples>
