# How-to : Example macros

PTSIM付属のマクロファイルを紹介します。

## 実行方法
以下、PTSIMの実行ディレクトリ(例: ~/PTSproject-install/PTSapps/DynamicPort)で作業します。

## 例題マクロファイル
PTSIMコードに付属するマクロファイルをコピーして用います。例えば、マクロファイルが、`Sample0.mac`である場合は、以下のようにマクロファイルを実行ディレクトリにコピーして実行します。
```
$ cp  ./macros/DynamicPort/Sample0.mac  .
```

実行
```
$ ./bin/PTSdemo  -m  Sample0.mac
Or
$ ./bin/PTSdemo  -i  Sample0.mac
```

*補足*
1. 例題によっては、設定用の他のファイルが必要となる場合があります。
2. 例題マクロファイル内で、別のマクロファイルを呼び出す場合があります。

---

## Beam modeling
#### Spot Scan beam 
 - [SampleScan.mac](./HowTo/SampleScan.md)<br>
 スポットスキャニングの例題マクロファイルです。
 
#### Line Scan beam
 - [SampleLineScan.mac](./HowTo/SampleLineScan.md)<br>
 ラスターキャニングの例題マクロファイルです。

---

## Beam line and patient modeling

#### Beam-module geometry
 - [Sample0.mac](./HowTo/Sample0.md)<br>
  Passive-Wobblingのビームラインの例題マクロファイルです。

 - [Sample1.mac](./HowTo/Sample1.md)<br>
  Passive-Wobblingのビームラインの例題マクロファイルですが、最初は治療室(G4Room)のみ実体化されています。

 - [SampleWobbler.mac](./HowTo/SampleWobbler.md)<br>
  Wobbler-Magnetの例題マクロファイルです。

<!--
 - [SampleRotating.mac](./HowTo/SampleRotating.md)<br>
  回転モジュレータの例題マクロファイルです。
-->

#### GDML Beam-module geometry
 - [SampleGDMLr.mac](./HowTo/SampleGDMLr.md)<br>
 GDMLファイルのジオメトリを構築する例題マクロファイルです。

 - [SampleGDMLw.mac](./HowTo/SampleGDMLw.md)<br>
 実体化したビームモジュールのオメトリをGDMLファイルに出力する例題マクロファイルです。 

<!--
 - [SampleGDMLwobbling.mac](./HowTo/SampleGDMLwobbling.md)<br>
 GDMLファイルのワブラー電磁石を利用する例題マクロファイルです。  
-->


<!--
#### VTK Legacy format(Voxel geometry)
 - [SampleGeomScan.mac](./HowTo/SampleGeomScan.md)<br>
 ジオメトリをVTK Legacy形式のボクセルデータで出力する例題マクロファイルです。

 - [SampleScoreVTK.mac](./HowTo/SampleScoreVTK.md)<br>
-->

#### DICOM-CT
 - [Sample2.mac](./HowTo/Sample2.md)<br>
 DICOM-CTジオメトリの例題マクロファイルです。DICOM-CTデータが必要です。<br>
 [ExampleA5](../Tutorial/exampleA1.md)の解説を参照してください。

<!--
#### DICOM-CT w/ RTS
 - [Sample4.mac](./HowTo/Sample4.md)<br>
  DICOM-RTS

 - [SampleRoIDICOM.mac](./HowTo/SampleRoIDICOM.md)<br>
DICOM, RTS, RoI-material assignment

 - [SampleRoIRepl.mac](./HowTo/SampleRoIRepl.md)<br>
DICOM, RTS, RoI-CTvalue replacement

#### DICOM-CT and RTDose
 - [SampleRTDose.mac](./HowTo/SampleRTDose.md)<br>
 SampleRTDose.mac: GridInfo for RTDose
-->

#### ICRP145 mesh phantom
 - [SampleICRP145.mac](./HowTo/SampleICRP145.md)<br> 
 ICRP145メッシュファントムジオメトリの例題マクロファイルです。ICRP145dataが必要です。
 
-----
## Parallel World
#### Field in Paralell World
 - [SamplePWField.mac](./HowTo/SamplePWField.md)<br>
  水ファントムをパラレルワールドに配置して、その領域に磁場を印加する例題マクロファイルです。[exampleA11](../Tutorial/exampleA11.md)と基本的に同じです。

#### Layered Mass Geometry
 - [SamplePWLayer.mac](./HowTo/SamplePWLayer.md)<br> 
 水ファントムをマスワールドに配置して、パラレルワールドに物体を配置して水ファントムに物体を埋め込む例題マクロです。基本的に[exampleA7](../Tutorial/exampleA7.md)と同じです。
 
#### Scoring in Parallel World
 - [SamplePWScore.mac](./HowTo/SamplePWScore.md)<br>
 - [SamplePWScore2.mac](./HowTo/SamplePWScore2.md)<br>
 - [SamplePWScore3.mac](./HowTo/SamplePWScore3.md)<br>   

---

## Flexible Scoring
#### SD Attachment
 - [SampleAttachSD.mac](./HowTo/SampleAttachSD.md)<br>
 基本的に[exampleA10](../Tutorial/exampleA10.md)と同じです。

 - [SampleMultiDet.mac](./HowTo/SampleMultiDet.md)<br>
 複数スコアボリュームの例題マクロファイル
 
 - [SampleMultiDet2.mac](./HowTo/SampleMultiDet2.md)
 複数スコアボリュームの例題マクロファイル 

#### TriggerSD
 - [SampleTriggerSD.mac](./HowTo/SampleTriggerSD.md)<br>
 基本的に[exampleA10](../Tutorial/exampleA9.md)と同じです。 

#### RI bit
 - [SampleRIbit.mac](./HowTo/SampleRIbit.md)<br>
 RIをTrackInformationで記録します。
 
---

## PHSP beam
#### IAEA Phsp data
 - [Sample6w.mac](./HowTo/Sample6w.md)<br>
 IAEAphspデータ出力の例題マクロファイルです。

 - [Sample6r.mac](./HowTo/Sample6r.md)<br>
  Sample6w.macのIAEAphsp出力データを読み込む例題マクロファイルです。

 - [SampleIAEArw.mac](./HowTo/SampleIAEArw.md)<br>
  IAEAphspを読み込みとIAEAphsp出力を繰り返す例題マクロファイルです。

#### In house Phsp data
 - [SampleEvtDumper.mac](./HowTo/SampleEvtDumper.md)<br>
  独自フォーマットのPHSPファイル出力の例題マクロファイルです。

 - [SampleEvtDumper2.mac](./HowTo/SampleEvtDumper2.md)<br>
  独自フォーマットのPHSPファイル出力の例題マクロファイルです。
  
 - [SampleEvtFile.mac](./HowTo/SampleEvtFile.md)<br>
  独自フォーマットのPHSPファイルを読み込む例題マクロファイルです。

---