# ISBNからNotionのデータベースに書籍登録 by iPhoneショートカット

yokoyです。
手元にある書籍のバーコードを読み取ってnotionデータベースに登録するショートカットを作成したので
手順を公開します。

https://youtube.com/watch?v=SvSS2BCtgj8?feature=share

こんな人におすすめです

- **Notionで書籍管理を行いたい**
- **Notionで書籍管理を行なっているけど、、**
    - **表紙の画像を探してダウンロードするのがめんどくさい**
    - **タイトルとか著者名のコピペがめんどくさい**

## 有料部分について
本記事には有料パートがありますが、無料パートだけでショートカットを作成可能です。
無料部分で作成手順を解説しており、有料部分には作成済のショートカットのリンクを載せています。
「作るのはめんどくさい」「とにかく早く使いたい」という人は有料版の購入をお勧めいたします。

## 目次

## 記事を書いた経緯

類似の記事はいくつか存在しますが、今回私が本記事を書いた理由は大きく2つです。

### 書誌検索APIとしてrakutenBooksAPIを使用している記事がない

既存の類似記事の多くがopenDBを使用していますが、
openDBは既に提供が終了しており、利用ができないようです。
https://openbd.jp/news/20230725.html

### ショートカットリンクを共有している記事がない

作り方の手順を詳細に解説した記事は存在しますが、ショートカットリンクをそのまま公開しているものは見つけられませんでした。
iPhoneのショートカットを1から作成するのは、忙しい人や触ったことのない人にとってはかなりハードルが高いかと思います。
本記事では有料部分で作成済のショートカットの共有リンクを公開しています。
ユーザーが行う作業はいくつか残ります(APIの登録やnotion上でのデータベースIDの取得等）が、ショートカットを1から作成する必要が無くなる点は有益です。

## 想定読者

- **Notionのアカウントを持っている**
- **iPhoneユーザである**
- **Notionで書籍管理を行いたい**
    - **表紙の画像を探してダウンロードするのがめんどくさい**
    - **タイトルとか著者名のコピペがめんどくさい**

## 事前準備

ショートカットの作成に入る前に4つの事前準備が必要です
（有料版を購入される場合も、この準備は必要になります）

* **ショートカットアプリのインストール**
* **データベースの作成とIDの取得**
* **notionインテグレーションキーの取得**
* **楽天ブックスAPIの登録**

以下、それぞれについて詳細を説明します

### ショートカットアプリのインストール

https://apps.apple.com/jp/app/ショートカット/id915249334

ほとんどの人はすでにインストールされていると思いますが、
見当たらない方は上のリンクからインストールしてください

### データベースの作成

以下のようなデータベースを用意してください

[images/notionDB.png]

列はいくつあっても良いですが、以下の4つを必ず含むようにしてください
※（）内はプロパティ名です

* タイトル
* 著者（テキスト）
* 出版社（テキスト）
* 表紙（ファイル＆メディア）

列名は上の通りにしてください
列の順番は適当で問題ないです
必ずインラインデータベースではなくフルページデータベースにしてください

### データベースIDの取得

作成したデータベースのリンクを取得してください
ブラウザでデータベースページを開くか、
notionでデータベースを開いて右上の3点リーダから「リンクをコピー」してどこかにペーストします

```
https://www.notion.so/XXXXXXXXXX?v=.....
```

上記のso/と?v=で挟まれた32桁の英数字がデータベースIDになります

### Notionインテグレーションキーの取得

下記リンクからnotionアカウントにログインし、インテグレーションの追加を行なってください
https://www.notion.so/profile/integrations

[images/integretion1]

* 「新しいインテグレーション」をクリック

[images/integretion2]

* 「インテグレーション名」は何でもいいです
* 「関連ワークスペース」はデータベースがあるワークスペースを指定
* 「種類」は内部のまま
* 「ロゴ」はなくてよい
* 「保存」をクリック

[images/integretion3]

その後表示される「内部インテグレーションシークレットキー」を表示します
"ntn_"から始まる文字列が今回追加したインテグレーションのキーになります

### rakutenbooksapiの登録

次にAPIに登録しアプリケーションIDを発行します
以下のURLからアプリID発行に進んでください
※楽天のアカウントを持っていない方は事前にアカウント登録をした上で以下に進んでください

https://webservice.rakuten.co.jp

[images/API1]

* 「アプリID発行」をクリック

[images/API2]

* 「アプリ名」は適当でOK
* 「アプリURL」も適当でOK
* 認証キーを入力し、「規約に同意して新規アプリを作成」

その後表示されるアプリID/デベロッパーIDをショートカットで使用します

## ショートカットの作成

ではここからショートカットを作成する手順を説明していきます
スクショがメインですが、適宜説明も入れています
スクショ下の箇条書きは使用しているアクション名です

### 各種変数の設定

[images/shortcut_set_hensu1.png]

* テキスト
* 変数を設定

3回繰り返します

### ISBNの読み取り

[images/shortcut_get_isbn1.png]

* QRまたはバーコードをスキャン
* URL

macだとカメラを起動するアクションは追加できないので、
これだけiPhoneから追加しました

### APIを叩いて結果を取得

[images/shortcut_get_APIresponse1.png]

* URLの内容を取得

例えば、ISBN=9784088900827の本を検索したときの結果はこんな感じ
（長すぎるので一部抜粋しています）

```
{
  "GenreInformation": [],
  "Items": [
    {
      "Item": {
        "publisherName": "集英社",
        "seriesName": "ヤングジャンプコミックス",
        "title": "ゴールデンカムイ 1",
        "itemUrl": "https://books.rakuten.co.jp/rb/13062647/?rafcid=wsc_b_bs_1018165179551203384",
        "size": "コミック",
        "largeImageUrl": "https://thumbnail.image.rakuten.co.jp/@0_mall/book/cabinet/0827/9784088900827.jpg?_ex=200x200",
        "isbn": "9784088900827",
        "itemPrice": 594,
        "salesDate": "2015年01月19日頃",
        "author": "野田サトル",
      }
    }
  ],
  "count": 1,
  "first": 1
}
```

### （おまけ）検索結果が無い場合の処理

[images/shortcut_if_nobook1.png]

* 辞書の値を取得
* 入力から数字を取得
* if文
    * アラートを表示
    * このショートカットを停止

### 検索結果から必要な情報を取得

先ほど取得したJSONからタイトルや著者名などの情報を変数に格納します

[images/shortcut_get_APIresponse2.png]

* 辞書の値を取得
* リストから項目を取得
* 辞書の値を取得
* 変数を設定
* 辞書の値を取得
* 変数を設定（以下、繰り返し）

### Notionデータベースに登録

[images/shortcut_add_notion1.png]

* テキスト
* 変数を設定

[images/shortcut_add_notion2.png]

* URLの内容を取得
* アラートを表示

おつかれさまでした！
ショートカットの設定は以上になります

## 動作チェック

実際にショートカットを実行してみます。
実行するだけで自動でカメラが起動します。

> 本の裏にバーコードが2つある場合は、下側を指で隠して撮影してください  
下のバーコードが読み取られた場合は検索にヒットしません

初めてショートカットを実行した場合は、APIの使用許可を聞かれるポップアップが出現することがありますが、「許可」をタップしてください。

## ショートカットリンク

これまでのショートカット作成手順を再現するのがめんどくさいという人は、
ここからの有料版を購入してください。

- **トラブル回避のため、”事前準備”をすべて完了した上でご購入をお願い致します。**
- **本ショートカットはiOS18.3で動作確認を実施しています。**
- **これより古いver.では正常に動作しない可能性があることをご理解の上、ご購入をお願い致します。**
- **ショートカットをダウンロードしたのち、事前準備で取得したID類をショートカット内に入力する作業は必要となりますので、ご理解のほどよろしくお願い致します。**

では、以下がショートカットリンクになります。

https://www.icloud.com/shortcuts/9e4b300b17624513806420a94a481b4e

[images/add_shortcut.png]

上記リンクを開き、
「ショートカットを追加」をクリックしてください

### 事前準備で取得したid類の登録

ショートカットを開き、idの部分をあなたのidに書き換えます。
まずはショートカットアプリを開いてください。

[images/added_shortcut.png]

すべてのショートカットの一番上に【共有】ISBN読取→Notionというショートカットができているはずです。
ショートカット右上の3点リーダをタップしてください。

[images/shortcut_set_hensu1.png]

XXXXXXを削除し、それぞれ適切なid, キーをコピペしてください。
ここが間違っていると正常に動作しないので間違えないよう注意してください。
3箇所すべて書き換えられたら、「完了」を押して終了です。

> ちなみに左上のアイコンやショートカット名をタップすることで自由にアイコンや名前を変更できます

### ショートカットの実行

では実際にショートカットを実行します。
やり方は簡単です、ショートカットアプリから【共有】ISBN読取→Notionのショートカットをタップするだけ！
自動でカメラが起動するのでお手元の本の裏のバーコードを読み取ってください

> 本の裏にバーコードが2つある場合は、下側を指で隠して撮影してください  
下のバーコードが読み取られた場合は検索にヒットしません

[images/realbook.png]


初めてショートカットを実行した場合は、APIの使用許可を聞かれるポップアップが出現することがありますが、「許可」をタップしてください。

検索がうまくいけば、「"本のタイトル"をnotionに追加しました！」と表示が出ます。
notionに移動し、データベースを開いてみましょう。
無事、バーコード読取した書籍が追加されていれば成功です。

> 「をnotionに追加しました！」と表示される場合はidの入力がうまくいってない可能性が高いです。
もう一度、applicationidを見直してみてください。
「本が見つかりませんでした」と表示される場合は、楽天ブックスAPI内に無い書籍を読み込んでいます。
別の本でもう一度試してみてください。

**ショートカットが動かない場合のチェックポイント**

- APIキーやデータベースIDなどが間違っていないか？
- iOSのバージョンが最新か？
- iCloudが有効になっているか（「設定」>「Apple ID」>「iCloud」>「ショートカット」がオンになっていること）
- 以上を全て試しsafariでiCloudリンクを開き直す

上記を試しても正常に動作しない場合は、お手数ですが以下のリンクを参考にクリエイターページから問い合わせをお願い致します。

[https://note.com/info/n/n033f422de5bb]

このショートカットがみなさまのお役に立てることを祈っております。
