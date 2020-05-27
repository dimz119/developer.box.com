---
rank: 3
related_endpoints: []
related_guides: []
related_pages: []
required_guides: []
related_resources: []
alias_paths:
  - /docs/build-a-box-skill
  - /page/box-skills-kit
  - /docs/creating-a-box-skill-using-the-box-skills-kit-node-sdk-and-serverless
category_id: skills
subcategory_id: null
is_index: false
id: skills/kit
type: guide
total_steps: 3
sibling_id: skills
parent_id: skills
next_page_id: skills
previous_page_id: skills/examples
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/skills/kit.md
---
# Box Skills Kit

Box Skills Kitは、Box Skills開発プロセスでよく必要とされる複雑な操作の多くを抽象化するために設計されたノードラッパーです。

以下のような複雑な操作を抽象化します。

* Boxから送信された通知ペイロードの関連データを解釈し、抽出する。
* [Skillsイベントペイロード](guide://skills/handle/payload)のダウンスコープトークンを使用して認証済みの呼び出しを実行し、Boxに保存されているファイルをダウンロードする。
* Boxに保存されているファイルにメタデータを再保存する際の操作を簡素化する。

Node Box Skillsキットの完全な概要については、[GitHubのドキュメント][github-skills-kit]を参照してください。

[github-skills-kit]: https://github.com/box/box-skills-kit-nodejs/tree/master/skills-kit-library
