---
rank: 3
related_endpoints:
  - delete_collaboration_whitelist_entries_id
related_guides:
  - collaborations
  - collaboration-whitelists/list-whitelists
  - collaboration-whitelists/create-whitelist
related_pages: []
required_guides: []
related_resources: []
alias_paths: []
category_id: collaboration-whitelists
subcategory_id: null
is_index: false
id: collaboration-whitelists/delete-whitelist
type: guide
total_steps: 3
sibling_id: collaboration-whitelists
parent_id: collaboration-whitelists
next_page_id: collaboration-whitelists
previous_page_id: collaboration-whitelists/list-whitelists
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/collaboration-whitelists/delete-whitelist.md
---
<!-- alex disable whitelist -->

# コラボレーションホワイトリストの削除

コラボレーションホワイトリストのエントリを削除すると、自分の会社とそのホワイトリストに登録されているドメインとの間にコラボレーションを作成できなくなります。

コラボレーションホワイトリストを削除するには、削除リクエストに対してドメインホワイトリストエントリIDを指定します。このIDは、[新しいホワイトリストエントリの作成][create]または[社内のホワイトリストの取得][list]時に返されます。

<Samples id="delete_collaboration_whitelist_entries_id">

</Samples>

[create]: guide://collaboration-whitelists/create-whitelist

[list]: guide://collaboration-whitelists/list-whitelists
