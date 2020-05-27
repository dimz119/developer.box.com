---
rank: 3
related_endpoints: []
related_guides:
  - applications/custom-apps/oauth2-setup
  - authentication/access-tokens
required_guides: []
related_resources: []
alias_paths: []
notes: Needs a massive cleanup
category_id: applications
subcategory_id: applications/web-app-integrations
is_index: false
id: applications/web-app-integrations/configure
type: guide
total_steps: 3
sibling_id: applications/web-app-integrations
parent_id: applications/web-app-integrations
next_page_id: applications/web-app-integrations
previous_page_id: applications/web-app-integrations/user-experience
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/applications/web-app-integrations/configure.md
---
# ウェブアプリ統合の作成

次のガイドでは、カスタムアプリのウェブアプリ統合を設定する方法について説明します。

## 前提条件

開始する前に、以下の要件を満たしている必要があります。

* 会社の[開発者コンソール][devconsole]にアクセスできる必要があります。アクセスできない場合は、[Developerアカウント][devaccount]にサインアップしてください。
* 開発者コンソールで[OAuth 2.0認証][custom-oauth2]を使用してカスタムアプリを作成済みである必要があります。

## 1. 新しい統合を作成する

開発者コンソールにログインし、アプリケーションを探し、左側のサイドバーで\[統合]パネルを見つけます。\[ウェブアプリ統合を作成]をクリックします。

## 2. 統合を構成する

好きなように統合を構成します。以下に、各値のガイドを示します。

### アプリ情報

<!-- markdownlint-disable line-length -->

| フィールド            | 説明                                                   |
| ---------------- | ---------------------------------------------------- |
| 統合名              | 統合の名前。ユーザーがファイルを選択すると、ウェブアプリのコンテキストメニューにこの名前が表示されます。 |
| 説明               | Boxアプリギャラリーに表示される統合の説明。                              |
| サポートされているファイル拡張子 | この統合が有効になるファイルの種類。この統合は、この拡張子が付いたファイルにのみ表示されます。      |

<!-- markdownlint-enable line-length -->

### コールバック設定

<!-- markdownlint-disable line-length -->

| フィールド            | 説明                                                                                                                                                                                               |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| ユーザーエクスペリエンス     | 統合がサーバー側統合であるか、ポップアップ統合であるか。                                                                                                                                                                     |
| 事前コールバックのURL     | ユーザーがプロンプトを受け入れたときにコールバックパラメータの送信先となるURL。これは、アプリケーションサーバーでAPI呼び出しを実行するURLであることがほとんどですが、HTTPリクエストを受け入れるように構成されたエンドポイントにすることもできます。                                                                 |
| クライアントコールバックのURL | ポップアップ統合で、最初のリクエストの後にBoxからの追加のコールバックリクエストを処理するコールバックURL。アプリケーションがRESTメソッドでファイルパラメータを指定した場合、事前コールバックのURLはクライアントから発信できません。その結果、サーバーが必要なインターフェイスをユーザーに送信できるように、2番目のリクエストがクライアントからサーバーに送信される必要があります。 |

<!-- markdownlint-enable line-length -->

### 統合ステータス

この統合のステータス。

* **開発**: 統合は、アプリケーションに割り当てられた開発者のみが表示し、使用できます。このオプションは、アプリケーションがまだ開発中で、開発者が統合をテストする場合に最もよく使用されます。
* **オンライン**: 統合は、すべてのBoxユーザーが表示し、使用できます。このオプションは、開発が完了し、アプリケーションをアプリギャラリーで公開する準備ができている場合に最もよく使用されます。
* **メンテナンス**: 統合は、アプリケーションに割り当てられた開発者のみが表示し、使用できます。このオプションは、アプリケーションが一般にリリースされ、メンテナンスでの更新の実行または問題のトラブルシューティングが必要な場合に最もよく使用されます。このオプションを使用すると、統合の開発者を除くすべてのユーザーに対して統合が一時的にオフラインになります。

### コールバックパラメータ

\[コールバックパラメータ]セクションでは、ユーザーが確認プロンプトを受け入れるとBoxからコールバックURLに送信されるパラメータを構成します。構成しないと、BoxからコールバックURLにパラメータが送信されません。

以下のパラメータが使用可能です。

<!-- markdownlint-disable line-length -->

| パラメータ                 | 説明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `user_id`             | The Box ID of the user. This information is used in popup integrations in which user authentication is required to complete an action. You can store the Box ID in your application to enable your application to authenticate subsequent requests from the integration.                                                                                                                                                                                                                                                                                                                                                          |
| `user_name`           | The full name or email address of the Box user. Not all Box users specify their names at all times.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `file_id`             | The Box ID of the file. You can use this ID to make Box API calls that affect the file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `file_name`           | The name of the file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `file_extension`      | The extension of the file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `auth_code`           | The OAuth 2 authorization code for the file, supplied by Box when an authentication succeeds. Your application must supply this authorization code to Box in exchange for an OAuth 2.0 access token and OAuth 2.0 refresh token in order to make API calls. An authorization header containing a valid access token must be included in every Box API request.                                                                                                                                                                                                                                                                    |
| `redirect_to_box_url` | In popup integrations, the URL to which requests are sent by the confirmation prompt. This parameter is applicable only to Popup integrations. Use this URL to redirect users to the All Files page. This parameter closes the popup panel and refreshes the All Files page to reflect any changes performed by the integration. If you do not want to add this parameter to your application, you can specify the entire URL. **Success**: `#redirect_to_box_url#&status=success&message=Your%20action%20was%20successful%2E`. **Failure**: `#redirect_to_box_url#&status=failure&message=Your%20action%20was%20unsuccessful%2E` |

<!-- markdownlint-enable line-length -->

## Box統合の使用例

ユーザーがポップアップ統合を選択すると、Boxから事前コールバックのURLにコールバックリクエストが送信されます。これにより、構成済みのコールバックパラメータがサーバーに送信されます。クライアントが必要なデータを最初のリクエストからすべて取得できない場合は、Boxが2番目のリクエストを送信することもあります。

次の例では、クライアントコールバックのURLが必要ありません。

* ポップアップ統合で、`download_file_url`コールバックパラメータを使用してREST呼び出しを実行する。
* ユーザーが確認プロンプトで\[OK]をクリックしてポップアップを受け入れる。
* Boxが次のURLにリクエストを送信する(事前コールバックのURLにコールバックパラメータを追加): `http://www.doceditor.com/service?apikey=abc&file=&redirect=`。
* コールバックURLからの応答により、リクエストを送信したユーザーにユーザーインターフェイスが表示される。ポップアップには、アクションを続行するために必要なすべての情報が表示されているため、追加のクライアントコールバックは必要ありません。

次の例では、クライアントコールバックのURLが必要です。

* ポップアップ統合で、ファイルコールバックパラメータを使用してREST呼び出しを実行する。
* ユーザーが確認プロンプトで\[OK]をクリックしてポップアップを受け入れる。
* ポップアップによって表示されたページで、Boxからリモートサーバーに、ファイルのコンテンツを含むPOSTリクエストとともにコールバックパラメータが送信される。
* Boxがリモートサーバーから応答を受信し、クライアントにクライアントコールバックのURLへの応答を投稿するよう指示する。このURLで識別されたサーバーが応答を解釈し、適切なセッションIDを持つユーザーをリダイレクトします。

## クライアントコールバックのURLのリクエスト形式

BoxからクライアントコールバックのURLに送信されるPOSTリクエストは、事前コールバックのURLから応答を取得し、元のコールバックと同じデータとともに応答を同じURLに転送します。

<!-- markdownlint-disable line-length -->

| クライアントコールバックのURL                                                                                     | 例                                                        |
| ---------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| 2つのGETパラメータと1つのPOSTパラメータ: `http://your-client-callback-url.com/?get_param1=value1&get_param2=value2` | `POST data: post_param1=value1initial_callback_response` |

<!-- markdownlint-enable line-length -->

The response to the client-callback request is an HTTP status 302, redirecting
the user to the correct URL or to the HTML for a user interface.

ほとんどの場合、URLはウェブアプリ統合のために開発された個々のAPIまたはカスタムスクリプトを指します。これは、事前コールバックのURLの結果を解析します。また、このURLは、インターネット上で一般公開する必要があることに注意してください。

## 統合の一般公開

Box統合を一般公開するには、統合をアプリギャラリーに掲載する必要があります。詳細については、[アプリギャラリー][app-gallery]ガイドに従ってください。

[custom-oauth2]: g://applications/custom-apps/oauth2-setup

[devconsole]: https://app.box.com/developers/console

[devaccount]: https://account.box.com/signup/n/developer

[app-gallery]: g://applications/app-gallery
