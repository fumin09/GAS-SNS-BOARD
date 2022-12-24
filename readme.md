# GoogleAppsScript(GAS)を利用した掲示板webアプリ

このアプリはGoogle Workspaceのサービスのみを利用して掲示板を利用することができます。

初期セットアップを行うには、Google Workspace、GAS、claspの知識が多少必要にある場合があります。

## 事前準備
アプリを使用するには以下のファイルとフォルダが必要です。  
名前は任意で良いです。  

### スプレッドシート
+ SNS_BORAD_DATA
  + 掲示板に投稿されたデータを保持するスプレッドシート

### フォルダ
+ SNS_FILE
  + 掲示板に添付されたファイルを保存するフォルダ

作成したスプレッドシート、フォルダの共有はURLを知っている人(編集可)にしてください。

### 【SNS_BORAD_DATA】スプレッドシートの設定

スプレッドシートシート名は「data」にしてください。
「data」シートの1行目に以下を追加してください

セル内：
|   | A  | B    | C          | D              | E              | F        | G        | H    | I         | J       | K      | L      |
|:-:|:---|:-----|:-----------|:---------------|:---------------|:---------|:---------|:-----|:----------|:--------|:-------|:-------|
| 1 | ID | 親ID | 削除フラグ | タイムスタンプ | 投稿アカウント | 投稿者アイコンURL | 投稿者名 | タイトル | 本文 | リンクURL | 画像URL | 閲覧者 |


## ソースコードをGASにアップロード
.clasp.jsonの「scriptId」を指定して、pushしてください。  
claspが不明な方は、「src」内になる全てのファイルをGASのスクリプトエディタ上で作成し、中身をコピーアンドペーストしてください。  
「appsscript.json」のみ左メニューの プリジェクトの設定 > 「appsscript.json」マニフェスト ファイルをエディタで表示する にチェックを入れるとエディタ上に表示されます。  

### スクリプトプロパティの設定
GASのスクリプトエディタの左メニュー プリジェクトの設定 > スクリプトプロパティの追加 から以下の値を追加してください。

| プロパティ  | 値  | 説明    |
|:-|:---|:-----|
| COMMENT_DISPLAY_COUNT | 10 | 掲示板のコメントを一度に表示する件数を設定します  最大10までが運用に耐えられるレベルになります |
| SNS_BOARD_SHEET_NAME | < SHEET_NAME > | 掲示板に投稿されたデータを保持するスプレッドシートのシート名を指定します |
| SNS_BOARD_URL | < URL > | 掲示板に投稿されたデータを保持するスプレッドシートのURLを指定します |
| SNS_DISPLAY_COUNT | 50 | 掲示板の投稿を一度に表示する件数を設定します  最大100までが運用に耐えられるレベルになります |
| UPLOAD_FOLDER_URL | < URL > | 掲示板に投稿された画像を保存するフォルダーのURLを指定します |


### データ整理トリガーの設定
スプレッドシートのセルは限界があります。  
なので、トリガーを設定し削除フラグがついているデータをデータとして削除します。  

設定方法  
トリガー > トリガーの追加 
* 実行する関数を選択：organizeSnsData
* 実行するデプロイを選択：Head
* イベントソースを選択：時間手動型
* 時間ベースのトリガーのタイプを選択：月ベースのタイマー
* 日を選択：1日
* 時刻を選択：午前3時〜4時
※「時間ベースのトリガーのタイプを選択」は自身の更新頻度に合わせてください

保存をクリック。

事前準備は完了です。
