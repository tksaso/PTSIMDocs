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

![exampleA91](../images/exampleA91.png)

陽子線を照射してみます。
```
Session: /run/beamOn 100
```
![exampleA92](../images/exampleA92.png)

終了
```
Session: exit
```

### マクロファイルの解説
解説するコマンド部分のみを抜粋して説明します。
マクロファイルでは、治療室にMLCと水ファントムを設置して、MLCにTriggerSDを取付ることにより、水ファントムの線量分布にMLCで散乱された粒子がどのように寄与しているかを確認する例を想定しています。MLCにかかるようにGPSの初期設定で、陽子線の拡がって照射されるような設定となっています。

```{code-block}
:linenos:
#
# (PreInit State)
/control/verbose 1
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
/gps/particle proton
/gps/energy   190. MeV
/gps/direction 0 0 -1
/gps/pos/type Beam
/gps/pos/sigma_x   30.0  mm
/gps/pos/sigma_y   40.0  mm
/gps/pos/centre  0. 0. 3500. mm
/gps/ang/type  beam2d
/gps/ang/sigma_x   30.0  mrad
/gps/ang/sigma_y   20.0  mrad
#
# WaterPhantom
/G4M/Module/Phantom/size 150. 150. 250.0 mm
/G4M/Module/Phantom/dim 300. 300. 500.
/G4M/Module/select Phantom
/G4M/Module/rotate 0. 180. 0. degree
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
#/run/beamOn 10000
#
```

#### 論理ボリュームの選択
TriggerSDを有効にする論理ボリュームを選択し、TriggerSDを取付ます。
はじめに論理ボリュームを選択します。モジュール内で論理ボリューム名が重複している場合、はじめにインタラクティブモードで実行して`/G4M/Module/dumpLV`コマンドを用いてモジュール内の論理ボリュームを調べて識別番号を確認し、`/G4M/Module/selectLV`コマンドでモジュール名と識別番号を指定して当該の論理ボリュームを選択する。

(補足)  
はじめに`/G4M/Module/dumpLV`コマンでで論理ボリュームを調べるときには、`/G4M/Module/selectLV`コマンドと`/G4M/Module/attachTrgID`コマンドは、コメントにして実行します。
標準出力の表示を確認して、論理ボリュームの識別番号を確認します。

その後に、`/G4M/Module/selectLV`コマンドと`/G4M/Module/attachTrgID`コマンドを有効化して、論理ボリュームやトリガービットを指定して実行することになります。

- MLCモジュールの内部構造を調べる。
```
Idle> /G4M/Module/dumpLV     MLC
```
`/G4M/Module/dumpLV MLC`の表示は次のようになります。
```
/G4M/Module/dumpLV MLC
0 MLC : Air(1)
1  Iron : Iron(0)
```
上記の表示は、最初が論理ボリュームの識別番号、２番目が論理ボリューム名、コロンの後は物質名と括弧書きで子論理ボリュームの個数です。

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

#### 実行と解析例
照射数を10000にして実行してみましょう。マクロファイルの`/run/beamOn`を有効にして照射数を10000に修正した後に実行してみましょう。
```
$ ./bin/PTSdemo  -m  exampleA9.mac
```

実行後に、`A9.root`が作成されているので、解析してみましょう。
MLCでの散乱放射線による深度エネルギー付与分布を見て見ましょう。

```
$ root A9.root
root[] .ls
root[] NT->Print()
root[] NT->Draw("iz","de*(triggerid==2)")
root[] NT->Draw("iz","de")
root[] NT->Draw("iz","de*(triggerid==2)","same")
root[] NT->Draw("trigerx:triggery:triggerz","triggerid==2")
```

MLCで散乱された粒子による水ファントム内の深度エネルギー付与分布  
`NT->Draw("iz","de*(triggerid==2)")`
![exampleA9iztrig2](../images/exampleA9iztrig2.png)

水ファントム内の全深度エネルギー付与分布とMLC散乱粒子の分布の重ね書き
`NT->Draw("iz","de")`及び`NT->Draw("iz","de*(triggerid==2)","same")`の描画
![exampleA9iztrig2cmp](../images/exampleA9iztrig2cmp.png)

MLCでの散乱位置
`NT->Draw("trigerx:triggery:triggerz","triggerid==2")`の描画
![exampleA9iztrig2MLC](../images/exampleA9iztrig2MLC.png)

以上