# Spectator View
　Spectator Viewとは，HoloLensを利用している人の様子を低遅延・高画質で共有する仕組みです。Mixed Reality Capture ではHoloLensの映像そのもののを伝送しているので，ネットワーク環境によっては数秒，あるいは数十秒の遅延が上，画質も安定しません。ですが，IPアドレスとログインID，Passwordさえ分かっていれば使える機能なので実装は特にいりません。一方のSpectator View はHoloLensの映像そのものを送っている訳ではなく，オブジェクトの位置や姿勢，イベントなどを共有しているだけであり，仕組みとしてはSharingと同じです。実行環境もHoloLensからはあくまで位置情報のみで，Editor上でCGと一眼レフカメラなどから取得した映像を合成していす。ですので，実装に漏れがあるとオブジェクトが双方で共有されませんが，その分映像としては高画質高品質になりますので，MS公式のプロモーションビデオなどでも頻繁に使われているキャプチャ方式です。(PV詐欺だとか言われる原因でもあるんですが…) 以下にSpectatorViewの特徴とMRCとの違いを表にまとめました。

## SpectatorViewの特徴
- カメラ映像はデジタル一眼レフカメラなど高性能なカメラを使用する
- 表示されるホログラム(オブジェクト)はPCで描画する
- Sharingアプリのみ対象となる(スタートメニューなどの表示はできない)
- HoloLensの位置情報のみ実機から取り込む

【引用】
[HoloLens Spectator Viewを試してみた](https://www.naturalsoftware.jp/entry/2017/02/25/184758)

## MixedRealityCaptureとSpectatorViewの違い

||手軽さ|遅延|画質|実装|必要デバイス(コスト)|
|:---:|:---:|:---:|:---:|:---:|:---|
|MixedRealityCapture|簡単|大きい|荒い|不必要|HoloLens 1台，映像の出力先(PCやスマホなど)|
|SpectatorView|手間|小さい|きれい|必要|HoloLens 2台，外付けGPU，対応カメラ|

## SpectatorView for iOS
　また，SpectatorViewのカメラをiOSデバイスで代用するコンポーネントも公開されています。SpectatorViewといっても先で言ったように仕組みは単なるSharingです。詳細は[こちら](https://blog.nextscape.net/archives/Date/2018/07/hololens-spectatorview)。
