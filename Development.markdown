# アプリケーションの開発環境
HoloLensのアプリケーションを開発する上で，いくつかのツールが必要になります。またそれらツールは単に使えばいいという訳ではなく，バージョンや設定についても考えなくてはなりません。ここでは，各ツールの紹介と解説をします。

---
# 開発ツール
## Unity
HoloLensの3Dアプリケーション開発環境は，[Unity](https://unity3d.com/jp)を使った開発がスタンダードであり，ストアに並ぶHoloLens3Dアプリケーションの多くがUnityで作られたものです。  
Unity（別名:Unity3D）は、統合開発環境を内蔵し、複数の機材(platform)に対応するゲームエンジンです。
Unityのバージョンによって，HoloLensで利用できるAPIやサポートされる機能が変わってきます。
現在の推奨環境は，[2017.4.x LTS](https://unity3d.com/jp/unity/qa/lts-releases) です。2018-2020の2年間のサポートが約束されており，安定性を求める場合こちらのバージョンを使うことをお勧めします。  
最新のUnity2018でも開発は可能ですが，後述するMixed Reality Toolkit の相性問題もあるため最新の環境が最良とは断言できません。

### UnityHub
Unity公式から提供されている複数バージョンのUnityEditorを一括で管理するツールです。これまでのUnityは異なるバージョンをインストールする度にショートカットが生成され，それらを使い分ける形をとっていました。UnityHubでは，UnityHubから指定したバージョンでプロジェクト新規作成，プロジェクトの一覧管理，，Hub経由でインストールしたUnityに後からVuforiaなどのコンポーネントを追加するなどができます。[(UnityHubを用いて複数バージョンのUnityEditorを管理する)](https://qiita.com/k7a/items/ca4547725434580bc3a9)  

Vuforiaや後述するScriptingBackendの入れ忘れをした時に後からの導入が楽になるので，HoloLensアプリ開発でも役に立ちます。

[DownLoad](https://public-cdn.cloud.unity3d.com/hub/prod/UnityHubSetup.exe)

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

# Unityの設定
Unityでアプリケーションを作成する際には，どのプラットフォーム向けのアプリケーションを作成するか指定します。File->Build Settingsからプラットフォームを指定してSwitch Platformで切り替えます。HoloLensでは，<b> Universal Windows Platform </b>を選択します。このUWPとは，様々なデバイス上で動作するアプリケーションを単一のフレームワーク上に統合する仕組みです。ただ，System.IOなど一部UWPでは使い方が異なるAPIがあったり，そもそもUWPでは存在しないAPIがあるなどくせ者であり，このような背景からAssetStoreで購入したAssetが他のプラットフォームでは動くが，UWPでは動かないなんてことがよくあります。

---
## Build Settings
アプリケーションのビルドに必要な項目です。File->Build Settings もしくは Ctr + Shift + B(Buildの意) で開きます

### Target Device
HoloLensを指定しても，そのままのAnyPlatformでも特に問題はありません。

### Build Type
3Dのアプリケーションを作る際にはD3Dを，Edgeのような2Dのアプリケーションを作る際にはXAMLを指定します。

### SDK
先の Windows 10 SDK のことです。基本的には新しいSDKが入っていれば特に問題ありません。プルダウンからSDKが入っているかどうか確認してください。

### Visual Studio Version
2017を選択してください。

### Unity C# Projects
.NETを指定している場合のみ選択できます。slnファイルをC#で書き出せるので，Unityから都度ビルドしなくとも，こちらのslnファイルからコードの編集ができます。また，UWPのコードを書く際には生成したslnファイルから編集するのが楽です。

---
## Player Settings
さまざまなオプションを設定します。

### Other Settings
#### Scripting Runtime Version
.NET Frameworkサポートと同等のバージョンが利用できます。2017までは.Net3.5が標準で，.Net4.6は試験的に利用可能でした。2018からは.Net4.6が標準になります。.Net4.6ではコーディングで受ける恩恵が多い上，MRTKでも問題なく動作するため，外部のAssetとの相性が悪くない限り4.6がいいでしょう。

[C#6.0時代のUnity](https://qiita.com/divideby_zero/items/71a38acdbaa55e88e2d9)

#### Scripting Backend
UWPのアプリケーション開発ではこの[Scripting backend](https://docs.unity3d.com/ja/current/Manual/windowsstore-scriptingbackends.html)を.NETかIL2CPPを選択する必要があります。どちらでもいいという訳ではなく，.NET環境とIL2CPP環境のそれぞれで，動くものと動かないものがあったりします。例えばMRTKで過去のissueを検索してみると，IL2CPPではSharingが動作しないなどといった報告も見かけます。  
あまり解説記事も見当たらずおまじない的な扱いで使われている気もしますが，表面的な違いで言えば以下の違いになります。

||ビルド速度|実行速度|C#プロジェクト出力の可否|サポート|
|:---:|:---:|:---:|:---:|:---:|
|.NET|早い|速くはない|可|将来的に切られる|
|IL2CPP|遅い|速い|不可|将来的に移行|

これらの違いはJITコンパイルとAOTコンパイルの仕組み，違いをまず理解する必要があります。

【参考】
- [IL2CPPのしくみ](https://docs.unity3d.com/ja/current/Manual/IL2CPP-HowItWorks.html)
- [IL2CPPに関する軽い話](https://www.google.co.jp/url?sa=t&rct=j&q=&esrc=s&source=web&cd=6&cad=rja&uact=8&ved=2ahUKEwiKtLyY3OHcAhUEQN4KHcxdCxcQFjAFegQIBhAB&url=https%3A%2F%2Fwww.slideshare.net%2FWooramYang%2Fil2-cpp-wooramyang-49666751&usg=AOvVaw2_L54wiVpvcgtfgYa2Lqvu)
- [【Unity】Stripping Levelについて (プラットフォームによる差異の項)](https://www.f-sp.com/entry/2016/02/23/180816)

##### 導入
.NETまたはIL2CPPのいずれかが<b>アプリケーションのビルドに必要です。</b> UnityにComponentがなければビルドできません。

###### Unityを新規にインストールする場合
以下の項目をOnにしてインストールしてください。
- 2017: Windows Store .Net (or IL2CPP) Scripting Backend
- 2018: UWP Build Supported .Net (or IL2CPP)

###### 既存のUnityに追加する場合
Unityのインストーラから再度導入する形になります。新規インストールと同じ手順です。

###### UnityHubから追加する場合
Installsから追加したいUnityのバージョンからAddComponentで導入できます。
- 2017: Windows Store .Net (or IL2CPP) Scripting Backend
- 2018: UWP Build Supported .Net (or IL2CPP)

##### IL2CPPでの開発
###### Nativeコードの記述
ネイティブコードはVS上で編集する上で、インテリセンスが効きません。.Netでは、UWP C#プロジェクトを出力しその先でネイティブコードの記述を行っていました。IL2CPPでは、ProjectName.Playerという専用のアセンブリファイルが生成されているのに気がつくと思います。このアセンブリ内では、AssemblyC-Sharpと同じ階層構造をもったファイル群が生成されており、この中ではインテリセンスが有効な状態でコードの編集が行えます。

###### ビルド速度
一般的にビルド速度は遅いです。そこで高速化のための4つのポイントを紹介します。  

1. インクリメントビルド  
１度ビルドしたら、それ以降は同じ出力先を選択してビルドするという方法です。これは恐らく自然にやっていることだとは思います。  

2. プロジェクトとビルドフォルダーをマルウェア対策のソフトウェアスキャンから除外  
Windows Defender-> ウィルスと驚異の防止 -> ウィルスと驚異の防止の設定の項の「設定の管理」-> 除外の項の「除外の追加または削除」から出力先を除外に追加します。これは大幅にビルド時間の短縮を狙えます。  

3. プロジェクトとビルドフォルダーを Solid State Drive (SSD) に格納  
SSDを使いましょう。  

4. Script Only Build  
ビルドの対象をスクリプトに絞り込みます。BuildSettingsでDevelopment モードを有効にした時のみに選択可能になります。一度ビルドしたTarget先を選択する必要になります。  

[HoloLensのIL2CPPでの開発](https://www.tattichan.work/entry/2018/09/22/HoloLensのIL2CPPでの開発)

---
### Publish Settings
#### Capabilities
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
### XR Settings
<b>Virtual Reality Supported が有効でないと3DViewになりません。必ず有効にしてください。</b>  

また，Virtual Reality SDKのSettingから指定できるEnableDepthBufferSharingは，安定化平面の設置に関わります。こちらは有効にする必要はありません。詳細は[こちら](https://www.tattichan.work/entry/depthbuffer)。  

#### 注意
Virtual Reality Supportedを有効にした状態でシーンを再生すると，Mixed Reality Portal が立ち上がってしまう場合があります。そのような場合，Window -> Holographic Emulation -> Emulation Mode を None から Remote to Device に変更することでこの問題を回避することができます。

---

# アプリケーションのビルド
　作ったアプリをHoloLensにインストールするには２段階のプロセスを踏みます。  

　1. Unityでビルドし，C# or C++ プロジェクトファイルを生成する  
　2. 生成したプロジェクトファイルをVSでビルドし，HoloLens上に展開する

## 1.Unityでビルドする
　Build SettingsからBuildを押します。この時出力先のファイルをきかれるので，出力用のファイルをHoloLensAppなどといった名前で新規生成し，そこをビルド先にしましょう。
 
## 2. VSでビルドする
　1でビルドした先を開くと，XXX.slnというソリューションファイルが生成されていますので，VSから開きます。開いたら，VS上部の<b>ローカルデバイスをデバイスに変更し，DebugをReleaseに変更，さらにその隣をx86に指定します</b>。設定が終わったら，HoloLensをUSBで接続し，デバッグなしで実行(Ctr+F5)を押してアプリをビルドします。

---

# Mixed Reality Toolkit
[Mixed Reality Toolit](https://github.com/Microsoft/MixedRealityToolkit-Unity)は，HoloLensのアプリケーション開発をサポートするコンポーネント群です。よく勘違いされるのですが，MRTKはHoloLensアプリの開発に必ずしも必要ではありません。

Mixed Relaity Toolkitには大きく2つのバージョンがあり，
Unity 5.6 - 2017 をサポートするバージョンと，2018以上をサポートするvNextと呼ばれるバージョンがあります。調べて出てくる情報の多くが前者に該当します。vNextは開発中のバージョンであり，その大きな違いはHoloLensやIHMD以外にもOpenVR対応など，より多くのプラットフォームをカバーしている点です。しかしながら，それぞれで使い方も作りも大きく異なるので別物と思った方がいいです。

先のGitHubリンクの[Release](https://github.com/Microsoft/MixedRealityToolkit-Unity/releases)からダウンロードすることができます。HoloToolkit-Unity-XXXX.X.XがMRTKの本体で，HoloToolkit-Unity-Examples-XXXX.X.Xでその使い方を学ぶことができます。

注意点として，Mixed Reality Toolkit はUnityの多くのバージョンをサポートしてくれてはいますが，最新の環境では動作しなかったり，Unity5といった古いバージョンでは一部動作しなかったりします。利用したい環境に合わせた適切なパッケージを選択することが重要になりますので，<b><u>Upgrade Guide をよく読みましょう。</u></b>

詳細は，
- [Mixed Reality Toolkit](https://github.com/tattichan/HoloLensQuestions/blob/master/MixedRealityToolkit.markdown)
- [Mixed Reality Toolkit (vNext)](https://github.com/tattichan/HoloLensQuestions/blob/master/vNext.markdown)

で扱います。

---

# Holographic Remoting
ちょっとした変更に対して，HoloLensアプリを毎回実機にビルド，デプロイするのは時間も手間もかかります。Holographic Remoting はその確認作業の手間を大きく削減してくれます。  
Holographic Remotingを使うには，HoloLensとUnityEditor側でそれぞれ準備が必要です。  

## HoloLens: 
HoloLensのMicrosoft Storeから，[Holographic Remoting Player](https://www.microsoft.com/ja-jp/p/holographic-remoting-player/9nblggh4sv40) をDLし起動する。

## UnityEditor: 
1. File-> Build Settings -> Player Settings -> XR Settings -> Virtual Reality Settings を有効にする
2. Window -> Holographic Emulation -> Emulation Mode を None から Remote to Device に変更する
3. Remote Machine に HoloLens に表示されているIPアドレスを入力する -> Connect ボタンを押す
4. UnityEditorを再生するとHoloLensに映像が表示されるはずです。

## よくある誤解と注意点
Holographic Remotingは確かに便利機能ですがあくまで実行しているのは，PC側でHoloLens上ではありません。エアタップといった入力イベントはシミュレートできますが，HoloLensのカメラは利用できない，つまりVuforiaといったHoloLensのカメラを使ったデバッグはできません。また，UWPのコードも同様に実行されるのはEditor環境なので実行されないということに注意してください。

---
