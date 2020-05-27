---
rank: 1
related_endpoints: []
related_guides: []
required_guides: []
related_resources: []
alias_paths:
  - /docs/deep-linking
  - /docs/deep-linking-to-box-mobile-apps
category_id: best-practices
subcategory_id: null
is_index: false
id: best-practices/mobile-deep-linking
type: guide
total_steps: 3
sibling_id: best-practices
parent_id: best-practices
next_page_id: best-practices/branding-guidelines
previous_page_id: ''
source_url: >-
  https://github.com/box/developer.box.com/blob/master/content/guides/best-practices/mobile-deep-linking.md
---
# モバイルディープリンク

Boxのモバイルアプリでは、フォルダオブジェクトとファイルオブジェクトへのディープリンクがサポートされています。ウェブページまたはネイティブアプリからディープリンクを使用してBoxで直接オブジェクトを開くことができます。

Boxのモバイルアプリでは、以下のURLがサポートされています。

<!-- markdownlint-disable line-length -->

| アプリケーション        | オブジェクトタイプ   | ディープリンクのURL                            | iOSおよびAndroid |
| --------------- | ----------- | -------------------------------------- | ------------- |
| **Box**         | フォルダ        | `boxapp://folder?id=[folderid]`        | Version 3.7+  |
|                 | ファイル        | `boxapp://file?id=[fileid]`            |               |
|                 | Shared Link | `boxapp://sharedlink?url=[sharedlink]` |               |
|                 |             |                                        |               |
| **Box for EMM** | フォルダ        | `boxemm://folder?id=[folderid]`        | Version 3.7+  |
|                 | ファイル        | `boxemm://file?id=[fileid]`            |               |
|                 | Shared Link | `boxemm://sharedlink?url=[sharedlink]` |               |

<!-- markdownlint-enable line-length -->
