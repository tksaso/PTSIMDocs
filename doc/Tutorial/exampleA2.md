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
/gps/particle proton
/gps/energy   190. MeV
/gps/direction 0 0 -1 
/gps/pos/centre  0. 0. 3500. mm
/gps/pos/type Beam
/gps/pos/sigma_x   10.0  mm
/gps/pos/sigma_y   20.0  mm
/gps/pos/centre  0. 0. 3500. mm
/gps/ang/type  beam2d
/gps/ang/sigma_x   3.0  mrad
/gps/ang/sigma_y   6.0  mrad
#
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


 


