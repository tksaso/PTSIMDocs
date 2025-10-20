# Example A0(基本構成)

 例題のシミュレーション空間(治療室)の作成方法を解説し、基本的な設定方法を体験します。
 - 物質作成
 - 治療室の登録

以下、PTSIMの実行ディレクトリ(例: ~/PTSproject-install/PTSapps/DynamicPort)で作業します。

## 例題マクロファイル
PTSIMコードに付属するマクロファイル`exampleA0.mac`をコピーして用います。
```
$ cp  ./macros/tut/exampleA0.mac  .
```

実行
```
$ ./bin/PTSdemo  -i  exampleA0.mac
```
治療室だけの空間です。終了します。
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
/control/execute ./macros/common/gps.mac
#
#
#/run/beamOn 10
#
```

#### 物質作成
 使用する物質を予め作成しておく必要があります。
 物質作成コマンドは、`/G4M/Material/create`[解説](../cmd-reference.md#material)で引数に物質名を与えます。

#### 外部マクロファイルの呼び出し
`/control/execute`は、別のマクロファイルを読み込んで実行します。
このサンプルマクロでは、物理過程の選択`phys.mac`と初期粒子の選択`gps.mac`で使用しています。
これらのファイルの内容については、別途説明します。

#### システムの選択と治療室（ワールドボリューム）の登録
システムの選択は、`/G4M/System DynamicPort`コマンドで行います。これで一択になります。
治療室は、`/Dynamic/Module/Room/register`コマンドで行います。引数は各辺の半長とその単位です。
シミュレーション空間のワールドボリュームとなり、その名称は*Room*です。空間はG4_AIRで満たされます。
登録後にRoom固有コマンド[解説](../BeamModules/Room.md)が利用可能です。


#### Run Initialize
モジュールの登録後に、`/run/initialize`を実行します。Geant4の初期化が行われワールドボリュームが作成されます。

#### 初期粒子発生の選択設定
初期粒子発生器を`/My/PrimaryGenerator/select`コマンドで選択します。
このサンプルでは、GPS, General Particle Sourceが選択されており、GPSの固有設定は別のマクロファイル`gps.mac`に記載されています。GPSの詳細は別途説明します。


これらが最小構成の基本コマンドです。

