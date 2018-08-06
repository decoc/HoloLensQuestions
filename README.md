# HoloLensQuestions
## アプリケーションの開発環境
### Unity
Unity（別名:Unity3D）は、統合開発環境を内蔵し、複数の機材(platform)に対応するゲームエンジンです。
Unityのバージョンによって利用できるAPIやサポートされる機能が変わってきます。
現在の推奨環境は，[2017.4.x LTS](https://unity3d.com/jp/unity/qa/lts-releases)です。2018-2020の2年間のサポートが約束されており，安定性を求める場合こちらのバージョンを使うことをお勧めします。

### Mixed Reality Toolkit
[Mixed Reality Toolkit](https://github.com/Microsoft/MixedRealityToolkit-Unity)は，HoloLensのアプリケーション開発をサポートするコンポーネント群です。

### Trouble Shooting
OSやUnityのバージョンによっては描画が安定しない，クラッシュするなど動作が不安定な場合があります。解決しない場合，既知の問題の可能性があります。[Issue Tracker](https://issuetracker.unity3d.com/) や [Release Note](https://unity3d.com/jp/unity/whats-new/unity-2018.2.0)が問題解決のヒントになります。

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
