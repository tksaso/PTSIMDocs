# Example A3(ビーム機器登録/操作）

シミュレーション空間にビームモジュールを配置する際の利用例を解説します。
 - ビームモジュールの登録
 - ビームモジュールの実体化

以下、PTSIMの実行ディレクトリ(例: ~/PTSproject-install/PTSapps/DynamicPort)で作業します。

## 例題マクロファイル
PTSIMコードに付属するマクロファイル`exampleA3.mac`をコピーして用います。
```
$ cp  ./macros/tut/exampleA3.mac  .
```

実行
```
$ ./bin/PTSdemo  -i  exampleA3.mac
```

終了
```
Session: exit
```

### マクロファイルの解説
解説するコマンド部分のみを抜粋して説明します。
この例題では、ユーザ実装用に用意したG4MBMTemplateクラス（空の箱）とMulti-leaf Collimatorの機器の配置を通じて説明します。

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
/Dynamic/Module/register BMT  G4MBMTemplate  dummy.dat  0. 0. 0. mm 0. 0. 0. deg
/Dynamic/Module/register MLC  G4MMLCX ./data/Sample/G4MMLCX/mlc.dat   0. 0. 1000. mm 0. 0. 0. deg
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
# Beam-module settings
#
/G4M/Module/select BMT
/G4M/Module/translate 0. 0. -100 mm
/G4M/Module/install BMT
#
/G4M/Module/select MLC
/G4M/Module/rotate  0. 0. 45. degree
/G4M/Module/install MLC
#
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
配置座標や回転角の設定を省略した場合は、原点に回転なしの配置になります。
モジュールタイプ{mtype}は、モジュールのクラス名で、パラメタファイル{param}と対応します。利用可能なモジュールタイプとパラメタの書式については[解説](../cmd-reference.md#Beam-Module-Registration)を参照してください。



#### ビーム機器の選択と配置条件の再設定
 ビーム機器の配置座標や回転は、ビーム機器名を引数に`/G4M/Module/select`コマンドで選択した後に、`/G4M/Module/translate｀コマンド及び`/G4M/Module/rotate`コマンドで変更することができます。

#### ビーム機器の実体化
 設定したビーム機器を治療室に実体化して配置するには、ビーム機器名を引数に`/G4M/Module/install`コマンドを実行します。







