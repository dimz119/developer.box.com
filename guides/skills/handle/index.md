---
rank: 1
related_guides:
  - skills/handle/payload
  - skills/handle/metadata
alias_paths: []
category_id: skills
subcategory_id: skills/handle
is_index: true
id: skills/handle
type: guide
total_steps: 2
sibling_id: skills
parent_id: skills
next_page_id: skills/handle/payload
previous_page_id: ''
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/skills/handle/index.md
---
# Skillsイベントの処理

[呼び出しURL](guide://skills/invocation-url)として設定されているアプリケーションまたはサイト内では、一般に以下の2つのペイロードを処理する必要があります。

* [イベントペイロード](guide://skills/handle/payload): スキルが設定されているフォルダに新しいファイルがアップロード、コピー、または移動されたときのBox Skills通知。
* [メタデータペイロード](guide://skills/handle/metadata): 構築してBox内の元のファイルに保存し直す必要があるメタデータペイロード。このリクエストを構成するデータは、機械学習システムなどのファイル処理システムから取得されます。
