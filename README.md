# scp_crawler

Scrapy で構築した Web クローラーです。SCP-JP と SCP International Hub からデータを収集します。

## インストール

```
make install
```

## 全体クロール

すべてのスパイダーを実行し、SCP-JP と SCP International Hub のデータを data ディレクトリに出力します。

```bash
make crawl
```

## scrapy CLI で個別実行

scrapy コマンドを使って、スパイダーを個別に実行できます。

利用可能なスパイダー一覧を表示:

```bash
scrapy list
```

SCP International Hub のアイテムをクロールし、任意のファイル名で保存:

```bash
scrapy crawl scp_int -o scp_international_items.json
```

## 生データの構造

取得するコンテンツは 2 種類です。

- SCPアイテム
- SCP短編

すべてのコンテンツ（アイテム / 短編）には次の情報が含まれます。

* URL
* タイトル
* レーティング
* タグ
* 履歴（リビジョン ID、日時、作者、コメント）
* 本文コンテンツ（サイトナビゲーション等を除いた本文 HTML）

SCPアイテムには以下も含まれます。

* SCP 識別子（例: SCP-3000）
* SCP 番号（取得できる場合）
* SCPシリーズ
  * 1-5（将来のシリーズ拡張にも対応）
  * joke, explained, decommissioned
  * Generic International（メインサイト由来）
  * 各国タグ（International Hub 由来）

### 生成ファイル

クロール結果は、オブジェクト配列を含む複数の JSON ファイルとして出力されます。

| ファイル名          | ソース         | 種別   | ターゲット |
| ------------------- | ------------- | ------ | ---------- |
| goi.json            | Main          | Tale   | goi        |
| scp_items.json      | Main          | Item   | scp        |
| scp_titles.json     | Main          | Title  | scp        |
| scp_hubs.json       | Main          | Hub    | scp        |
| scp_tales.json      | Main          | Tale   | scp        |
| scp_int.json        | International | Item   | scp_int    |
| scp_int_titles.json | International | Title  | scp_int    |
| scp_int_tales.json  | International | Tale   | scp_int    |

make TARGET（例: make goi / make scp）を実行すると、対象サイトのファイルを生成します。make data を実行すると不足ファイルを補完します。

すべて再生成する場合は make fresh を実行します。

## 後処理データ

postproc システムは Titles / Hubs / Items / Tales を使って、より包括的なデータセットを生成します。既存データの結合・相互参照・拡張を行います。


## ライセンス

SCP Wiki 上のテキストコンテンツは CC BY-SA 3.0 ライセンスで公開されています。

このプロジェクトは画像ファイルをダウンロードしません。
