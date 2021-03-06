# 1. 脱水管理アプリケーション


# 2. 脱水管理アプリ開発計画
日付，天気，湿度，トレーニング，時間，運動前体重，運動後体重，飲水量をデータベースに保存し日々の脱水を管理する．
## 2.1. 機能
### 一般ユーザー向け
- 天気，湿度，トレーニング，時間，運動前体重，運動後体重，飲水量入力しデータベースに保存
- 結果を表示
- 掲示板機能
- 今日の入力結果によってコメントを表示

### 管理ユーザー向け
- 結果を表示
- 新規ユーザー登録
- 簡単な分析機能
- 掲示板機能
- 結果をCSV形式でダウンロード

## 2.2. 設計
ユーザー用，管理者用の画面はhtml,CSS,javascriptで作成し，アプリケーションサーバはPytholnのFlaskで作成する．データはMySQLで管理する．

## 2.3. データベース構成

### トレーニング結果
|ID|password|組織|登録年度|日付|天気|湿度|トレーニング|時間|運動前体重|運動後体重|飲水量|
|---|---|---|---|---|---|---|---|---|---|---|---|
|azm|????|静大|2019|2019-08-02|晴れ|50|マラソン|2|70|68.9|0|
|dai|????|静大|2019|2019-08-02|雨|40|twitter|2|60|59.8|0.1|
|tomo|????|静大|2019|2019-08-02|曇り|30|ゲーム|2|55|54.9|0.3|
|azm|????|静大|2019|2019-08-03|晴れ|50|マラソン|2|70|68.9|0|
|dai|????|静大|2019|2019-08-03|雨|40|twitter|2|60|59.8|0.1|
|tomo|????|静大|2019|2019-08-03|曇り|30|ゲーム|2|55|54.9|0.3|

### コメント内容
|送信者|受信者|内容|
|---|---|---|
|azm|ALL|天才とは努力する凡才のことである．|
|azm|ALL|今日は暑いですね．|

※ALLは全員へ

### 正規化2 (実装テーブル構成)

#### テーブル1 (ユーザ情報)
|id|password|type|name|orgid|year|
|---|---|---|---|---|---|
|azm|????|su|間宮|shizuoka|2019|
|dai|????|su|宮川|shizuoka|2019|
|ken|????|ge|けんしん|shizuoka|2019|
|tomo|????|ge|土屋|shizuoka|2019|

#### テーブル2 (組織情報)
|orgid|orgname|
|---|---|
|shizuoka|静大|


#### テーブル3（トレーニング結果）
|id|day|weather|humidity|training|time|bweight|aweight|water|
|---|---|---|---|---|---|---|---|-
|azm|2019-08-02|0|50|マラソン|2|70|68.9|0|
|dai|2019-08-02|1|40|twitter|2|60|59.8|0.1|
|tomo|2019-08-02|2|30|ゲーム|2|55|54.9|0.3|
|azm|2019-08-03|0|50|マラソン|2|70|68.9|0|
|dai|2019-08-03|1|40|twitter|2|60|59.8|0.1|
|tomo|2019-08-03|2|30|ゲーム|2|55|54.9|0.3|

#### テーブル4 (掲示板)
|day|to|from|title|contents|
|---|---|---|---|---|
|2019-08-11|ALL|azumi|名言|天才とは努力する凡才のことである．|
|2019-08-12|ALL|azumi|名言|今日は暑いですね．|

※ALLは全員へ

## 2.4. アプリケーションサーバーのプログラム説明

### プログラム my_function2_csv.py
#### get_user_dic()
すべてのユーザーのIDとパスワードを取得（{'username':'password'}） 例:{'azumi':'mamiya','daiki':'miyagawa'}

#### def get_user_info()
すべてのユーザーのID，パスワード，種別，実名，組織，年度を取得（辞書形式）

#### sql_ALLuser_profile(user_name, user_pass)
すべてのユーザーのID，種別，実名，組織，年度を取得（辞書形式,パスワード以外,)

#### kakunin(user_name, user_pass)
一般ユーザー用のログイン処理，user_nameとuser_passが正しければTrue，誤りであればFalseを返す．

#### admin_kakunin(user_name, user_pass)
管理者用ログイン処理

#### get_admin()
管理者のリストを取得

#### sql_data_send(user_name, user_pass, bweight, aweight, training, time, water, weather, humidity, temp)

データを追加する

#### sql_data_get(user_nm)

データを取得する

#### sql_data_get_latest_all()

最近のデータを取得する

#### sql_message_send(userid, userpass, group, title, contents)
掲示板に文章を追加

#### sql_message_get(userid, userpass, max_messages = 10)
掲示板の内容を取得

#### adduser(admin,adminpass,info)
ユーザーを追加する

#### sql_data_per_day(day)
day日のみのデータを取得？



#### sql_makecsv(file)
CSVデータを生成する









#### kakunin(user_name, user_pass)

### プログラム server.py
#### <一般ユーザー用>
#### 関数 entry()
アプリにアクセスしたとき，ログイン画面を送信  
#### 関数 show()
IDとパスワードが合っていれば，そのIDの結果の画面を送信する．
#### 関数 hello()
データ入力フォームを送信
#### 関数 enter()
入力データを送信する．
#### <管理ユーザー用>
#### 関数　admin_entry()
管理者用のログイン画面を送信する．
#### 関数 admin_show()
管理者用のメイン画面を送信する．
#### 関数 admin_watch()

