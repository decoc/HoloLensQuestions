# HoloLensQuestions
## アプリケーションの開発環境
### Unity
HoloLensの3Dアプリケーション開発環境は，[Unity](https://unity3d.com/jp)を使った開発がスタンダードです。Unity（別名:Unity3D）は、統合開発環境を内蔵し、複数の機材(platform)に対応するゲームエンジンです。3Dアプリケーションの多くがUnity製になります。

Unityのバージョンによって，HoloLensで利用できるAPIやサポートされる機能が変わってきます。
現在の推奨環境は，[2017.4.x LTS](https://unity3d.com/jp/unity/qa/lts-releases) です。2018-2020の2年間のサポートが約束されており，安定性を求める場合こちらのバージョンを使うことをお勧めします。  
最新のUnity2018でも開発は可能ですが，後述するMixed Reality Toolkit の相性問題もあるため最新の環境が最良とは断言できません。

### Mixed Reality Toolkit
[Mixed Reality Toolkit](https://github.com/Microsoft/MixedRealityToolkit-Unity)は，HoloLensのアプリケーション開発をサポートするコンポーネント群です。例えばHoloLens用のカメラオブジェクトや入力，UX周りをPrefabを用いて直ぐにシーンを構築できるだけではなく，UnityProjectの初期設定やアプリケーションのデプロイなどもサポートしてくれます。  

Mixed Reality Toolkitのバージョンはいくつかあり，先のGitHubリンクの[Release](https://github.com/Microsoft/MixedRealityToolkit-Unity/releases)からダウンロードすることができます。HoloToolkit-Unity-XXXX.X.XがMRTKの本体で，HoloToolkit-Unity-Examples-XXXX.X.Xでその使い方を学ぶことができます。

注意点として，Mixed Reality Toolkit はUnityの多くのバージョンをサポートしてくれてはいますが，最新の環境では動作しなかったり，Unity5といった古いバージョンでは一部動作しなかったりします。利用したい環境に合わせた適切なパッケージを選択することが重要になりますので，<b><u>Upgrade Guide をよく読みましょう。</u></b>

以下は最新の環境での例です。

- Unityのバージョン: 2017.4.x
- Unityのバージョン: 2017.2~ (HoloLensとImmersiveの両方で動作させたい場合)
- Unityの最低推奨バージョン: 2017.1 (5.6でも動作はする)
- Windows SDK: 10.0.17134 (Unity2017.2~の場合)
- Visual Studio 2017
- Fall Creators Update

#### MRTKの機能
MRTKは非常に多機能で数多くのコンポーネントがあります。

どんなことができるかの全体を把握するにはこちらのまとめがおすすめです。  
[Mixed Reality Toolkit (MRTK)の有用なビルディングブロック](https://medium.com/@dongyoonpark/mixed-reality-toolkit-mrtk-%E3%81%AE%E6%9C%89%E7%94%A8%E3%81%AA%E3%83%93%E3%83%AB%E3%83%87%E3%82%A3%E3%83%B3%E3%82%B0%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF-4eb343d474cb)

#### MRTKを使った簡単なアプリケーション作成までの手順
たぶんこれが一番シンプルで分かりやすいと思います。  
[HoloLens ホログラフィックアプリケーションを作ってみた（Cube初級編）](https://www.nttpc.co.jp/technology/holoLens.html)

### Trouble Shooting
#### 調べる
OSやUnityのバージョンによっては描画が安定しない，クラッシュするなど動作が不安定な場合があります。解決しない場合，既知の問題の可能性があります。[Issue Tracker](https://issuetracker.unity3d.com/) や [Release Note](https://unity3d.com/jp/unity/whats-new/unity-2018.2.0)が問題解決のヒントになります。

#### 聞いてみる
HoloLensのFBコミュニティグループで聞いてみると誰かしら反応があると思います。  
[HoloLens/Windows Mixed Reality ユーザーズグループ](https://www.facebook.com/groups/149608962203470/?ref=group_header)

他にもTwitterで活動しているHoloLensアプリデベロッパーが多くいるので，捕まえて聞いてみるのも手です。

## よくあるHoloLensの質問
### HoloLensで写真を撮る方法
1. 音声コマンドで: Hey cortana, take a picture
2. ボタンで:音量ボタン 上下同時押し
3. PCから: Device Portal  
4. スクリプトで: https://docs.microsoft.com/en-us/windows/mixed-reality/locatable-camera-in-unity

### GPSとりたい
ないです。

### 環境認識できるんでしょ？
壁と床、物があるくらいの判定はできますが人とかは判別できません。

### バンドが壊れた
問い合わせしましょう。Devエディションはそもそもサポート外なので望み薄だと思ったほうがいいです。

### 複数台あれば同じものが見えるんでしょ？
Sharing機能はOS側の機能ではなく，実装する必要があります。
