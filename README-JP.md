# 世界初デジタル香りハッカソン

[English](README.md) / [日本語](README-JP.md)

Hackaromaのメインページ: [こちら](https://www.aromajoin.com/hackaroma)

1. 概要
   - 期間: 5月13日～6月19日
   - 公式ルール: [PDF](https://drive.google.com/file/d/1pwpCksr0kRWzzq3HsPF0bcMUr-uwWLaL/view)
   - アイデア選考ラウンド（5月13日～5月28日）:
     - 提案書ってどのように作れる？ → [ガイド](https://paper.dropbox.com/doc/Perfecting-your-Hackaroma-Proposal--AzWa4BFYALfWgkcztSeRTRhaAQ-8VblQZyV0ehKdyAmCSeOV)
     - 提案書ってどんなイメージ？→[サンプル](https://www.dropbox.com/s/scac6vm2f5fsuzf/200508_HackaromaProposalTemplateJP.pdf?dl=0)
   - 開発ラウンド（5月28日～6月19日）:
     - 開発に進む6チーム（個人可）は自分のアイデアを実現します。開発キットの発送には努めますが、開発キットが届く前から開発を進めるのが望ましいです。

     - アロマシューターはiOS, Android, Java, Javascript, Python, C++等環境でSDKが整っているAWS IoTを使ってインターネットを通して動かせます。もっと見たい人は [こちら](https://docs.aws.amazon.com/iot/latest/developerguide/iot-sdks.html)から見られます。詳しい説明は下に書きます。

     - 最終日6月19日（金）後で定める時間にオンライン発表会が行われます。開発に進む6チームは自分のアイデアや実現について発表してデモを行います。デモの時事前に登録されたアロマシューターを持っている人は全て同じ香りを体験できるようにします。それは**世界初香りデータのライブストリーミング**です。

     - 開発に進む6チームと遠隔にいる審査員はそれぞれアロマシューターを一台、アロマカートリッジを１セットを持ちます。全てのカートリッジセットは同じで、１セットにあるカートリッジ数は決まっていなんですが、6個以上です。

2. アロマシューターを使った事例等
   - [香りを加えた VR](https://www.dropbox.com/s/9xse6isg22fhuw9/200109_VRHeroVideo.mp4?dl=0)
   - [香り Walkman](https://www.youtube.com/watch?v=r9MUcdwxsR4)
   - [リラックス用香り](https://www.youtube.com/watch?v=p1f5A-vXAv8)
   - [アロマサイネージ](https://aromajoin.com/solutions/aroma-signage)
   - [その他](https://aromajoin.com/solutions/arts-and-science)

3. 開発ラウンドでどんな香りが使えますか？

   - 開発に進む6チームが使えるアロマカートリッジのセットが決まりました。各チームはそれぞれアロマシューターキットの一台と下記香りリストの一個ずつもらえます。

     - Chanel #5の模倣
     - マンゴ
     - コーヒー
     - ラベンダー
     - フォレスト
     - ヒノキ
     - スペアミント
     - お香
     - レモン
     - フレッシュな刈った草
     - 燃えたゴム
     - 海
     - キャラメル

   - 合計13種類（キャラメルを追加することになりました）ありますが、同時に6種類までアロマシューターで使えます。どう思いますか？何かいいアイデアに繋がりそうですか？

4. どうやってインターネット上アロマシューターを制御することができますか？ ([古くなった設定](/assets/files/old_AromaShooterControlGuide-JP.md))

   - HTTPを基づいたアロマシューター制御フロー。
   
    ![Flow of controlling Aroma Shooter via Internet](/assets/images/HTTP4AS-JP.png)
   
   - 手順
   
     - 最初はアロマシューターをインターネットが繋がるローカルネットワークに繋げます。やり方が分からない場合は[こちら](https://github.com/aromajoin/controller-http-api)を参照してください。
     - HTTPリクエストを送信してアロマシューターを制御します。※ *アロマシューターは何も制御されずに1時間程度たってから自動的に切断します。その場合、電源を外してからもう一度差し込んだら再起動して接続できます。*
   
   - ユニキャスト対ブロードキャスト
   
     - キャスティングタイプをユニキャストにしたら、ファイナリストの方々は自分の持っているアロマシューター**だけ**制御できます。
     - キャスティングタイプをブロードキャストにした場合、ファイナリストの方々は自分が持っているアロマシューターを含めて事前に登録された全てのアロマシューターを制御できます。
     - **開発期間中**はユニキャストともブロードキャストともファイナリストの方々の持っているアロマシューターだけ制御できます。
     - ファイナリストの方々にそれぞれ**リハーサル**時間を設けて、あそこでブロードキャストは用意しているテスティングデバイスの全てとそのファイナリストが持っているアロマシューターを制御できます。
     - 最終発表時ではブロードキャストは全てのデバイスを制御するので、**当日はデモ以外の目的でブロードキャストをめったに使わないでください。**
   
   - どんなキーが必要ですか？
   
     - API Key: メールで共有させて頂きます。
     - Private Key: メールで共有させて頂きます。ファイナリストの方々はそれぞれ別のキーを用意しています。
   
   - HTTP APIはどんなものですか?
   
     - **要注意!**: アロマシューターはあるポートから連続で10秒以上噴射したら壊れる場合があります。例えば、ポート3から15秒噴射したい場合、5秒噴射、5秒休憩、5秒噴射という形はお勧めします。
   
     - URL: https://lvxcf3yn35.execute-api.us-west-2.amazonaws.com/prod/as2/control
   
     - リクエストの種類: POST
   
     - API Keyはヘーダに入れ込む必要があります。
   
     - 送信データー: 下記形式のJSONデーターです。
   
       - 噴射する：
   
         ```json
           {
               "private_key":"your_private_key_that_we_provide",
               "casting":"unicast/broadcast",
               "type":"diffuse",
               "duration":"time_to_diffuse_in_milisecond",
               "booster":"true/false", // 無臭ポートを噴射するかしないかの設定
               "channel":"1~6",
               "intensity":"0-100"
           }
         ```
       - 例:
   
         ```json
           {
               "private_key":"place_holder",
               "casting":"unicast",
               "type":"diffuse",
               "duration":"3000",
               "booster":"true",
               "channel":"1",
               "intensity":"100"
           }
         ```
   
           
   
       - 噴射停止:
   
         ```json
         {
             "private_key":"your_private_key_that_we_provide",
             "casting": "unicast/broadcast",
             "type": "stop",
             "channel": "1~6"
         }
         ```
   
       - 例:
   
         ```json
         {
             "private_key":"place_holder",
             "casting":"broadcast",
             "type":"stop",
             "channel":"1"
         }
         ```
   
         
   
   - どうやってHTTP リクエストを試せますか?
   
     - お勧めは Postmanです。
      ![Postman url and header set up](/assets/images/postman_header.png)
      (URLとヘーダ設定)
      ![postman http data set up](/assets/images/postman_data.png)
      (JSON データ設定)
   
   - 一日何回までリクエストを送れますか?
   
     - 本APIサーバーは有料サービスであるAWS Serverless frameworkを使っています。一日全部で5000リクエスト程度受付できます。
     - その数を超えた場合、一時的サーバーを使えません。日変わったらまた使えるようになります。
     - おおよそファイナリストの方々はそれぞれ一日800回程度使えます。覚えておいてください。
   
   - 最後に、何か質問があったら、お気軽に hackaroma@aromajoin.comまで連絡してください。
