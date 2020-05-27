---
rank: 2
related_endpoints: []
related_guides: []
required_guides: []
related_resources: []
alias_paths:
  - /docs/box-embed
category_id: embed
subcategory_id: null
is_index: false
id: embed/box-embed
type: guide
total_steps: 2
sibling_id: embed
parent_id: embed
next_page_id: embed
previous_page_id: embed/box-dicom
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/embed/box-embed.md
---
# Box Embed

Box EmbedはHTMLベースのフレームワークで、Boxの機能全体を埋め込み、場所を問わずに使えるようにします。Box Embedを使用すると、ファイルのアップロード、検索、コメント付け、共有、タグ付けに加え、最も重要な操作としてBox Editを使用したファイルの編集も可能になります。

## 構成

### ウェブから構成

BoxウェブアプリからBox Embedのコードを取得するには、目的のフォルダに移動し、フォルダの横にある省略記号をクリックして、\[その他の操作]に移動し、\[埋め込みウィジェット]をクリックします。

<ImageFrame border>

![Box Embed](./box-embed.png)

</ImageFrame>

サイズ、表示方法、および並べ替えを調整するためのオプションが表示されます。

<ImageFrame border>

![Box Embedの構成](./box-embed-2.png)

</ImageFrame>

埋め込みウィジェットのカスタマイズが完了したら、あとは埋め込みコードをコピーしてサイトまたはウェブアプリケーションに貼り付けるだけです。

## プログラムを使用して構成

Box Embedをさらにカスタマイズする場合は、プログラムを使用してカスタマイズできます。埋め込みのスニペットの形式は次のとおりです。

<!-- markdownlint-disable line-length -->

```html
<iframe
  src="https://{custom_domain}.app.box.com/embed/s/{shared link value}?view={list or icon}&sortColumn={name, date, or size}&sortDirection=ASC"
  width="{pixels}"
  height="{pixels}"
  frameborder="0"
  allowfullscreen
  webkitallowfullscreen
  msallowfullscreen
></iframe>
```

<!-- markdownlint-enable line-length -->

### 共有リンクの値の検索

プログラムを使用して埋め込み`iframe`を構築するには、まず、共有リンクの値を生成または検索します。この値を検索する1つの方法として、Boxウェブアプリを使用します。

<ImageFrame border>

![Boxの共有](./box-share.png)

</ImageFrame>

また、[`GET /files/:id`](e://get-files-id)または[`GET /folders/:id`](e://get-folders-id)エンドポイントを使用してクエリパラメータ`fields=shared_link`を渡すことにより、APIを介してこの共有リンクの値を検索することもできます。

```curl
curl https://api.box.com/2.0/folders/12345?fields=shared_link \
    -H "authorization: Bearer ACCESS_TOKEN"
```

```json
"shared_link": {
  "url": "https://cloud.box.com/s/bxtkjxgiq6v50zfap4h1xez5qthn186u",
  "download_url": null,
  "vanity_url": null,
  ...
}
```

### パラメータ

次に、表示のカスタマイズオプションを選択します。構成可能なパラメータ(省略可)のリストを以下に示します。

<!-- markdownlint-disable line-length -->

|                       |                                                                                              |
| --------------------- | -------------------------------------------------------------------------------------------- |
| `view`                | The view type for your files or folders. Can be `list` (default) or `icon`.                  |
| `sortColumn`          | The order the files or folders are sorted in. Can be `name`, `date` (default), or `size`.    |
| `sortDirection`       | The sort direction of files or folders. Can be `ASC` (default) or `DESC`.                    |
| `showParentPath`      | Hide or show the folder path in the header of the frame. Can be `true` or `false` (default). |
| `showItemFeedActions` | Hide or show file comments or tasks. Can be true (default) or false.                         |

<!-- markdownlint-enable line-length -->

### 全画面表示機能

Box Embedスニペットの全画面表示機能を有効にするために、オブジェクトを全画面に表示可能にする場合は、以下のパラメータの1つ以上を`<iframe>`に含めてください。

* `allowfullscreen`
* `webkitallowfullscreen`
* `mozallowfullscreen`
* `oallowfullscreen`
* `msallowfullscreen`

## Expiring Embed Links

ファイルの場合、[`GET /files/:id`](e://get-files-id)を呼び出し、`fields`クエリパラメータを使用して`expiring_embed_link`をリクエストすることもできます。

```curl
curl https://api.box.com/2.0/files/12345?fields=expiring_embed_link \
  -H "authorization: Bearer ACCESS_TOKEN"
```

```json
{
  "etag": "1",
  "expiring_embed_link": {
    "token": {
      "access_token": "1!rFppcinUwwwDmB4G60nah7z...",
      "expires_in": 3646,
      "restricted_to": [
        {
          "object": {
            "etag": "1",
            "file_version": {
              "id": "34567",
              "sha1": "1b8cda4e52cb7b58b354d8da0068908ecfa4bd00",
              "type": "file_version"
            },
            "id": "12345",
            "name": "Image.png",
            "sequence_id": "1",
            "sha1": "1b8cda4e52cb7b58b354d8da0068908ecfa4bd00",
            "type": "file"
          },
          "scope": "base_preview"
        },
       ...
      ],
      "token_type": "bearer"
    },
    "url": "https://cloud.app.box.com/preview/expiring_embed/...."
  },
  "id": "12345",
  "type": "file"
}
```

`url`属性を`<iframe>`内で使用すると、自動で期限切れになるBox Embedインターフェイスを埋め込むことができます。

```html
<iframe
  src="<YOUR-GENERATED-BOX-EMBED-LINK"
  width="{pixels}"
  height="{pixels}"
  frameborder="0"
  allowfullscreen
  webkitallowfullscreen
  msallowfullscreen
/>
```

### パラメータ

UIをカスタマイズするために、このURLにさらにパラメータを追加することもできます。そのためには、以下のパラメータをクエリパラメータとして`url`に追加します。最終的なURLは、次のようになります。

```sh
https://app.box.com/preview/expiring_embed/[HASH]?[parameterName]=true
```

<!-- markdownlint-disable line-length -->

|                   |                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `showDownload`    | Shows the download button in the embedded header bar if the viewer has permissions to download the file. Document file types will also show a print button since print and download are governed by the same permissions. Defaults to `false`.                                                                                                                                                                                              |
| `showAnnotations` | Enables users with permission Preview and above to annotate document and image previews. Also shows annotations that are already on the document. To learn more about the file types that annotations is available on as well as the types of annotations, you can refer to our Annotations page. Annotations are available today on web browsers only. On mobile browsers, users will be able to view annotations but not create new ones. |

<!-- markdownlint-enable line-length -->

## カスタムロゴ

有料のBoxをお使いの場合は、ファイルのプレビューに表示されるBoxのロゴを削除できます。削除するには、**管理コンソール**の\[**Enterprise設定**]、\[**カスタム設定**]に移動し、\[**埋め込みウィジェットのカスタマイズ**]をオフに切り替えてBoxのロゴを非表示にします。

## 制限

Box Embedは、モバイルブラウザ向けには最適化されていないため、モバイルデバイス用に設計されたウェブエクスペリエンスでは使用しないでください。多くのUI Element(**ダウンロード**オプションや**印刷**オプションなど)はモバイルブラウザに表示されない可能性があります。

[logo]: https://community.box.com/t5/Get-Started-Guide-for-New-Admins/Customize-Your-Account-s-Branding/ta-p/301
