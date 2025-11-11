# Example A10(attachSD）

これまでの例題紹介では、水ファントム(G4MWaterPhantomクラス）やDICOMジオメトリ(G4MNestedDICOMクラス）を使用したスコアリングを取り上げてきました。これら２つのビームモジュールには、スコアリング機能がはじめから装備されています。  
PTSIMでは、スコアリング機能を装備していないビームモジュールにもスコアリング機能をコマンドで装備することが可能です。

ビームモジュールにスコア機能を装備する
 - 論理ボリュームを選択する
 - SensitiveDetector(SD)を作成する
 - SDを論理ボリュームに取付ける
 - スコアリング

以下、PTSIMの実行ディレクトリ(例: ~/PTSproject-install/PTSapps/DynamicPort)で作業します。

## 例題マクロファイル
PTSIMコードに付属するマクロファイル`exampleA10.mac`をコピーして用います。
```
$ cp  ./macros/tut/exampleA10.mac  .
```

実行
```
$ ./bin/PTSdemo  -i  exampleA10.mac
```

![exampleA101](../images/exampleA101.png)

陽子線を照射
```
Session: /run/beamOn 100
```

![exampleA102](../images/exampleA102.png)


終了
```
Session: exit
```

### マクロファイルの解説
解説するコマンド部分のみを抜粋して説明します。
マクロファイルでは、治療室に配置された散乱体(Scatter)をスコアリングボリュームにする手順を例に説明します。


```{code-block}
:linenos:
#
# (PreInit State)
#
# Material
/control/execute ./macros/common/materials.mac
#
# PhysicsList
/control/execute ./macros/common/phys.mac
#
# System and module registration
/G4M/System DynamicPort
/Dynamic/Module/Room/register 525.  525.  3550. mm
#
/Dynamic/Module/gdml/schema  ./data/schema/gdml.xsd
/Dynamic/Module/register Scatter G4MBMGdml ./data/Sample/G4MBMGdml/Scatter.gdml 0. 0. 2700. mm
#
# Run Initialize
/run/initialize
#
# (PreInit State)
#
# Primary particle
/My/PrimaryGenerator/select GPS
/control/execute ./macros/common/gps.mac
#
# Beam-module installation
/G4M/Module/install Scatter
#
# Create SD
/G4M/Module/createSD   ScatterSD HitsCollection
#
# Scoring
/My/runaction/dumpfile      A10.root
/My/runaction/ntuple/merge  true
#
/My/runaction/ntuple/create SCT ScatterSD/HitsCollection
/My/runaction/ntuple/addColumn SCT evno I
/My/runaction/ntuple/addColumn SCT pid  I
/My/runaction/ntuple/addColumn SCT de   F keV
/My/runaction/ntuple/addColumn SCT x    F mm
/My/runaction/ntuple/addColumn SCT y    F mm
#
#/run/beamOn 10
#
```

#### SDを作成
SDの固有名とヒットコレクション名を指定して、SDを作成します。
```
Idle> /G4M/Module/createSD   ScatterSD HitsCollection
```

#### 論理ボリュームの選択
スコアリング機能を有効にする論理ボリュームを選択し、SDを取付ます。
はじめに論理ボリュームを選択します。モジュール内で論理ボリューム名が重複していない場合、`/G4M/Module/selectLVByName`コマンドでモジュール名と論理ボリューム名を指定することで特定の論理ボリュームを選択します。
```
Idle> /G4M/Module/dumpLVByName   Scatter  ScatterL1
```

#### SDを選択した論理ボリュームに取付ける
```
Idle> /G4M/Module/attachSD   ScatterSD
```

#### スコアリング
スコアリングとしてNtuple型で保存する例を示します。
```
/My/runaction/ntuple/create SCT ScatterSD/HitsCollection
/My/runaction/ntuple/addColumn SCT evno I
/My/runaction/ntuple/addColumn SCT pid  I
/My/runaction/ntuple/addColumn SCT de   F keV
/My/runaction/ntuple/addColumn SCT x    F mm
/My/runaction/ntuple/addColumn SCT y    F mm
```

#### スコアリング結果例
```
$ root A10.root
root[] .ls
root[] SCT->Print()
root[] SCT->Draw("x:y","de","colz")
root[] .q
```

#### Tips: SDの設定について
`/G4M/Module/createSD`で作成されるSDは、`G4MDetectorSD`クラスが用いられています。こちらは水ファントムやDICOMモジュールの組み込み型SDと異なり、コマンドを用いて設定を変更できるようになっています。詳細は、[コマンドリファレンス](../cmd-reference.md#sensitivedetector-detectorsd)を参照してください。
