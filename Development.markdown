# アプリケーションの開発環境
## Unity
HoloLensの3Dアプリケーション開発環境は，[Unity](https://unity3d.com/jp)を使った開発がスタンダードであり，ストアに並ぶHoloLens3Dアプリケーションの多くがUnityで作られたものです。  

Unity（別名:Unity3D）は、統合開発環境を内蔵し、複数の機材(platform)に対応するゲームエンジンです。
Unityのバージョンによって，HoloLensで利用できるAPIやサポートされる機能が変わってきます。
現在の推奨環境は，[2017.4.x LTS](https://unity3d.com/jp/unity/qa/lts-releases) です。2018-2020の2年間のサポートが約束されており，安定性を求める場合こちらのバージョンを使うことをお勧めします。  
最新のUnity2018でも開発は可能ですが，後述するMixed Reality Toolkit の相性問題もあるため最新の環境が最良とは断言できません。

---
## Visual Studio
コードを書くためのツールは何でもいいのですが，Unityで作ったアプリをビルドし，HoloLens上に展開するためには，[Visual Studio 2017](https://imagine.microsoft.com/ja-jp/Catalog/Product/530) が必要になります。  

---
## Windows 10 SDK
HoloLensアプリをビルドするためには，Windows 10 SDK が必要です。環境は構築できているのにビルドできないという場合，Windows 10 SDKが入っていないという事がよくありました。HoloLensのアプリ開発には10.0.14393が最低限必要でしたが，例えばImmersive向けでコントローラを扱うには最低限10.0.16299以上であったり，更に振動機能が使いたい場合は10.0.17134が必要になります。MRTKを使う時には特に引っかかるポイントだったりします。  

### インストールする
Windows 10 SDK は[Archive](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive)から直接DLもできますが，Visual Studioからインストールするのが楽です。  
「Visual Studio Installer」 を立ち上げて，「詳細->変更」を押すと一覧が表示されます。「ユニバーサルWindowsプラットフォーム開発」を選択してインストールすると最新のWindows 10 SDK がインストールされます。また，この時オプションから過去のバージョンのWindows SDK も指定してDLすることができます。

---
## Mixed Reality Toolkit
[Mixed Reality Toolkit](https://github.com/Microsoft/MixedRealityToolkit-Unity)は，HoloLensのアプリケーション開発をサポートするコンポーネント群です。よく勘違いされるのですが，MRTKはHoloLensアプリの開発に必ずしも必要はありません。ですが，MRTKを使うことでコーディングが苦手でも開発が楽にすむ部分が多いです。例えばHoloLens用のカメラオブジェクトや入力，UX周りをPrefabを用いて直ぐにシーンを構築できるだけではなく，UnityProjectの初期設定やアプリケーションのデプロイなどもサポートしてくれます。  

Mixed Reality Toolkitのバージョンはいくつかあり，先のGitHubリンクの[Release](https://github.com/Microsoft/MixedRealityToolkit-Unity/releases)からダウンロードすることができます。HoloToolkit-Unity-XXXX.X.XがMRTKの本体で，HoloToolkit-Unity-Examples-XXXX.X.Xでその使い方を学ぶことができます。

注意点として，Mixed Reality Toolkit はUnityの多くのバージョンをサポートしてくれてはいますが，最新の環境では動作しなかったり，Unity5といった古いバージョンでは一部動作しなかったりします。利用したい環境に合わせた適切なパッケージを選択することが重要になりますので，<b><u>Upgrade Guide をよく読みましょう。</u></b>

以下はMRTK 2017.4.0.0の例です。

- Unityのバージョン: 2017.4.x
- HoloLensでもImmersiveでも両方で動作させたい場合のUnityのバージョン: 2017.2~
- Unityの最低推奨バージョン: 2017.1 (5.6でも動作はする)
- Windows SDK: 10.0.17134 (Unity2017.2~の場合)
- Visual Studio 2017
- Fall Creators Update

### MRTKの機能
MRTKは多機能で数多くのコンポーネントがあります。

#### 概要把握
どんなことができるかの全体を把握するにはこちらのまとめがおすすめです。  
[Mixed Reality Toolkit (MRTK)の有用なビルディングブロック](https://medium.com/@dongyoonpark/mixed-reality-toolkit-mrtk-%E3%81%AE%E6%9C%89%E7%94%A8%E3%81%AA%E3%83%93%E3%83%AB%E3%83%87%E3%82%A3%E3%83%B3%E3%82%B0%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF-4eb343d474cb)

#### 機能一覧
|機能名|説明|HoloLens|IHMD|
|:--:|:---|:---:|:---:|
|Input|Gaze，ジェスチャー，音声入力，モーションコントローラを扱うライブラリ|〇(モーションコントローラを除く)|〇|
|Sharing|複数デバイス間でやりとりするためのライブラリ|〇|〇|
|SpatialMapping|HoloLensで生成される空間マッピングを扱うライブラリ|〇|×|
|SpatialSound|空間音響を扱うライブラリ|〇|〇|
|UX Controls|BoundingBoxなどUXを扱うライブラリ|〇|〇|
|Utilites|共通で使えるヘルプツール|〇|〇|
|SpatialUnderstanding|ソファや壁などの部屋にあるものを大まかに識別するライブラリ|〇|×|
|Build|ビルドとデプロイの自動化ツール|〇|〇|
|Boundary|Immersive用に壁や床，移動可能範囲を描画する|×|〇|

#### MRTKを使ったアプリケーション作成までの手順

たぶんここが一番シンプルで分かりやすいと思います。  
[HoloLens ホログラフィックアプリケーションを作ってみた（Cube初級編）](https://www.nttpc.co.jp/technology/holoLens.html)

一連の開発プロセスを学ぶにはこちらのスライドがおすすめです。  
[UnityによるHoloLensアプリケーション入門](https://www.slideshare.net/YuichiIshii/unityhololens-92864583)

---

## Holographic Remoting
ちょっとした変更に対して，HoloLensアプリを毎回実機にビルド，デプロイするのは時間も手間もかかります。Holographic Remoting はその確認作業の手間を大きく削減してくれます。  
Holographic Remotingを使うには，HoloLensとUnityEditor側でそれぞれ準備が必要です。  

### HoloLens: 
HoloLensのMicrosoft Storeから，[Holographic Remoting Player](https://www.microsoft.com/ja-jp/p/holographic-remoting-player/9nblggh4sv40) をDLし起動する。

### UnityEditor: 
1. File-> Build Settings -> Player Settings -> XR Settings -> Virtual Reality Settings を有効にする
2. Window -> Holographic Emulation -> Emulation Mode を None から Remote to Device に変更する
3. Remote Machine に HoloLens に表示されているIPアドレスを入力する -> Connect ボタンを押す
4. UnityEditorを再生するとHoloLensに映像が表示されるはずです。

### よくある誤解と注意点
Holographic Remotingは確かに便利機能ですがあくまで実行しているのは，PC側でHoloLens上ではありません。エアタップといった入力イベントはシミュレートできますが，HoloLensのカメラは利用できない，つまりVuforiaといったHoloLensのカメラを使ったデバッグはできません。また，UWPのコードも同様に実行されるのはEditor環境なので実行されないということに注意してください。

## Trouble Shooting
### 調べる
OSやUnityのバージョンによっては描画が安定しない，クラッシュするなど動作が不安定な場合があります。解決しない場合，既知の問題の可能性があります。[Issue Tracker](https://issuetracker.unity3d.com/) や [Release Note](https://unity3d.com/jp/unity/whats-new/unity-2018.2.0)が問題解決のヒントになります。また，MRTKの不具合に関しては[issue](https://github.com/Microsoft/MixedRealityToolkit-Unity/issues)から検索しましょう。

### 聞いてみる
HoloLensのFBコミュニティグループで聞いてみると誰かしらから反応があると思います。  
[HoloLens/Windows Mixed Reality ユーザーズグループ](https://www.facebook.com/groups/149608962203470/?ref=group_header)

他にもTwitterで活動しているHoloLensアプリデベロッパーが多くいるので，捕まえて聞いてみるのも手です。

    ※質問の際は，Unityのバージョン，MRTKのバージョンなど開発環境を明記して聞きましょう。  
    問題が開発環境由来の場合が多く，それだけで問題が解決する場合も多いです。  
    回答する側も問題の切り分けがしやすいので意識してみましょう。

---
## 書籍
HoloLens参考書読書会で使われた書籍です。英語の書籍ですが，[こちら](http://blog.d-yama7.com/hololens_reading)に日本語での登壇資料がまとまっています。  
・[Microsoft HoloLens Developer's Guide](https://www.amazon.co.jp/dp/B01MUP8J5E/ref=dp-kindle-redirect?_encoding=UTF8&btkr=1)

VRコンテンツ開発ガイドという名前ですが，HoloLens開発の入門にもスポットを当てています。  
・[VRコンテンツ開発ガイド 2018](https://www.amazon.co.jp/dp/4844367846/ref=cm_sw_r_cp_ep_dp_n2-vBbYNEKHPV)

こちらはHoloLensというよりImmersiveを中心に，日本マイクロソフトの高橋忍さんが執筆された本です。  
・[Windows Mixed Realityアプリ開発入門　Unityで作るVR＆HoloLensアプリケーション](https://www.amazon.co.jp/gp/product/B07F73DK7W?ie=UTF8&camp=1207&creative=8411&creativeASIN=B07F73DK7W&linkCode=shr&tag=yuuji01-22&btkr=1)

---
## ブログ/Qiita
|||
|:---|:---|
|[MRが楽しい](http://bluebirdofoz.hatenablog.com/)|HoloLensに関する記事投稿数は断トツで多いです。内容も丁寧で読みやすいです。|
|[D.YAMA Blog](http://blog.d-yama7.com/)|イラストも多く，HoloLensの入力周りについて非常に丁寧に説明されています。特に位置合わせに関する記事は参考になると思います。|
|[なんかいろいろしてみます](http://akihiro-document.azurewebsites.net/categories/hololens/)|HoloLensの実装でよく当たる問題について解説付きのコードが載っています。|
|[たるこすの日記](https://tarukosu.hatenablog.com/entry/2016/11/26/184525)|HoloLensを様々なデバイスと繋げています。|
|[miyauraさんのQiita](https://qiita.com/miyaura)|Qiitaで多くの記事を投稿しています。MRTKの使い方についてはmiyauraの記事が参考になると思います。|
|[morio36さんのQiita](https://qiita.com/morio36)|主にHoloLensとAzureとの連携記事を多く投稿しています。|
|[凹Tips](http://tips.hecomi.com/entry/2017/02/12/211458)|HoloLensの記事数はあまり多くはないですが非常に参考になるかと思います|
|[HoloLens光学系の謎に迫る](https://www.slideshare.net/AmadeusSVX/hololens-85758620)|こちらはSlideですがHoloLensの仕組みに迫る内容なので一度読んでみるといいと思います。|
|[デコシノニッキ](https://www.tattichan.work/)|このWikiを書いている人のブログです。雑多です。|

---
## Mixed Reality Academy
[Mixed Reality Academy](https://docs.microsoft.com/en-us/windows/mixed-reality/academy) はマイクロソフトからMRコンテンツを開発するための学習ページです。内容は全て英語で，アップデートもそこまで頻度は高くはありませんが，開発で使う機能の一つ一つを学習することができます。HoloLensだけではなく，Immersiveの学習内容もサポートされており，Azure連携の仕方も学べます。

