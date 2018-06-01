# restCrawler

 restCrawler は、TwitterのAPIを使って、指定したキーワードを持つツイートを制限の範囲で遡れるだけ遡って取得を行い、JSONデータをMongoDBに格納するプログラムです。

 ## 注意事項
  __このプログラムは、動作解説用のサンプルです。__
 
  本来ならば必要な、エラー処理やログ取得といったアレコレを全てすっ飛ばし、動作の概略を説明するためのものになっています。
  
  そのため、
  - 一応動作する筈ですが、チェックしてません  
  (本番プログラムから不要箇所を削って作ったため)。
  - 詳細の解説は別の場所で行っています。　<-後でやります
  - 無保証・無サポート。  
  使うのもご自由にどうぞですが、何の保証もしません。

  また、
  - 素人の手習いのため、コードの汚さには目をつぶってください(特に変数名とか)。
  - Twitter APIに関する基本的な事項(開発者登録など)はご自分で。
  - APIの制限に関しては、Twitter社に問い合わせてください。
  - 濫用によってアカウント停止とか食らっても泣かないこと。

---
## 動作環境と必要パッケージ
### 動作環境
　(元となったプログラムで)確認している動作環境は以下の通りです。

- Windows + Anaconda3(Ver 3.6)
- Debian + Docker(Alpine Linux + Python 3.6)

 奇をてらった書き方はしていないので、python3以降なら動くハズ

### パッケージ
 依存パッケージのうち、標準外のものは以下の通りです。

 - [Tweepy](https://github.com/tweepy/tweepy)  
　GitHubから最新のものを取得することを推奨。動作確認は3.6.0

 - pymongo  
  データの格納を行っています。pipで取れる最新で問題なし。

  他、Sqlite3 を使ってます。付属の"setting.db"の通りのテーブルが必要です。  
  (改めて作る時は、「CREATE TABLE "GeneralSettings" ([MaxID] INTEGER);」を実行する。)

---
## 使い方

1. MongoDBを用意する  
このプログラムでは認証等は省略しているので、そのように設定する。  
(じゃなかったら、スクリプト側を認証対応に書き換える)
2. Pythonが動作する環境で、「restCrawler.py」と「setting.db」を同じフォルダに置く
3. restCrawler.py内の設定とかを環境にあわせて書き換えて保存。
4. 以下のコマンドを実行  

~~~ sh
$ python restCrawler.py
~~~
(たまに"python"が"python3"だったりするが、環境に応じて柔軟に。)

---
## 実行の注意事項
- **下手なキーワードで実行すると、ものすごい量のツイートであふれます。**  
DBのデータサイズやメモリサイズなどには十分な注意が必要です。
- 認証には、[アプリケーション単独認証](https://developer.twitter.com/en/docs/basics/authentication/overview/application-only.html) を利用しています。  
ツイート取得には有利ですが、その他の制限があるので気をつけてください。
- APIの実行リミット回避はTweepyに依存しています。  
リミット制限時の動作についてはTweepyのソースを参照してください。こちらでは(オプション指定以外)何もしてません。

---
## その他
 TwitterのAPIについて、2018年初旬から大きな変動が起きつつあります。このスクリプトは2017年の秋に作られ、その時点のAPI仕様に基づいてデータが取得されることを期待しているもののため、状況の変化によって全く利用できなくなる可能性があります。
