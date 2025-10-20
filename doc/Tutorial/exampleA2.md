# Example A2(初期粒子発生GPS）

シミュレーション空間で初期粒子発生器の選択方法とその１つであるGeneral Particle Source, GPSの設定方法を解説します。
 - 初期粒子発生器GPSの選択
 - GPS設定
 - Ntupleでの確認

以下、PTSIMの実行ディレクトリ(例: ~/PTSproject-install/PTSapps/DynamicPort)で作業します。

## 例題マクロファイル
PTSIMコードに付属するマクロファイル`exampleA2.mac`をコピーして用います。
```
$ cp  ./macros/tut/exampleA2.mac  .
```

実行
```
$ ./bin/PTSdemo  -i  exampleA2.mac
```
治療室の原点\(アイソセンター\)に水ファントムが配置されたシミュレーション空間です。

終了
```
Session: exit
```

### マクロファイルの解説
解説するコマンド部分のみを抜粋して説明します。

```{code-block}
:linenos:
#
# (PreInit State)
#
# Material
/G4M/Material/create G4_AIR
/G4M/Material/create G4_WATER
#
# PhysicsList
/control/execute ./macros/common/phys.mac
#
# System and module registration
/G4M/System DynamicPort
/Dynamic/Module/Room/register 525.  525.  3550. mm
#
# Run Initialize
/run/initialize
#
# (PreInit State)
#
# Primary particle
/My/PrimaryGenerator/select GPS
#
# GPS commands
/gps/particle proton
/gps/ene/type  Gauss
/gps/ene/mono   190. MeV
/gps/ene/sigma   0.3 MeV
/gps/pos/centre  0. 0. 3500. mm
/gps/pos/type Beam
/gps/pos/sigma_x   10.0  mm
/gps/pos/sigma_y   20.0  mm
/gps/pos/centre  0. 0. 3500. mm
/gps/direction 0 0 -1 
/gps/ang/type  beam2d
/gps/ang/sigma_x   3.0  mrad
/gps/ang/sigma_y   6.0  mrad
#
# 
/My/runaction/dumpfile      A2.root
/My/runaction/ntuple/merge  true
#
# Check the primary particle generator#
/My/runaction/ntuple/primary  true
#
#/run/beamOn 10
#
```

#### 初期粒子発生器の選択
 初期粒子発生器(Primary Particle Generator)は、`/My/PrimaryGenerator/select`[解説](../cmd-reference.md#primary-beam-generator)コマンドで引数に使用する初期粒子発生器の名称を与えます。選択可能な初期粒子発生器は、前述の解説を参照してください。このマクロファイルでは、`GPS`, General Particle Sourceを選択しています。

#### GPSコマンド
GPSコマンドについての詳細は、Geant4のドキュメント[GPS](https://geant4.web.cern.ch/documentation/dev/bfad_html/ForApplicationDevelopers/GettingStarted/generalParticleSource.html)を参照してください。
基本的に次のコマンドで構成されます。

1. 粒子種設定`/gps/particle`  
2. 発生位置`/gps/pos`
3. 発生方向`/gps/direction`及び`/gps/ang`

発生位置や方向に関しては、`type`指定があり、その選択によって有効なコマンドも変わりますので注意してください。

#### 初期粒子の分布確認
通常、Qtウインドウでの可視化で発生点や方向などを確認で来ますが、`/My/runaction/ntuple/primary ture`コマンドで初期発生粒子の情報をNtupleデータで確認することができます。保存されるNtupleの名称は、`PRIM`となります。
```
 root -l A2.root 
root [0] 
Attaching file A2.root as _file0...
(TFile *) 0x13565e850
root [1] .ls
TFile**		A2.root	
 TFile*		A2.root	
  KEY: TTree	PRIM;1	Primary Particle Info.
root [2] PRIM->Print()
******************************************************************************
*Tree    :PRIM      : Primary Particle Info.                                 *
*Entries :     1000 : Total =          104133 bytes  File  Size =      44194 *
*        :          : Tree compression factor =   2.28                       *
******************************************************************************
*Br    0 :evno      : Int_t PRIM                                             *
*Entries :     1000 : Total  Size=       8649 bytes  File Size  =       3385 *
*Baskets :        2 : Basket Size=      32000 bytes  Compression=   2.41     *
*............................................................................*
*Br    1 :vtxid     : Int_t PRIM                                             *
*Entries :     1000 : Total  Size=       8654 bytes  File Size  =       1548 *
*Baskets :        2 : Basket Size=      32000 bytes  Compression=   5.27     *
*............................................................................*
*Br    2 :np        : Int_t PRIM                                             *
*Entries :     1000 : Total  Size=       8639 bytes  File Size  =       1546 *
*Baskets :        2 : Basket Size=      32000 bytes  Compression=   5.27     *
*............................................................................*
*Br    3 :x0        : Float_t PRIM                                           *
*Entries :     1000 : Total  Size=       8639 bytes  File Size  =       5634 *
*Baskets :        2 : Basket Size=      32000 bytes  Compression=   1.45     *
*............................................................................*
*Br    4 :y0        : Float_t PRIM                                           *
*Entries :     1000 : Total  Size=       8639 bytes  File Size  =       5648 *
*Baskets :        2 : Basket Size=      32000 bytes  Compression=   1.44     *
*............................................................................*
*Br    5 :z0        : Float_t PRIM                                           *
*Entries :     1000 : Total  Size=       8639 bytes  File Size  =       1561 *
*Baskets :        2 : Basket Size=      32000 bytes  Compression=   5.22     *
*............................................................................*
*Br    6 :t0        : Float_t PRIM                                           *
*Entries :     1000 : Total  Size=       8639 bytes  File Size  =       1543 *
*Baskets :        2 : Basket Size=      32000 bytes  Compression=   5.28     *
*............................................................................*
*Br    7 :pid       : Int_t PRIM                                             *
*Entries :     1000 : Total  Size=       8644 bytes  File Size  =       1564 *
*Baskets :        2 : Basket Size=      32000 bytes  Compression=   5.21     *
*............................................................................*
*Br    8 :ke        : Float_t PRIM                                           *
*Entries :     1000 : Total  Size=       8639 bytes  File Size  =       4707 *
*Baskets :        2 : Basket Size=      32000 bytes  Compression=   1.73     *
*............................................................................*
*Br    9 :px        : Float_t PRIM                                           *
*Entries :     1000 : Total  Size=       8639 bytes  File Size  =       5661 *
*Baskets :        2 : Basket Size=      32000 bytes  Compression=   1.44     *
*............................................................................*
*Br   10 :py        : Float_t PRIM                                           *
*Entries :     1000 : Total  Size=       8639 bytes  File Size  =       5634 *
*Baskets :        2 : Basket Size=      32000 bytes  Compression=   1.45     *
*............................................................................*
*Br   11 :pz        : Float_t PRIM                                           *
*Entries :     1000 : Total  Size=       8639 bytes  File Size  =       4481 *
*Baskets :        2 : Basket Size=      32000 bytes  Compression=   1.82     *
*............................................................................*
root [3] PRIM->Draw("ke")
```
