---
rank: 1
related_endpoints: []
related_guides:
  - skills/
  - skills/handle/payload
related_pages: []
required_guides: []
related_resources: []
alias_paths: []
category_id: skills
subcategory_id: null
is_index: false
id: skills/invocation-url
type: guide
total_steps: 3
sibling_id: skills
parent_id: skills
next_page_id: skills/examples
previous_page_id: ''
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/skills/invocation-url.md
---
# Box Skills呼び出しURL

新しい[Box Skillsアプリケーション](guide://applications/custom-skills)の作成時に、呼び出しURLを指定するよう求められます。これは、Skillsアプリが監視するフォルダ内にファイルがアップロード、コピー、移動されたときに、Boxから送信される[イベント通知ペイロード](guide://skills/handle/payload)の送信先です。

この通知をリッスンしているWebサイトまたはアプリケーションは、Box上のファイルと、ファイルからインサイトを取得するために使用されている機械学習システムなどのシステムの間のブリッジとして機能します。

## 呼び出しURLの要件

呼び出しURLは以下の要件を満たす必要があります。

* パブリックに使用できること。`localhost`には通知を送信できません。
* URLに直接`HTTP POST`リクエストをリッスンするコードが含まれていること。Box Skillsは、HTTP POSTリクエストを介してイベント通知を送信します。

## 呼び出しURLの背後での一般的なホスティング方法

呼び出しURLの背後にあるアプリケーションに使用される一般的なホスティング方法はいくつかあります。

通常、このミドルウェアサービスは以下の処理を行う必要があります。

* Boxからのイベント通知をキャプチャする。
* Boxからファイルをダウンロードし、処理システムに送信する。
* 機械学習システムからの応答をリッスンする。
* 機械学習システムからの応答を、Boxメタデータ形式に変換する。
* Boxに保存されているファイルに新しいメタデータを適用する。

このようなサービスは一般的に、会社のセキュリティ要件と承認されているサービスに応じて、次の方法でホストされます。

<!-- markdownlint-disable line-length -->

| 方法            | 説明                                                                                                                                                                                                                                                                  |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| セルフホスティング     | 会社内での外部ホスティングサービスの使用が承認されていない場合、またはすべてのアプリケーション機能を自社サーバー内でホストする必要がある場合にこの方式を使用します。このホスティング方法では、自社サーバーでホストされているコードにリンクするURLを公開します。                                                                                                                                   |
| サーバーレス関数      | イベント通知には、Skillsを介して散発的に送信されるという特徴があるため、[AWS Lambda][aws_lambda]、[Google Cloud Functions][google_functions]、[Microsoft Azure Functions][azure_functions]などのサーバーレス関数が適しています。このようなサーバーレス関数はイベントの処理中にのみ実行され、課金の対象となります。サーバーレス関数には公開URLがセットアップされ、このURLが呼び出しURLとして使用されます。 |
| 外部アプリケーションホスト | サーバーレステクノロジが望ましくない場合、[Heroku][heroku]、[Firebase][firebase]などの外部アプリケーションホスティングソリューションも使用できます。このようなアプリケーションはそれぞれ固有のサービスでホストされ、アプリケーション用の公開URLが呼び出しURLとして使用されます。                                                                                                       |

<!-- markdownlint-enable line-length -->

[aws_lambda]: https://aws.amazon.com/lambda/

[google_functions]: https://cloud.google.com/functions/

[azure_functions]: https://azure.microsoft.com/en-us/services/functions/

[heroku]: https://www.heroku.com/

[firebase]: https://firebase.google.com/
