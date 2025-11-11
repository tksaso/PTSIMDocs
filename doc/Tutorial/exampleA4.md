# Example A4(GDMLビーム機器）

前章では、粒子線治療で用いられる機器のビームモジュールクラスを実体化しました。しかし、独自に機器を設計して配置したいときも出てきます。その場合には、G4BMGdmlクラスのビームモジュールを登録して、GDML, Geometry Description Markup Languageで記述したファイルをインポートしてビーム機器を作成することができます。

この例題では、シミュレーション空間にGDMLで記述した独自にビームモジュールを配置する方法を紹介します。
 - GDMLファイルの作成
 - GDMLビームモジュール登録と実体化

以下、PTSIMの実行ディレクトリ(例: ~/PTSproject-install/PTSapps/DynamicPort)で作業します。

## 例題マクロファイル
PTSIMコードに付属するマクロファイル`exampleA4.mac`をコピーして用います。
```
$ cp  ./macros/tut/exampleA4.mac  .
```

実行
```
$ ./bin/PTSdemo  -i  exampleA4.mac
```
![exampleA4](../images/exampleA4.png)

終了
```
Session: exit
```

### マクロファイルの解説
解説するコマンド部分のみを抜粋して説明します。
この例題では、GDMLで記述したExtract(ビーム出口の円盤）とCollimator(穴の空いた円筒形）の場合で解説します。

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
/Dynamic/Module/register Extract G4MBMGdml ./data/Sample/G4MBMGdml/Extract.gdml 0. 0. 3500. mm 
/Dynamic/Module/register Collimator G4MBMGdml ./data/Sample/G4MBMGdml/Collimator.gdml 0. 0. 1500. mm
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
# Beam-module installation
/G4M/Module/install Extract
/G4M/Module/install Collimator
#
#/run/beamOn 10
#
```

#### ビーム機器の登録
 使用するビーム機器を`PreInit State`で、`/Dynamic/Module/register`コマンドにより登録します。
コマンドの引数は順番に、モジュールの固有名{mname}、モジュールタイプ{mtype}、パラメタファイル名{param}、配置座標と単位、回転角と単位です。
```
PreInit> /Dynamic/Module/register  {mname} {mtype} {param}  {x} {y} {z} {lunit} {rx} {ry} {rz} {runit}
```
配置座標や回転角は省略すると原点、回転なしになります。例題マクロでは、配置座標のみ与えています。
GDMLのビーム機器も用いる場合は、`{mtype}`は`G4MBMGdml`となります。そしてパラメタファイル`{param}`は、GDMLファイルになります。

#### GDMLスキーマ
GDMLのXMLタグを定義したファイルをスキーマと呼びます。GDMLファイルには、スキーマへのパスがGDMLファイルの保存場所からの相対パスで指定されて記載されています。前述のようにGDMLファイルが実行ディレクトリ意外にあるときは、スキーマファイルのパスを`/Dynamic/Module/gdml/schema  ./data/schema/gdml.xsd`のように設定することを推奨します。

#### GDMLファイル

GDMLでのジオメトリ記述方法の詳細は、[GDMLマニュアル](https://gdml.web.cern.ch/GDML/doc/GDMLmanual.pdf)を参照してください。

大まかには、次のセクションに分けられます。
- パラメタ変数などを定義する`<define/>`セクション
- 物質を定義する`<material/>セクション
- 形状を定義する`<solid>`セクション
- 全体の構造を定義する`<structure>`セクション
があります。  

また、`<structure>`セクションの中に、
- 論理ボリュームを宣言する`<volume>`セクション
- 論理ボリューム内に子論理ボリュームを配置する`<physvol>`セクション
があります。

その他、位置座標を示す`<position/>`タグや回転を示す`<rotation/>`タグがあります。

最も簡単な例として、`Extract.gdml`を示します。

```xml
:caption: Extract.gdml
<?xml version="1.0" encoding="UTF-8" standalone="no" ?>
<gdml xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="./data/schema/gdml.xs\
d">

  <define/>

  <materials/>

  <solids>
    <tube aunit="deg" deltaphi="360" lunit="mm" name="ExtractS1" rmax="50" rmin="0" startphi="0" z="0.1"/>
  </solids>

  <structure>
    <volume name="ExtractL1">
      <materialref ref="Titanium"/>
      <solidref ref="ExtractS1"/>
      <auxiliary auxtype="Color" auxvalue="#8080004c"/>
    </volume>
  </structure>

  <setup name="Extract" version="1.0">
    <world ref="ExtractL1"/>
  </setup>

</gdml>
```

*PTSIMでは、モジュール毎にGDMLファイルを作成します。そのため、PTSIM独自ルールがあります。
1. 原則的に物質は`/G4M/Material/create`コマンドで作成しておいてください。
   デフォルトではGDMLの`<material/>`セクションで定義されている物質はスキップして作成しません。Geant4のGDMLパーサは、GDMLファイル内で物質を定義しているかを確認します。PTSIMでは、物質を先に作成しておく方法を採用しているため、実行時に警告文がでますが警告は無視して構いません。
2. 登録時のモジュール名`{mname}`とGDMLファイル内の<setup name=... >の名称は同じにしてください。

*またGDML共通ですが、`<solid name=...>`, `<volume name=...>`, `<physvol name=...>`などの`name`は、固有名となるようにしてください。複数同名のものがあるとXMLパーサは判断できなくなり、正しい構造が構築できなくなります。

#### ビーム機器の選択と配置条件の再設定
 他のビーム機器と同様に扱えます。ビーム機器の配置座標や回転は、ビーム機器名を引数に`/G4M/Module/select`コマンドで選択した後に、`/G4M/Module/translate`コマンド及び`/G4M/Module/rotate`コマンドで変更することができます。

#### ビーム機器の実体化
 他のビーム機器と同様に扱えます。 設定したビーム機器を治療室に実体化して配置するには、ビーム機器名を引数に`/G4M/Module/install`コマンドを実行します。





