# Watson Visual Recognitionのカスタムモデル使ったNode-RED画像認識アプリ作成 
<br>


## 1. 事前準備
### 1. IBM Cloudのアカウントを取得
お持ちでないかたは [こちら](https://cloud.ibm.com/registration?cm_mmc=Email_Events-_-Developer_Innovation-_-WW_WW-_-nishito\tokyo\japan&cm_mmca1=000019RS&cm_mmca2=10004805&cm_mmca3=M99938765&cvosrc=email.Events.M99938765&cvo_campaign=000019RS
)から取得お願いします。

取得方法の動画はこちらです: https://youtu.be/Krn1jQ4iy_s


## 2. Visual Recognition サービスの作成
Visual Recognition サービスを作成していない場合は、作成します。既に作成済みであれば、作成済みのものを使用できます。

>画面イメージのある手順を参照したい場合は[Watson APIを使うための前準備: サービスの作成と資格情報の取得](https://qiita.com/nishikyon/items/9b8f697db7ad0a693839)の[1. Watson サービスの作成](https://qiita.com/nishikyon/items/9b8f697db7ad0a693839#1-watson-%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9%E3%81%AE%E4%BD%9C%E6%88%90)を参照して下さい (`2. サービスの資格情報取得`は実施不要です)。以下の`6. (オプション) カスタム分類クラスの作成` はその後に実施ください。

1. https://cloud.ibm.com/login よりIBM Cloudにログイン

2. 表示された「ダッシュボード」の上部のメニュー「カタログ」をクリック

3. 表示された「カタログ」の上部の検索フィールドに`Visual Recognition`と入力し、「検索」ボタンをクリック。

4. 表示された AIカテゴリの`Visual Recognition`をクリック。

5. `Visual Recognition`サービス作成の画面が表示されるので、右側のの'作成'をクリックして、サービスを作成する。

6. (オプション) カスタム分類クラスの作成

自分の写真で分類クラスを作成したい場合は実施してください。
>こちらは省略可能です。省略した場合はIBMが提供する食品に特化した分類クラス`food`を使用します。

[Watson Visual Recognition カスタムクラスを作ろう!](https://qiita.com/nishikyon/items/7d1c07e2f50c1002e815)を参照してカスタム分類クラスの作成を行ってください。
[10. トレーニング開始](https://qiita.com/nishikyon/items/7d1c07e2f50c1002e815#10-%E3%83%88%E3%83%AC%E3%83%BC%E3%83%8B%E3%83%B3%E3%82%B0%E9%96%8B%E5%A7%8B)まで実施お願いします。

## 3. Node-REDの立ち上げ
[IBM CloudでNode-REDの立ち上げ方](https://qiita.com/nishikyon/items/46c15bc392e6274dd41d)を参照して、Node-REDフローエディタの立ち上げまで行ってください。

[2.　必要な場合: サービスの接続](https://qiita.com/nishikyon/items/46c15bc392e6274dd41d#2%E5%BF%85%E8%A6%81%E3%81%AA%E5%A0%B4%E5%90%88-%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9%E3%81%AE%E6%8E%A5%E7%B6%9A)で2で作成した`Visual Recognition サービス`を接続してください。

## 4. フローの読み込み
1. [watsonflow_full.json](https://raw.githubusercontent.com/kyokonishito/watson-vr-nodered/master/watsonflow_full.json)をPCにダウンロードします。

2. [IBM Cloud Node-RED JSON形式のフローデータの読み込ませ方](https://qiita.com/nishikyon/items/1bdf3bccdfe64100cf1d)を参照して、ダウンロードした[watsonflow_full.json](https://raw.githubusercontent.com/kyokonishito/watson-vr-nodered/master/watsonflow_full.json)を読み込ませます。

## 5. (オプション)　カスタム分類クラス　Classifier IDの指定
Visual RecognitionのカスタムクラスのIDを記入します。

カスタムクラスを作成していない場合は、IBMが提供している食物に特化したカスタムクラスのid,`food`が指定済みです。

1. 2で[Watson Visual Recognition カスタムクラスを作ろう!](https://qiita.com/nishikyon/items/7d1c07e2f50c1002e815)を参照してカスタムクラスを作成した場合は、
[11. モデルの表示](https://qiita.com/nishikyon/items/7d1c07e2f50c1002e815#11-%E3%83%A2%E3%83%87%E3%83%AB%E3%81%AE%E8%A1%A8%E7%A4%BA) と[12. Classifier IDの取得](https://qiita.com/nishikyon/items/7d1c07e2f50c1002e815#12-classifier-id%E3%81%AE%E5%8F%96%E5%BE%97)の手順に従い、Classifier IDをコピーします。

2. ノード「カスタム認識のパラメータ設定」をダブルクリックします。

3. 4行目の`food`を１でコピーしたClassifier IDに置き換えます。

置き換え前:
```
msg.params.classifier_ids = "food";
```

置き換え後の例:
```
msg.params.classifier_ids = "CustomModel_1211194384"
```
4. 右上の「デプロイ」をクリックします。

## 6. アプリケーションの動作確認
ブラウザーで
`https://＜アプリケーション名＞.＜ドメイン名＞/watsonvr`　にアクセスします。

よくわからない場合は
今開いているNode-REDの画面のURLの先頭部分が
`https://＜アプリケーション名＞.＜ドメイン名`です。
これに`/watsonvr`をつけてください。

アプリケーションの画面が表示されます。
`「ファイルの選択」`から写真を選んだ後、各青ボタンをクリックして、Visual Recognitionの結果を確認します。

- Watsonで認識（Watson学習済みモデルを利用):
  -W atsonが写真を認識した内容を表示します。

- Watsonで認識（カスタムモデルを利用):
  - カスタムモデル認識したクラスを表示します。

>スマートフォンでの確認：

>一番下にQRコードが表示されているので、それをスマートフォンのカメラで読んでUアプリケーションのRLにアクセすると、スマートフォンでも結果を確認できます。スマートフォンでは`「ファイルの選択」`ボタンでその場で撮った写真も認識可能です。






