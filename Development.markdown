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

## Capabilities
UnityからアプリをデプロイしてもCapabilitiesが有効になっておらず，機能が使えないという場合があります。例えば，カメラやマイクにアクセスする場合や，クライアントとしてサーバーと通信する，ファイルへアクセスする場合などはCapabilitiesでアプリケーションが利用できるように権限を与えてやる必要があります。  CapabilitiesはUnityのFile-> Build Settings -> Player Settings -> XR Settings -> Publish Settingsから設定します。

量が多いので代表的なものだけあげていきます。

|Capabilities|説明|
|:--:|:---|
|InternetClient|Sharingなどネットワークを経由して情報をやりとりする場合に必要です。|
|WebCam|VuforiaなどCameraを使う場合に必要です。|
|Microphone|音声入力などを使う場合に必要です。|
|SpatialPerception|SpatialMappingを使う場合に必要です。|
|Bluetooth|Xboxコントローラなどを使う場合に必要です。|

---

# Scripting Backend
UWPのアプリケーション開発ではこのScripting backendを.NETかIL2CPPを選択する必要があります。あまり解説記事も見当たらずおまじない的な扱いで使われている気がします。表面的な違いで言えば以下の違いになります。

||ビルド速度|実行速度|C#プロジェクト出力の可否|サポート|
|:---:|:---:|:---:|:---:|:---:|
|.NET|早い|速くはない|可|将来的に切られる|
|IL2CPP|遅い|速い|不可|積極的に移行作業が進む|

また，.NET環境とIL2CPP環境のそれぞれで，動くものと動かないものがあったりします。例えばMRTKで過去のissueを検索してみると，IL2CPPではSharingが動作しないなどといった報告も見かけます。

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

---


