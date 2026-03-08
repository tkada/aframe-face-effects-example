# A-Frame: Face Effects
[English](README.md)

8th Wall Face Effects を使ってみましょう。この A-Frame サンプルでは、検出した顔に 3D メガネを装着し、生成されたフェイスメッシュにタトゥーを適用します。
[デモ](https://tkada.github.io/aframe-face-effects-example/)

![](https://media.giphy.com/media/PmjHEktbE2BVSkuHgN/giphy.gif)

## 使い方

1. このリポジトリで、Code → Download zip をクリックする
2.  zip を解凍し、作業したい場所にフォルダを置く
3. `npm install`
4. `npm run serve`
5. モバイル端末で接続するには [こちらの手順](https://8th.io/test-on-mobile) に従う
6. 推奨: 進捗を失わないよう [git](https://git-scm.com/about) でファイルを管理する

### GitHub Pages へのデプロイ

`main` ブランチへ push すると、GitHub Actions でビルドされ GitHub Pages に自動デプロイされます。

1. リポジトリの **Settings** → **Pages** を開く
2. **Build and deployment** の **Source** で **GitHub Actions** を選択する
3. `main` に push する（または Actions タブから「Deploy to GitHub Pages」ワークフローを手動実行）

デプロイ後は `https://<ユーザー名>.github.io/<リポジトリ名>/` で公開されます。

## 質問は？

[Github Discussions](https://github.com/orgs/8thwall/discussions) で質問するか、[Discord](https://8th.io/discord) でコミュニティに参加してください。

---

### Metaversal デプロイ向けの最適化

R18 では、全新 8th Wall Engine の Metaversal Deployment により、WebAR 体験を一度つくればスマートフォン、タブレット、PC、AR/VR ヘッドセットにデプロイできます。このプロジェクトにはいくつかのプラットフォーム別のカスタマイズがあります。

**body.html** では、```<a-scene>``` 内の ```xrweb``` コンポーネントに ```"allowedDevices: any"``` パラメータを追加しており、デスクトップを含むすべてのプラットフォームで開くようにしています。環境パラメータはオープンな砂漠空間を生成するようにカスタマイズされています。

---

### Face コンポーネントとプリミティブ

フェイスエフェクトには ```<a-scene>``` に ```xrface``` が必要です。

- cameraDirection: 使用するカメラ。'back' または 'front' を指定。セルフィーモードでは 'cameraDirection: front;' と 'mirroredDisplay: true;' を併用。（既定: back）
- allowedDevices: 対応デバイス種別。'mobile' または 'any' を指定。'any' で内蔵・外付け Web カメラ付きのノート PC やデスクトップも有効。（既定: mobile）
- mirroredDisplay: true の場合、出力ジオメトリの左右を反転し、カメラ映像の向きを反転。セルフィーモードでは 'mirroredDisplay: true' と 'cameraDirection: front;' を併用。（既定: false）
- meshGeometry: フェイスメッシュのどの部分の三角形インデックスを返すか。'face'、'eyes'、'mouth' の任意の組み合わせ。（既定: face）
- maxDetections: 同時に処理する顔の最大数。最大 3。（既定: 1）

```<xrextras-faceanchor>``` は検出した顔のトランスフォームを継承します。子エンティティは顔と一緒に動きます。```maxDetections``` > 1 のときは `face-id` プロパティで各顔のシーンを指定します。
- face-id: 子要素を対応する face id に紐づける。

```xrextras-hider-material``` は、背面のモデルの描画をブロックしつつ透明にしたいメッシュやプリミティブに適用します。シーンでは head-occluder.glb に適用されています。

```<xrextras-face-mesh>``` はシーン内にフェイスメッシュを生成します。

```<xrextras-face-attachment>``` は検出したアタッチメントポイントのトランスフォームを継承します。子エンティティは指定したアタッチメントポイントと一緒に動きます。

- point: アタッチメントポイント名（既定: forehead）

アタッチメントポイント: forehead, rightEyebrowInner, rightEyebrowMiddle, rightEyebrowOuter, leftEyebrowInner, leftEyebrowMiddle, leftEyebrowOuter, leftEar, rightEar, leftCheek, rightCheek, noseBridge, noseTip, leftEye, rightEye, leftEyeOuterCorner, rightEyeOuterCorner, upperLip, lowerLip, mouth, mouthRightCorner, mouthLeftCorner, chin

### リファレンスアセット

フェイスメッシュの UV リファレンス画像は ```/assets/Tattoos/``` から 'uv-black.png' または 'uv-bright.png' をダウンロードしてください。

3D モデリングソフト用のフェイスメッシュモデルは[こちら](https://cdn.8thwall.com/web/assets/face/facemesh.obj)からダウンロードできます。

### Media Recorder

このプロジェクトでは [XRExtras.MediaRecorder](https://www.8thwall.com/docs/web/#customize-video-recording)、[XR8.MediaRecorder](https://www.8thwall.com/docs/web/#xr8mediarecorder)、[XR8.CanvasScreenshot](https://www.8thwall.com/docs/web/#xr8canvasscreenshot) により動画録画と写真撮影が可能です。

```<xrextras-capture-button>```: シーンにキャプチャボタンを追加します。

- capture-mode: キャプチャモード。**standard**（既定）: タップで写真、長押しで動画、**fixed**: タップで動画録画のオン/オフ、**photo**: タップで写真。

```<xrextras-capture-config>```: キャプチャメディアの設定。

- max-duration-ms: キャプチャボタンで許可する動画の最大長（ミリ秒）。エンドカード無効時は最大録画時間に対応。既定 15000。
- max-dimension: 幅・高さの最大録画解像度。既定 1280。

- enable-end-card: 録画メディアにエンドカードを含めるか。既定 'true'。
- cover-image-url: エンドカードのカバー画像。既定はプロジェクトのカバー画像。
- end-card-call-to-action: エンドカードの CTA 文言。既定 "Try it at:"。
- short-link: エンドカードのショートリンク文言。既定はプロジェクトのショートリンク。
- footer-image-url: エンドカードのフッター画像。既定は 'Powered by 8th Wall'。

- watermark-image-url: ウォーターマーク画像。既定 null。
- watermark-max-width: ウォーターマーク画像の最大幅（%）。既定 20。
- watermark-max-height: ウォーターマーク画像の最大高さ（%）。既定 20。
- watermark-location: ウォーターマークの位置。**topLeft**、**topMiddle**、**topRight**、**bottomLeft**、**bottomMiddle**、**bottomRight**（既定）のいずれか。

- file-name-prefix: ファイル名のユニークタイムスタンプの前に付ける文字列。既定 'my-capture-'。

- request-mic: 初期化時にマイクを設定するか（"auto"）、実行時に設定するか（"manual"）。
- include-scene-audio: true の場合、シーン内の A-Frame サウンドがレコーダー出力に含まれる。

```<xrextras-capture-preview>```: 再生・ダウンロード・共有ができるメディアプレビューのプレハブをシーンに追加します。

- action-button-share-text: Web Share API 2 対応時（iOS 14、Android）のアクションボタン文言。既定 "Share"。
- action-button-view-text: iOS で Web Share API 2 非対応時（iOS 13）のアクションボタン文言。既定 "View"。

XRExtras.MediaRecorder API の詳細は[こちら](https://www.8thwall.com/docs/web/#customize-video-recording)。

#### クレジット

ステレオメガネ: [jau0gan](https://www.turbosquid.com/3d-models/free-stereo-3d-model/613193)

---
