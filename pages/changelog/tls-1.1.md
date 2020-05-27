---
alias_paths:
  - /docs/tls-1
  - /docs/tls-11-deprecation
centered: true
rank: 2
category_id: changelog
subcategory_id: null
is_index: false
id: changelog/tls-1.1
type: page
total_steps: 3
sibling_id: changelog
parent_id: changelog
next_page_id: ''
previous_page_id: changelog/2018
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/pages/changelog/tls-1.1.md
---
# TLS 1.1のサポート終了に伴う移行

## 概要

APIとBox間の通信を安全に保つために、2020年3月31日以降は、Transport Layer Security (TLS) 1.1暗号化プロトコルに依存する製品とサービスのサポートを停止します。

## 一般的なテストの手順

TLS 1.1のサポート停止後もアプリケーションが機能することを確認できるように、新しいベースURL [`https://api-test.box.com/2.0/`][tls_test_url]が用意されました。これを使用して任意のBox APIエンドポイントをテストすることで、アプリケーションがTLS 1.2以上をサポートしているかどうかを確認できます。このテストエンドポイントは、TLS 1.1で作成されたリクエストを拒否します。

テストエンドポイントに対してAPI呼び出しを実行して接続拒否エラーが返された場合は、TLS 1.2以上をサポートするよう環境をアップグレードする必要があります。テストエンドポイントに対してAPI呼び出しを実行し、成功の応答が返された場合、その環境でTLS 1.2以上がサポートされているため、対策は不要です。

<Message type="warning">

このテストエンドポイントを使用したテストに対応しないエンドポイントは、アップロードエンドポイントのみです。

</Message>

## 言語別のテストおよびアップグレードの手順

### .NET

#### テスト(.Net)

.NET SDK 3.8.0以上

```csharp
BoxConfig config = new BoxConfig(/* config parameters */);
config.BoxApiUri = new System.Uri("https://api-test.box.com/2.0/");
```

#### アップグレード(.Net)

* .NET Framework 4.6にアップグレードします。または、.NET Framework 4.5にアップグレードし、アプリケーションに以下の行を加えます。

```csharp
//Make sure this line is globally scoped in your application.
ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12
```

* Box .NETまたは.NET Core SDKを使用している場合は、バージョン3.5.2以上にアップグレードします。

### Java

#### テスト(Java)

```java
BoxAPIConnection api = new BoxAPIConnection(/* connection parameters */);
api.setBaseURL("https://api-test.box.com/2.0/");
```

#### アップグレード(Java)

* Java Runtime Environment 1.8以上にアップグレードします。
* Box Java SDK 2.14.1以上にアップグレードします。

### Node

Nodeの公式ビルドを使用している場合、影響はありません。

#### テスト(Node)

```js
var BoxSDK = require('box-node-sdk');
var sdk = new BoxSDK({
  /* Other configuration parameters */
  apiRootURL: 'https://api-test.box.com'
});
```

### Ruby

#### アップグレード(Ruby)

Ruby 2.0.0以上およびOpenSSL 1.0.1以上にアップグレードします。

### Python

#### アップグレード(Python)

* Python 2.7.9以上を使用している場合、デフォルトでTLS 1.2以上と互換性があります。
* Python 2.7.8以下を使用している場合、Python 2.7.9以上に更新する必要があります。

### Android

#### アップグレード(Android)

Androidフレームワークのバージョン5.0以上(APIレベル20以上)にアップグレードします。

[tls_test_url]: https://api-test.box.com/2.0/
