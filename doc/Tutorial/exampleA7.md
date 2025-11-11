# Example A7(Layered-Mass）

シミュレーション空間に配置したジオメトリとオーバラップして物体を置きたい場合にLayered-Massジオメトリを用います。
例えばBranchyTherapyのように体内に線量カプセルを配置する場合などです。基本的には、パラレルワールドでのスコアリングと同様ですが、パラレルジオメトリの物質を有効化する点において違いがあります。

パラレルワールド・レイヤードマスジオメトリ
 - パラレルワールドの作成
 - パラレルワールド用プロセスの有効化
 - パラレルワールドへのジオメトリ構築
 - 

 - ファイルの作成
 - Lビームモジュール登録と実体化

以下、PTSIMの実行ディレクトリ(例: ~/PTSproject-install/PTSapps/DynamicPort)で作業します。

## 例題マクロファイル
PTSIMコードに付属するマクロファイル`exampleA7.mac`をコピーして用います。
```
$ cp  ./macros/tut/exampleA7.mac  .
```

実行
```
$ ./bin/PTSdemo  -i  exampleA7.mac
```

終了
```
Session: exit
```

### マクロファイルの解説
解説するコマンド部分のみを抜粋して説明します。
このマクロファイルでは、実空間(Mass-world)の治療室に水ファントムが配置されています。
パラレルワールドに水ファントムとオーバーラップするようにコリメータを配置しています。

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
/Dynamic/Module/register Collimator G4MBMGdml ./data/Sample/G4MBMGdml/Collimator.gdml 0. 0. 0. mm
#
# Create Parallel World
/My/DetConstruction/createPW  paraWorld0
#
# Activate Parallel World Process
/My/physics/pwProcess     paraWorld0  true
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
# (Mass-World) Scoring volume using WaterPhantom
/G4M/Module/Phantom/size   100. 100. 250.0 mm
/G4M/Module/Phantom/dim    100. 100. 250.
/G4M/Module/Phantom/material G4_WATER
/G4M/Module/install  Phantom
#
# (Parallel-World, Layered Mass)
/G4M/Module/install  Collimator  paraWorld0  true
#
# Replace Air-material with null.
/G4M/Module/selectLVByName Collimator  CollimatorL1
/G4M/Module/setMaterial  null
#
# Scoring
/My/runaction/dumpfile      A7.root
/My/runaction/ntuple/merge  true
#
# Track analysis
/My/runaction/ntuple/create    NT Phantom/HitsCollection 
/My/runaction/ntuple/addColumn NT pid  I
/My/runaction/ntuple/addColumn NT ix   I
/My/runaction/ntuple/addColumn NT iy   I
/My/runaction/ntuple/addColumn NT iz   I
/My/runaction/ntuple/addColumn NT de   F  keV
/My/runaction/ntuple/showScColumn NT
#
# BeamOn
/run/beamOn 100
#
```

#### Parallel Worldを作成する

パラレルワールドの作成は、以下のようにパラレルワールドにつける固有名を引数にコマンドを実行します。
```
PreInit> /My/DetConstruction/createPW  paraWorld0 
```

#### Parallel World用のプロセスを有効化
粒子をトラッキングする際にパラレルワールドを参照するように、専用のプロセスを追加します。
パラレルワールド・スコアリングと異なるのは２番目の引数で、Layered-Mass用であるフラグをtrueに設定するところです。

```
PreInit> /My/physics/pwProcess     paraWorld0   true
```
１番目の引数がパラレルワールドの固有名です。２番目の引数はパラレルワールドの物質を利用するかどうかを指定します。
Layerd-Massでは、このフラグを`true`に設定します。これにより、パラレルワールドに配置したボリュームの物質を優先的に参照してシミュレーションが行われます。パラレルワールドの物質が`null`であった場合は、通常通りにMass-Worldの物質が参照されます。

#### パラレルワールドでのジオメトリ構築

ジオメトリそのものは、通常のビームモジュールの扱いと一部を除き同じです。

- コリメータのビームモジュール登録
```
PreInit> /Dynamic/Module/register Collimator G4MBMGdml ./data/Sample/G4MBMGdml/Collimator.gdml 0. 0. 0. mm
```

- コリメータをパラレルワールドにLayered-Massジオメトリとして実体化
```
Idle> /G4M/Module/install  Collimator  paraWorld0  true
```
`/G4M/Module/install`コマンドを使って実体化します。１番目の引数はモジュール名、２番目の引数はパラレルワールドの固有名、３番目のフラグがLayered-Massのジオメトリとして実体化することを表します。

- コリメータビームモジュールの空気部分の物質を変更
コリメータはアルミニウム製で、中心部分に穴があいた形状をしています。水ファントムにコリメータを埋め込む想定をする場合、この中心の穴は、空気ではなく水ファントムの物質になるべきです。Mass-Worldの物質を参照するには、パラレルワールドの物質は`null`としなければなりません。

コリメータのGDMLファイルは、次のようになっており、空気部分の論理ボリューム名が`CollimatorL1`であることがわかります。
```{xml}
:linenos:
<?xml version="1.0" encoding="UTF-8" standalone="no" ?>
<gdml xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="./data/schema/gdml.xsd">

  <define/>

  <materials/>

  <solids>
    <tube aunit="deg" deltaphi="360" lunit="mm" name="CollimatorS1" rmax="75" rmin="0" startphi="0" z="200"/>
    <tube aunit="deg" deltaphi="360" lunit="mm" name="CollimatorS2" rmax="225" rmin="0" startphi="0" z="200"/>
  </solids>

  <structure>
    <volume name="CollimatorL1">
      <materialref ref="G4_AIR"/>
      <solidref ref="CollimatorS1"/>
      <auxiliary auxtype="Color" auxvalue="#8080804c"/>
    </volume>
    <volume name="CollimatorL2">
      <materialref ref="Aluminium"/>
      <solidref ref="CollimatorS2"/>
      <physvol name="CollimatorP1">
        <volumeref ref="CollimatorL1"/>
      </physvol>
      <auxiliary auxtype="Color" auxvalue="#8000004c"/>
    </volume>
  </structure>

  <setup name="Collimator" version="1.0">
    <world ref="CollimatorL2"/>
  </setup>

</gdml>
```

そこで、次のようにコリメータモジュール内の論理ボリュームを選択し、その物質を`null`に設定します。
```
/G4M/Module/selectLVByName Collimator  CollimatorL1
/G4M/Module/setMaterial  null
```
*ビームモジュール内で、論理ボリューム名に重複がある場合、`selectLVByName`コマンドは、最初に見つかった論理ボリュームを選択します。論理ボリューム名が重複している場合は、次の手順のように、モジュール内の構造を確認して、構造番号を指定してください。

1. インタラクティブモードで起動して、モジュール名を指定してモジュール内の論理ボリューム構造を表示する。
```
Idle>/G4M/Module/dumpLV Collimator
```
2. 表示から変更を要する論理ボリュームの階層番号を確認する
3. モジュール名と階層番号を指定して論理ボリュームを選択し、物質を設定する
```
Idle> /G4M/Module/selectLV Collimator  1
Idle> /G4M/Module/setMaterial  null
```

