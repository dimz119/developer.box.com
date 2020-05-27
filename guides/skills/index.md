---
rank: 170
alias_paths:
  - /docs/configure-a-box-skill
category_id: skills
subcategory_id: null
is_index: true
id: skills
type: guide
total_steps: 3
sibling_id: guides
parent_id: guides
next_page_id: ''
previous_page_id: skills/kit
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/skills/index.md
---
# Box Skills

Box Skillsでは、ファイルの基盤となるメタデータを強化することを目的として、Boxにアップロードされたファイル向けのカスタム処理サービスを結合できます。このシステムの利点は、さまざまなファイルの豊富な情報を保存しておき、自動化されたタスクや将来のプロセスに使用できることです。

一般に、Skillsアプリケーションのエンドツーエンドプロセスは以下のフローに従います。

<!-- markdownlint-disable line-length -->

| 手順                                                 | 説明                                                                                                                   |
| -------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| [アプリの設定](guide://applications/custom-skills/setup) | 会社全体または1つ以上のフォルダ内におけるアップロードイベントをリッスンするカスタムSkillsアプリを作成します。                                                           |
| [呼び出しURLの設定](guide://skills/invocation-url)        | カスタムSkillsアプリを作成したら、新しいファイルがアップロードされたときにBoxの通知が送られる呼び出しURLを設定します。                                                    |
| [イベントの解析](guide://skills/handle/payload)           | ファイルがフォルダにアップロード、コピー、または移動されると、呼び出しURLにイベントペイロードが送信されます。これにより、Boxにアップロードされたファイルにアクセスしてファイルにメタデータを保存するためのトークンが提供されます。 |
| [処理するファイルの送信](guide://skills/examples)             | ファイルコンテンツが、機械学習システムなどのインサイト処理を行う外部サービスに送信されます。                                                                       |
| [メタデータの保存](guide://skills/handle/metadata)         | 処理システムが完了すると、アップロードされているファイルに、そのインサイトがメタデータとして再保存されます。                                                               |

<!-- markdownlint-enable line-length -->

<Message>

# Skills Kit

Box Skillsとの統合を簡素化するため、上記の手順の複雑さを減らした[Skills Kit](guide://skills/kit)が提供されています。Skills Kitは現在、Nodeでのみ入手可能です。

</Message>
