# Vuforia  
　[Vuforia](https://unity3d.com/jp/partners/vuforia)とはQualcomm社が提供するARを作成するためのライブラリで，登録したマーカーをカメラを使って認識することができます。マーカーは平面だけではなく，
立体マーカー，3Dモデル，さらにクラウド認識にも対応しており，最近はUnityの一機能として組み込まれました。  

## Vuforiaと基準点
 VuforiaはHoloLensにも対応しており，認識したマーカーの位置に3Dオブジェクトを出すだけではなく，マーカーを基準点として扱うという使い方もよくされます。
 例えば，HoloLensの業務的な使い方では「壁を見ると配管や鉄骨が透けて見える」といった利用方法が考えられます。しかしながら，HoloLensは自分が現実でいる位置をUnityで作った部屋の位置に自動で対応させることはできません。そのままでは鉄骨の位置がずれている，酷いと部屋の外に鉄骨が見えるなど本来意図していない動作になるでしょう。あくまでHoloLensの原点はHoloLensアプリを起動した位置が原点（0,0,0）になります。しばしば空間認識能力というワードがこの誤解を招きます。この問題に対処するにはHoloLensアプリを起動する位置を工夫するというアナログな対処法になりますが，何度も立ち上げ直すのは正直やりたくない手法でしょう。ここでVuforiaが登場です。Vuforiaであれば何度もアプリを起動し直さなくとも認識したマーカーのUnity座標と原点座標の差分で各オブジェクトの位置を補正するだけなので位置合わせが非常に簡単に済みます。  
 またVuforiaはSharingにも利用できます。HoloLensは起動した位置が原点となるという話がありますが，つまり複数台のHoloLensでは皆それぞれ原点位置が変わってくるということです。そのままでは同じ位置に同じものを表示することは実現できません。一応SpatialMappingを使った位置合わせする機能がUnityによって提供されていますが，各々のHoloLensで持っているSpatialMapの情報は大きく異なる場合があり，位置合わせに多々失敗します。ですが，Vuforiaであれば認識した位置を共通の基準点としてそれぞれ補正をかけるだけで済むのです。また，この手法のいいところはスマホにも同じ手法が適応できるので，HoloLensだけではなく，スマホとも見ているものの位置が共有できます。  
 
 【参考】  
-  [お手軽にSharingの接続成功率と精度をあげる](http://blog.d-yama7.com/archives/569)
-  [Vuforiaを使ってSharingの接続成功率をあげる](http://blog.d-yama7.com/archives/600)

## Vuforiaの使い方
 Vuforiaを使うことで非常に多くの恩恵を受けられますが，一方設定項目が多いので使い方が分からないという話をよく聞きます。  
 UnityAssetをそのまま使う場合，Vuforiaのアカウント登録は必要ありませんが，任意のマーカーを使う場合には，Vuforiaのサイトへの登録，データベースの作成，ライセンスキーの取得が必要になります。  
 
 ### Vuforia Website: 
 1. アカウントが必要です。Vuforia Developer Portal から[登録](https://developer.vuforia.com/vui/auth/register) しましょう。
 2. DevelpタブからTargetManagerタブを選択し，Add Databaseを押してデータベースを追加します。
 3. 次に，Add Targetで認識させたいマーカーを登録します。Widthはメートル単位です。
 4. 登録ができたら，Download Database を押してUnityPackageをDLします。
 5. 最後にDevelpタブ->LicenseManagerタブから先ほど作成したデータベースを開き，ライセンスキーを控えておきます。
 
 【参考】
 [UnityとVuforiaでARアプリの作成](https://qiita.com/murasan/items/b31d9deb6d8b24bd9134)
 
### UnityEditor: 
 1. Unityを新規にインストール際は，Vuforia Augmented Reality SupportをOnにしてインストールしてください。既存のUnityに追加する場合，UnityHubを使っているならば，Installsから追加したいUnityのバージョンからAddComponentで導入できます。そうでない場合は，Build Settings / Player Settings /XR Settings に移動して，VuforiaをDownLoadし，Unityを開きなおしてください。
 2. GameObject->Vuforia->ARCameraを追加します。この時，Vuforiaのインポートダイアログが出るので許可してください。すると，VuforiaのAssetがプロジェクトにインポートされます。
 3. ARCameraのInspectorのVuforiaBehaviourのOpen Vuforia configuration をクリックします。
 4. App License Key に先ほど控えたライセンスキーを入れます。
 5. Digital EyewearのDeviceTypeをHandheldからDigitalEyewearに変更します。加えてDeviceConfigをVuforiaからHoloLensへ変更します。
 6. 次に登録したデータベースが使えるようにします。DLしたUnitypackageをインポートします。
 7. Vuforia configurationのDatabasesに登録したDatabaseが追加されるのでチェックを入れて有効にします。
 8. AR Camera がVuforiaマーカーを認識できるようにSceneにVuforia->Imageを追加します。
 9. InspectorのImage Target Behaviour のDatabaseに先ほどのDB名を指定します。
 10. 最後にBuild Settings / Player Settings / Publish Settings の Capabiliteisから，「Internet Client」「WebCam」「Microphone」を有効にします。
 
 これでHoloLens側から任意のマーカーを認識できるようになります。

## Vuforiaを使った位置合わせ
VuforiaのTracking状態に応じた処理は，先のImageTargetにアタッチされているDefault Trackable Event Handler が 参考になります。　　
ITrackableEventHandlerを継承し，OnTrackableStateChangedを実装した本クラスを通じてTrackingの状態を取得できます。例えば，Default Trackable Event Handlerでは，マーカーを認識したタイミングでImageTarget配下のオブジェクトを描画し，ロストしたタイミングで描画をオフにしています。  

ですので，例えばマーカーを認識したタイミングでマーカーの位置とクォータニオンをイベントで送信し基準点として各オブジェクトの位置を補正し，使わなくなったARCameraをオフにするといった処理もできます。Vuforiaカメラは立ち上げたままだとアプリケーションのパフォーマンスへ影響するため，必要に応じてオンオフする方が好ましいです。

