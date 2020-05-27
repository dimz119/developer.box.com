---
rank: 1
alias_paths: []
category_id: uploads
subcategory_id: uploads/direct
is_index: true
id: uploads/direct
type: guide
total_steps: 2
sibling_id: uploads
parent_id: uploads
next_page_id: uploads/direct/file-version
previous_page_id: ''
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/uploads/direct/index.md
---
# 直接アップロード

Boxにファイルをアップロードする最も簡単な方法は、直接アップロードを使用することです。直接アップロードには、ファイルサイズが最大`50MB`、1回のAPI呼び出しでアップロードできるのは1ファイルまでという制限があります。

## アップロードドメイン

Boxへのアップロードは、通常のAPI呼び出しとは異なるドメイン(`upload.box.com`)を介して行われます。アップロードコードを作成するときは、この点に注意する必要があります。すべてのBox SDKで、API呼び出しに適切なドメインが選択されます。
