# Mixed Reality Toolkit
[Mixed Reality Toolkit](https://github.com/Microsoft/MixedRealityToolkit-Unity)は，HoloLensのアプリケーション開発をサポートするコンポーネント群です。MRTKを使うことでコーディングが苦手でも開発が楽にすむ部分が多いです。例えばHoloLens用のカメラオブジェクトや入力，UX周りをPrefabを用いて直ぐにシーンを構築できるだけではなく，UnityProjectの初期設定やアプリケーションのデプロイなどもサポートしてくれます。  
Mixed Reality Toolkitのバージョンはいくつかあり，先のGitHubリンクの[Release](https://github.com/Microsoft/MixedRealityToolkit-Unity/releases)からダウンロードすることができます。HoloToolkit-Unity-XXXX.X.XがMRTKの本体で，HoloToolkit-Unity-Examples-XXXX.X.Xでその使い方を学ぶことができます。
注意点として，Mixed Reality Toolkit はUnityの多くのバージョンをサポートしてくれてはいますが，最新の環境では動作しなかったり，Unity5といった古いバージョンでは一部動作しなかったりします。利用したい環境に合わせた適切なパッケージを選択することが重要になりますので，<b><u>Release Note, Upgrade Guide をよく読みましょう。</u></b>

以下はMRTK 2017.4.0.0の例です。

- Unityのバージョン: 2017.4.x
- HoloLensでもImmersiveでも両方で動作させたい場合のUnityのバージョン: 2017.2~
- Unityの最低推奨バージョン: 2017.1 (5.6でも動作はする)
- Windows SDK: 10.0.17134 (Unity2017.2~の場合)
- Visual Studio 2017
- Fall Creators Update

## 概要把握
MRTKを使ってできることの概要。  
[Mixed Reality Toolkit (MRTK)の有用なビルディングブロック](https://medium.com/@dongyoonpark/mixed-reality-toolkit-mrtk-%E3%81%AE%E6%9C%89%E7%94%A8%E3%81%AA%E3%83%93%E3%83%AB%E3%83%87%E3%82%A3%E3%83%B3%E3%82%B0%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF-4eb343d474cb)

## 機能一覧
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

## MRTKを使ったアプリケーション作成までの手順

ここが一番シンプルで分かりやすいと思います。  
[HoloLens ホログラフィックアプリケーションを作ってみた（Cube初級編）](https://www.nttpc.co.jp/technology/holoLens.html)

一連の開発プロセスを学ぶにはこちらのスライドがおすすめです。  
[UnityによるHoloLensアプリケーション入門](https://www.slideshare.net/YuichiIshii/unityhololens-92864583)

# 使い方
## 設定
HoloLensアプリ開発に必要な設定を行ってくれるEditor拡張があります。Editorの上部に「Mixed Reality Toolkit」が追加されており，「Configure」から各種設定を行います。

### Apply Mixed Reality Project Setting
開発環境で述べたプロジェクトに必要な設定を行います。  

|項目|内容|デフォルト|
|:---|:---|:---:|
|Target Windows Universal UWP|プラットフォームをUWPに切り替えます|ON|
|Enable XR|XR supportedを有効にします|ON|
|Build for Direct3D|BuildTypeをD3Dにします|ON|
|Target Occulded Device|IHMD向けにクオリティ設定を変更する|OFF|
|Enable Sharing Service|Sharingに必要なコンポーネントをインポートする|OFF|
|Use Tookit-specific InputManager axis|Xboxコントローラを使うためのInput設定を行う|OFF|
|Set Default SplatialMapping Layer|LayerにSpatialMappingを追加する|ON|

### Apply Mixed Reality Scene Setting
アプリに必要なCameraやカーソルなどのコンポーネントをシーンに生成します。  

|項目|内容|デフォルト|
|:---|:---|:---:|
|Add the MixedRealityCamera prefab|MixedRealityCameraをシーンに追加する|ON|
|Move Camera to Origin|カメラを(0,0,0)に移動する|ON|
|Add the InputManager prefab|InputManagerを生成する|ON|
|Add the Default Cursor prefab|DefaultCursorを生成する|ON|
|Update World Canvas prefab|シーン内のWorldSpaceに設定されたCanvasがカメラにFocusManagerのUIRaycastCameraを利用するように変更|ON|

### Apply UWP Capability Setting
マイクやカメラなどの利用権限の設定を行います。  

|項目|内容|デフォルト|
|:---|:---|:---:|
|Microphone|DictationRacognizerなどに利用するマイクを有効化|OFF|
|WebCam|PhotoCaptureなどに利用するRGBカメラを有効化|OFF|
|Spatial Perception|SpatialMappingなどに利用する空間認識を有効化|ON|
|Internet Client|サーバーではなくクライアントとしてパブリックネットワークと通信するために有効化|OFF|
|Internet Client Server|クライアントだけではなくサーバーとしても機能させる場合に有効化|OFF|
|Private Network Client Server|プライベートネットワークでクライアントとして通信するために有効化|OFF|

## 基本コンポーネント
基本的に3つのコンポーネントが必要です。

Camera

|項目|内容|
|:---|:---|
|HoloLensCamera|HoloLens用に設定されたカメラです。|
|MixedRealityCamera|HoloLens兼IHMD用に設定されたカメラです。実行するデバイスに応じて自動で切り替えます。|
|MixedRealityCameraParent|MixedRealityCameraを拡張したカメラです。IHMD用のテレポート移動が使えます。|

Cursor

|項目|内容|
|:---|:---|
|BaseCursor||
|Cursor||
|CursorWithFeedback||
|DefaultCursor||
|InteractiveMeshCursor||

InputManager

|項目|内容|
|:---|:---|
|InputManager||

## その他のコンポーネント
SpatialMapping

|項目|内容|
|:---|:---|
|SpatialMapping||
