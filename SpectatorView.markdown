# Spectator View
Spectator Viewとは，HoloLensを利用している人の様子を低遅延・高画質で共有する仕組みです。Mixed Reality Capture ではHoloLensの映像そのもののを伝送しており，ネットワーク環境によっては数秒，あるいは数十秒の遅延があり，画質も安定しません。一方のSpectator View はHoloLensの映像そのものを送っている訳ではなく，オブジェクトの位置や姿勢，イベントなどを共有しているだけであり，仕組みとしてはSharingです。ですので，実装に漏れがあるとオブジェクトが双方で共有されません。以下に違いを表にまとめました。

MixedRealityCaptureとSpectatorView
||手軽さ|遅延|画質|実装|費用|
|:---:|:---:|:---:|:---:|:---:|:---|
|MixedRealityCapture|簡単|大きい|荒い|不必要|HoloLensが1台と出力先があればいいので費用はかからない|
|SpectatorView|手間|小さい|きれい|必要|外付けGPU，対応カメラが別途必要で費用はかかる|
