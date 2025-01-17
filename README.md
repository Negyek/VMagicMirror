[English Readme](https://github.com/malaybaku/VMagicMirror/blob/master/README_en.md)

![Logo](./docs/images/vmagicmirror_logo.png)

Logo: by [@otama_jacksy](https://twitter.com/otama_jacksy)

v1.8.1

* 作成: 獏星(ばくすたー)
* 2021/07/31

WindowsでVRMを表示し、追加のデバイスなしで動かせるアプリケーションです。

1. できること
2. ダウンロード
3. 質問など
4. (開発者向け)ビルド手順
5. OSS等のライセンス
6. ローカリゼーションについて


## 1. できること

* VRMを読み込み、キャラクターの上半身を表示します。
* キーボードとマウス操作をモーションとして反映します。
* 可変のクロマキーが適用できます。

キーボードとマウス操作のみでキャラクターが動く特徴から、以下のシチュエーションで活躍します。

* 機材の準備が面倒な時の配信
* ライブコーディング中の賑やかし
* デスクトップマスコット

## 2. ダウンロード

[Booth](https://booth.pm/ja/items/1272298)から取得可能です。

Windows 10環境でお使いいただけます。

操作方法については[マニュアル](https://malaybaku.github.io/VMagicMirror/)をご覧下さい。

## 3. 質問など

* [Twitter](https://twitter.com/baku_dreameater)
* [Blog](https://www.baku-dreameater.net/)


## 4. (開発者向け)ビルド手順

### 4.1. フォルダ配置

適当なフォルダ以下に、次の構成で配置します。

+ `Bin`
    + (空のディレクトリ)
+ `Unity`
    + このレポジトリ
+ `WPF`
    + [WPFのレポジトリ](https://github.com/malaybaku/VMAgicMirrorConfig)

Unity 2019.4系でUnityプロジェクトを開き、Visual Studio 2019でWPFプロジェクトを開きます。

メンテナの開発環境は以下の通りです。

* Unity 2020.3.8f1 Personal
* Visual Studio Community 2019 16.10.3
    * .NET Core 3.1 SDKがインストール済みであること
    * Visual Studioのコンポーネントで「C++によるデスクトップ開発」をインストール済みであること
        - UnityのBurstコンパイラ向けに必要なセットアップです。


### 4.2. アセットの導入

* [FinalIK](https://assetstore.unity.com/packages/tools/animation/final-ik-14290)
* [Dlib FaceLandmark Detector](https://assetstore.unity.com/packages/tools/integration/dlib-facelandmark-detector-64314)
* [OpenCV for Unity](https://assetstore.unity.com/packages/tools/integration/opencv-for-unity-21088)
* [OVRLipSync v1.28.0](https://developer.oculus.com/downloads/package/oculus-lipsync-unity/1.28.0/)
* [VRMLoaderUI](https://github.com/m2wasabi/VRMLoaderUI/releases) v0.3
* [Zenject](https://github.com/svermeulen/Extenject) (アセットストアから)
* SharpDX.DirectInput 4.2.0
    * [SharpDX](https://www.nuget.org/packages/SharpDX)
    * [SharpDX.DirectInput](https://www.nuget.org/packages/SharpDX.DirectInput/)
* [RawInput.Sharp](https://www.nuget.org/packages/RawInput.Sharp/) 0.0.3
* DOTween (アセットストアから)
* [Fly,Baby. ver1.2](https://nanakorobi-hi.booth.pm/items/1629266)
* [LaserLightShader](https://noriben.booth.pm/items/2141514)
* [VMagicMirror_MotionExporter](https://github.com/malaybaku/VMagicMirror_MotionExporter)

FinalIK, Dlib FaceLandmark Detector, OpenCV for Unityの3つは有償アセットであることに注意してください。

"Fly,Baby." および "LaserLightShader"はBOOTHで販売されているアセットで、ビルドに必須ではありませんが、もし導入しない場合、タイピング演出が一部動かなくなります。

Dlib FaceLandmark Detectorについては、アセットに含まれるデータセットを`StreamingAssets`フォルダ以下に移動します。導入にあたっては、Dlib FaceLandmark Detector本体のサンプルプロジェクト(`WebCamTextureExample`)を動かすなどして、ファイルが正しく置けているか確認します。

SharpDXは次の手順で導入します。

- 2つのNuGetギャラリーの`Download package`から`.nupkg`ファイルを取得し、それぞれ`.zip`ファイルとして展開します。
- 展開したzip内の`lib/netstandard1.3/`フォルダにそれぞれ`SharpDX.dll`および`SharpDX.DirectInput.dll`があるので、これらをUnityプロジェクト上の適当な場所に追加します。

RawInput.Sharpもほぼ同様の導入手順です。

- NuGetギャラリーから取得した`.nupkg`を展開し、中の`lib/netstandard1.1/RawInput.Sharp.dll`を取得します。
- 取得したDLLを、Unityプロジェクト上でAssets以下に`RawInputSharp`というフォルダを作り、その下に追加します。

OpenCVforUnityについては導入後、`DisposableOpenCVObject.cs`を次のように書き換えます。

```
    abstract public class DisposableOpenCVObject : DisposableObject
    {

//        internal IntPtr nativeObj;
        //Change to public member
        public IntPtr nativeObj;

```

以上のほか、手作業での導入は不要ですが、Unity Package Managerで下記を参照しています。

* [UniVRM](https://github.com/vrm-c/UniVRM) v0.66.0
* [UniRx](https://github.com/neuecc/UniRx)
* [MidiJack](https://github.com/malaybaku/MidiJack)
    * オリジナルのMidiJackではなく、Forkレポジトリです。


### 4.3. ビルド

* Unityでのビルド時には`Bin`フォルダを指定します。
* WPFでのビルドでは、`VMagicMirrorConfig`プロジェクトを右クリックし、`発行`を指定してフォルダ上にアプリケーションを配置します。
    - プロファイル設定は次のようにします。
        - 構成: `Release | x86`
        - ターゲットフレームワーク: `netcoreapp3.0`
        - 配置モード: `自己完結`
        - ターゲットランタイム: `win10-x86`
        - ターゲットの場所: PC上の適当なフォルダ
    - 上記の設定で発行すると、ターゲットのフォルダ上に`VMagicMirror.exe`を含むファイル群が出力されます。これらのファイルを`Bin/ConfigApp/`以下にコピーします。

フォルダ構成については配布されているVMagicMirrorも参考にしてください。

## 5. OSS等のライセンス

### 5.1. OSSライセンス

設定UIであるWPFプロジェクト内でOSSライセンスを掲載しています。

[https://github.com/malaybaku/VMagicMirrorConfig](https://github.com/malaybaku/VMagicMirrorConfig)

プレーンテキストとしては下記を参照下さい。

https://github.com/malaybaku/VMagicMirrorConfig/blob/master/VMagicMirrorConfig/VMagicMirrorConfig/Resources/LicenseTextResource.xaml


### 5.2. SketchFabから取得しているモデルについて

このレポジトリに含まれる`xbox_controller.fbx`はSketchFabにてAttribution 4.0 International (CC BY 4.0)で公開されているものを用いています。

作成者: Criegrrunov
モデル取得元URL: https://sketchfab.com/3d-models/xbox-controller-fb71f28a6eab4a2785cf68ff87c4c1fc

VMagicMirrorでは元モデルに対し、マテリアルを差し替えたものを用いています。これは他のキーボード、タッチパッド等と見た目の一貫性をはかるためです。

## 6. ローカリゼーションについて

日本語、英語以外のローカリゼーションでのContributionに興味がある場合、[about_localization.md](./about_localization.md)を参照して下さい。

