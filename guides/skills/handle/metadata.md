---
rank: 3
related_endpoints: []
related_guides: []
related_pages: []
required_guides:
  - skills/handle/payload
related_resources: []
alias_paths: []
category_id: skills
subcategory_id: skills/handle
is_index: false
id: skills/handle/metadata
type: guide
total_steps: 2
sibling_id: skills/handle
parent_id: skills/handle
next_page_id: ''
previous_page_id: skills/handle/payload
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/skills/handle/metadata.md
---
# Skillsメタデータの書き込み

機械学習システムからデータインサイトを取得できたら、次の手順はBoxに保存されているファイルのメタデータとしてデータを書き戻すことです。このプロセスには以下の3つの手順が含まれます。

1. 元の[イベントペイロード](guide://skills/handle/payload)を介して送信された書き込みトークンを使用して、Boxクライアントを設定します。
2. Skillsメタデータを適切な形式で準備します。
3. メタデータをファイルに書き戻します。

## 書き込みトークンを使用したBoxクライアントの設定

[イベントペイロード](guide://skills/handle/payload)から書き込みトークンが抽出されたら、開発者トークンの場合と同じ方法で新しい基本のBoxクライアントを作成できます。このクライアントを利用して、ファイルにメタデータを書き込むことができます。

<Samples id="x_auth" variant="init_with_dev_token">

</Samples>

## Skillsメタデータの準備

Skillsメタデータは、グローバルに利用可能な`boxSkillsCards`という名前のメタデータテンプレートを使用します。このテンプレートは、関連ファイルに保存されるJSON構造の特定の形式に従います。

```json
"cards": [{
    "created_at": "{{CURRENT_TIMESTAMP}}",
    "type": "skill_card",
    "skill_card_type": "{{SKILLS_CARD_TYPE}}",
    "skill_card_title": {
        "message": "{{CARD_TITLE}}"
    },
    "skill": {
        "type": "service",
        "id": "{{SKILL_ID}}"
    },
    "invocation": {
        "type": "skill_invocation",
        "id": "{{FILE_ID}}"
    },
    "duration": "{{DURATION_IN_SECONDS}}",
    "entries": "{{CARD_ENTRIES}}"
}]}
```

ルートの`cards`オブジェクトはオブジェクトの配列であるため、一度に1つまたは複数のカードをメタデータに適用できます。

上記のサンプルには、`{{ VALUE }}`としてラップされた動的な値が複数あります。これを、以下に説明する値に置き換える必要があります。

* `CURRENT_TIMESTAMP`: メタデータが作成された時刻。現在のタイムスタンプを設定する必要があります。
* `SKILLS_CARD_TYPE`: 作成するカードのタイプ。利用可能なオプションについては、下の「**Skillsカードのタイプ**」セクションを参照してください。
* `CARD_TITLE`: 書き込まれるカードのタイトル。コンテンツに付ける任意のタイトルです。
* `SKILL_ID`: スキルのID。Skillsアプリケーション名を設定する必要があります。
* `FILE_ID`: メタデータを書き込むファイルのID。[イベントペイロード](guide://skills/handle/payload)から抽出されます。
* `DURATION_IN_SECONDS` (省略可): 動画ファイルや音声ファイルなど、継続時間を持つコンテンツがある場合にのみ使用するオプションパラメータです。使用する場合、コンテンツの長さを秒単位で指定する必要があります。
* `CARD_ENTRIES`: 機械学習システムから取得されるデータ。この値はオブジェクトの配列です。下の「**Skillsカードエントリ**」を参照してください。

### Skillsカードのタイプ

以下に、Box Skillsで使用できるすべてのカードタイプを示します。メタデータJSONオブジェクトの`SKILLS_CARD_TYPE`値を、以下の表に示すカードのメタデータ値に置き換えてください。

<!-- markdownlint-disable line-length -->

| カードタイプ   | メタデータ値       | 説明                                                                                                                                      |
| -------- | ------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| キーワード    | `keyword`    | Presents a list of keywords to be shown in association with a file, optionally with relevant timestamps for when those keywords appear. |
| トランスクリプト | `transcript` | Displays a set of images with their relevant timestamps on a media file.                                                                |
| フェイス     | `timeline`   | Displays a transcript with the corresponding timestamps.                                                                                |

<!-- markdownlint-enable line-length -->

### Skillsカードエントリ

`CARD_ENTRIES`フィールドには、特定の形式のカードタイプのすべてのデータが含まれます。エントリは、関連データを含むオブジェクトの配列である必要があります。エントリオブジェクトには、カードタイプに応じて以下の形式のいずれかを使用します。

#### キーワードカードエントリ

<ImageFrame border center shadow width="200">

![画像](./skills-card-keyword.png)

</ImageFrame>

キーワードカードエントリにはオブジェクトごとに以下の2つの値が含まれます。

* `text`: 表示されるキーワードテキスト。
* `type`: 常に`text`。

```json
[
    { "text": "Keyword1", "type": "text" },
    { "text": "Keyword2", "type": "text" },
    ...
]
```

#### トランスクリプトカードエントリ

<ImageFrame border center shadow width="200">

![画像](./skills-card-transcript.png)

</ImageFrame>

トランスクリプトカードエントリには以下の複数の値が含まれます。

* `text`: トランスクリプト行エントリに表示するテキスト。
* `appears`: 行エントリの開始時間と終了時間を含むオブジェクトの配列。
  * `start`: 開始時間(秒)。
  * `end`: 終了時間(秒)。

```json
[
    { "text": "Line1", "appears": [{ "start": 0, "end": 10 }] },
    { "text": "Line2", "appears": [{ "start": 11, "end": 20 }] },
    ...
]
```

#### フェイスカードエントリ

<ImageFrame border center shadow width="200">

![画像](./skills-card-faces.png)

</ImageFrame>

フェイスカードエントリには以下の複数の値が含まれます。

* `text`: 項目に表示するテキスト。
* `appears`: 行エントリの開始時間と終了時間を含むオブジェクトの配列。
  * `start`: 開始時間(秒)。
  * `end`: 終了時間(秒)。
* `image_url` (省略可): 項目の脇に表示する画像。

```json
[
    {
        "text": "Callout 1",
        "appears": [{ "start": 0, "end": 10 }],
        "image_url": "https://mysite.com/image1.jpg"
    },
    {
        "text": "Callout 2",
        "appears": [{ "start": 11, "end": 20 }],
        "image_url": "https://mysite.com/image2.jpg"
    },
    ...
]
```

## ファイルへのSkillsメタデータの書き込み

Box内のファイルにメタデータオブジェクトを再適用するには、書き込みトークンを使用して作成されたクライアントオブジェクトを使用して、ファイルにメタデータを作成するためのリクエストを作成します。

メタデータを作成するメソッドを呼び出すときに、メタデータテンプレートを`boxSkillsCards`に設定し、上の手順で作成したオブジェクトにメタデータを設定します。

<Samples id="post_files_id_metadata_id_id">

<Message type="notice">

すでにメタデータが適用されているファイルにメタデータを適用しようとすると、`409 Conflict: tuple_already_exists`エラーが発生します。ファイルにメタデータを作成するときには、すべてのエラーをキャッチする必要があります。このエラーが発生した場合、代わりにファイルに対して[メタデータの更新](endpoint://put_metadata_templates_id_id)をリクエストします。

</Message>
