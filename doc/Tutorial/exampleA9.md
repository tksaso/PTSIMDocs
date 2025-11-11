# Example A9(TriggerSD）

TriggerSDを論理ボリュームに取り付けることにより、粒子がその論理ボリュームを通過した履歴をビット列で記録します。記録したビットは、スコアリングによりNtupleの情報として出力することが可能です。
ここでは、TriggerSDの使用方法を解説します。
 - 論理ボリューム選択
 - TiggerSDの取付
 - トリガービットの利用例

以下、PTSIMの実行ディレクトリ(例: ~/PTSproject-install/PTSapps/DynamicPort)で作業します。

## 例題マクロファイル
PTSIMコードに付属するマクロファイル`exampleA9.mac`をコピーして用います。
```
$ cp  ./macros/tut/exampleA9.mac  .
```

実行
```
$ ./bin/PTSdemo  -i  exampleA9.mac
```

終了
```
Session: exit
```

### マクロファイルの解説
解説するコマンド部分のみを抜粋して説明します。
マクロファイルでは、治療室にMLCと水ファントムを設置して、MLCにTriggerSDを取付ることにより、水ファントムの線量分布にMLCで散乱された粒子がどのように寄与しているかを確認する例を想定しています。

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
/Dynamic/Module/WaterPhantom/register Phantom
/Dynamic/Module/register MLC G4MMLCX ./data/Sample/G4MMLCX/mlc.dat 0. 0. 1000. mm
#
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
# WaterPhantom
/G4M/Module/Phantom/size 150. 150. 250.0 mm
/G4M/Module/Phantom/dim 300. 300. 500.
/G4M/Module/install Phantom
#
# MLC
/G4M/Module/install MLC
#
# TriggerSD
/G4M/Module/dumpLV     MLC
/G4M/Module/selectLV   MLC   1
/G4M/Module/attachTrgID  1
#
# Scoring
/My/runaction/dumpfile      A9.root
/My/runaction/ntuple/merge  true
#
# Track analysis
/My/runaction/ntuple/create    NT Phantom/HitsCollection 
/My/runaction/ntuple/addColumn NT evno       I
/My/runaction/ntuple/addColumn NT pid        I
/My/runaction/ntuple/addColumn NT ix         I
/My/runaction/ntuple/addColumn NT iy         I
/My/runaction/ntuple/addColumn NT iz         I
/My/runaction/ntuple/addColumn NT triggerid  I
/My/runaction/ntuple/addColumn NT triggerx   F
/My/runaction/ntuple/addColumn NT triggery   F
/My/runaction/ntuple/addColumn NT triggerz   F
/My/runaction/ntuple/addColumn NT de         F  keV
/My/runaction/ntuple/showScColumn NT
#
# BeamOn
/run/beamOn 100
#
```

#### 論理ボリュームの選択
TriggerSDを有効にする論理ボリュームを選択し、TriggerSDを取付ます。
はじめに論理ボリュームを選択します。モジュール内で論理ボリューム名が重複している場合、はじめにインタラクティブモードで実行して`/G4M/Module/dumpLV`コマンドを用いてモジュール内の論理ボリュームを調べて識別番号を確認し、`/G4M/Module/selectLV`コマンドでモジュール名と識別番号を指定して当該の論理ボリュームを選択する。

- MLCモジュールの内部構造を調べる。
```
Idle> /G4M/Module/dumpLV     MLC
```

- モジュール名と識別番号により論理ボリュームを選択する。
```
Idle> /G4M/Module/selectLV   MLC   1
```

#### TriggerSDを取付ける
取付ける際に、通過した際に立てるビット番号を指定する。
```
Idle> /G4M/Module/attachTrgID  1
```
ビット番号nであれば、ビットが立った時の10進数での値は2^nである。
つまり、ビット番号が１の場合は、10進数では2となる。

#### スコアリング
粒子情報は、スコ量`triggerid`に記録されている。また、ボリュームを通過した最終座標が`triggerx`, `triggery`,`triggerz`に記録されている。水ファントムでの線量とTriggerSDの情報をNtupleで保存する場合の例は、次のようなる。
```
/My/runaction/ntuple/create    NT Phantom/HitsCollection
/My/runaction/ntuple/addColumn NT evno       I
/My/runaction/ntuple/addColumn NT pid        I
/My/runaction/ntuple/addColumn NT ix         I
/My/runaction/ntuple/addColumn NT iy         I
/My/runaction/ntuple/addColumn NT iz         I
/My/runaction/ntuple/addColumn NT triggerid  I
/My/runaction/ntuple/addColumn NT triggerx   F
/My/runaction/ntuple/addColumn NT triggery   F
/My/runaction/ntuple/addColumn NT triggerz   F
/My/runaction/ntuple/addColumn NT de         F  keV
/My/runaction/ntuple/showScColumn NT
```


