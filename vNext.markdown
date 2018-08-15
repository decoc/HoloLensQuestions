# Mixed Reality Toolkit vNext

vNextは旧Mixed Reality Toolkit に代わる新しいツールキットです。これまで，MRTK
が対応するデバイスはHoloLensとIHMDだけでした。vNextからはこれらに加え，OpenVRやSteamVRにも対応します。vNextではUnity2017をサポートせず，対象は2018以上になります。  

【参考】
- [Mixed Reality Toolkit - Unityの次期バージョンvNextのコンセプト](https://qiita.com/miyaura/items/b702cfe19163e9dac067)
- [Mixed Reality Toolkit - vNEXTの構造を理解する ～ Unity Etidorとプロジェクト構成編 ～](https://qiita.com/miyaura/items/887348ddc3933b72ff05)

## 使い方
### 設定
Editor上部の「MixedRealityToolkit」の「Configure」をクリックすると，Scene上に<b>MixedRealityManager</b>が生成されます。新しくなったMRTKでは，CameraやCursorなどはMixedRealityManagerに設定されたProfileに応じて作成されます。Defaultでは，DefaultMixedRealityConfigurationProfileを使用します。
