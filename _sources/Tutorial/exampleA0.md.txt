# Example A0(基本構成)

 例題のシミュレーション空間(治療室)の作成方法を解説し、基本的な設定方法を体験します。

解説概要
 - 物質作成
 - システム選択
 - 治療室の登録
 - 初期粒子発生器の選択

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
治療室だけの空間です。

ビームを打ってみます。
```
Session: /run/beamOn 10
```
![exampleA0.png](../images/exampleA0.png)

終了します。
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
5行目から6行目で物質を作成しています。物質作成は、`/G4M/Material/create`コマンドで作成します。引数に物質名を与えます。 
上記の例では、NISTマテリアルデータベースで事前定義された物質を作成しています。
物質は、ジオメトリ構築前に作成しておく必要があります。 
物質作成コマンドの詳細並びにユーザ定義物質の作成については、[コマンドリファレンス](../cmd-reference.md#material)を参照してください。

#### 外部マクロファイルの呼び出し
`/control/execute`コマンドは、引数で指定した別のマクロファイルを読み込んで実行します。 
このサンプルマクロでは、9行目の物理過程の選択`phys.mac`と22行目で初期粒子の選択`gps.mac`で使用しています。
これらのファイルの内容については、別の例題紹介で説明します。

#### システムの選択と治療室（ワールドボリューム）の登録
12行目でシステムの選択を行なっています。治療施設に特化したシステムを実装した場合には、`/G4M/System`コマンドで切り替えできるようにPTSIMは設計されています。汎用的なシステムとしては、`/G4M/System DynamicPort`の一択になります。`DynamicPort`が選択されることにより、`/Dynamic/`で始まるコマンドが利用できます。
シミュレーション空間である治療室のシステムへの登録は、13行目の`/Dynamic/Module/Room/register`コマンドで行います。引数は各辺の半長とその単位です。  シミュレーション空間のワールドボリュームとなり、その名称は*Room*と決められています。デフォルトで治療室の空間はG4_AIRで満たされます。登録後でも*Room*のサイズや物質の変更が可能です。*Room*の固有コマンドについては、[コマンドリファレンス](../BeamModules/Room.md)を参照してください。

#### Run Initialize
モジュールの登録後に、16行目で`/run/initialize`を実行しています。このコマンド実行によって、Geant4の初期化が行われワールドボリュームが作成されます。このコマンド以前の状態を`PreInit>`ステート、以後を`Idle>`ステートと呼びます。

#### 初期粒子発生の選択設定
21行目で初期粒子発生器を`/My/PrimaryGenerator/select`コマンドで選択しています。
このサンプルでは、*GPS, General Particle Source*が選択されています。*GPS*で初期粒子発生条件を設定する固有コマンドは、別のマクロファイル`gps.mac`に記載されています。GPSの詳細は別の例題紹介で説明します。

これらが最小構成の基本コマンドです。

