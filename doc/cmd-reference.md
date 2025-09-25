# Command Reference

## コマンド引数の表記

コマンドに与える引数は、{} で囲んで表ます。  
また、その内容は、 **{値:型}** と表記しています。

```  
{value:type}  
```

| 値表記 |　説明 |
|:------|:------|
| value | 引数の値(変数名) |
| type | 引数の型(s=string, i=int, f=float, d=double)|  


* 記載例
```
/G4M/Module/select  {mname:s}  
```
引数として文字列型の値を指定します。引数名はmnameと呼びます。  

* 利用例 (文字列型の引数)
```
/G4M/Module/select  WaterPhantom  
```
**特に以下の３つの引数名については、以降では説明なく用いている場合があります。**

| 値表記 |　説明 |
|:---|:---|
| mname:s    |  Beam module name | 
| ifilename:s | Input file name |  
| ofilename:s  |Output file name  |
| unit:s       |Unit  |

---
## Geant4/PTSIM version
### Geant4のversionを表示する
```
/G4M/PTS/G4Version　{ofilename:s}
```
| 値表記 |　説明 |
|:---|:---|
| ofilename:s    |  出力ファイル名。標準出力は**stdout**を指定する。 |

### PTStoolkitのversionを表示する
```
 /G4M/PTS/TKVersion　{ofilename:s}
```
| 値表記 |　説明 |
|:---|:---|
| ofilename:s    |  出力ファイル名。標準出力は**stdout**を指定する。 |

### PTS application version numberを表示する
```
/G4M/PTS/AppVersion　{ofilename:s}
```
| 値表記 |　説明 |
|:---|:---|
| ofilename:s    |  出力ファイル名。標準出力は**stdout**を指定する。 |

---

## Primary Beam Generator
Primmary Beam geaneratpr（初期粒子発生器）を選択するコマンドです。
### 選択コマンド
```
/My/PrimaryGenerator/select　　{gname:s}
```

| 値表記 |　説明 |
|:---|:---|
| gname:s    |  Beam generator name. 下記に示す初期粒子発生器のいずれか１つを選択します。|

### Beam Generators
 - ParticleGun
 - [General Particle Source, GPS](./PrimGen/GPS.md)
 - BeamGun
 - ScanBeam
 - FocusGun
 - FocusGunEdist
 - FanBeam
 - IAEAphsp
 - EvtGun
 - Root3d

---

## Physics List
物理過程を選択します。PTSIMでは、Geant4が提供する物理コンストラクタ単位で登録を行う仕組みになっています。
### 登録コマンド
```
/My/physics/register　{physName:s}
```

| 値表記 |　説明 |
|:---   |:---  |
| physName:s    | Physics Constructor name |

 {PhysName:s}は、PTSIMのデフォルトで読み込まれるphys.macを参照してください。  
 物理カテゴリごとに１つの物理コンストラクタを選択します。

 ### Physics process options

 #### 指定RegionにStepLimitterを設定する
 ```
 /My/physics/stepLimitForRegion　{Region:s}  {stepsize:d}  {unit:s}
 ```

|値表記|説明 |
|:---|:---|  
| Region:s    |  Region name |  
| setpsize:d    |  ステップリミットの最大ステップ長 |  
| unit:s    |  デフォルト　mm |  
 
 PTSIMでは、各ビームモジュールごとにRegionを自動生成しています。原則的に{Region}と{mname}が同じになります。
 
 #### パラレルワールドプロセスを定義する
 ```
 /My/physics/pwProcess　{pwname:s} {layered:b} {ProcessName:s} {verbose:i}
 ``` 
| 値表記 |　説明 |
|:---|:---|
| pwname:s    |  Parallel World name |
| layered:b    |  Layered mass geometry用の場合はtrue, スコアのみはfalse |
| ProcessName:s   |  このプロセスの固有名 |
| verbose:i    |  verbose level |
 
パラレルワールド {pwname}を別途作成する必要があります。ジオメトリセクションを参照してください。  

#### 手動で物理過程を更新する
```
/G4M/Module/physicsModified
```
 
---
 ## System Selection

 PTSIMは、粒子線施設のシミュレーション体系を切り替えられるように設計してあります。これは施設特有の実装を、一般的な用途と切り離すためです。  
 そのため、一般的な利用に際しては、以下の説明の通り、選択するシステム名は**DynamicPort**を選択してください。
 
 ### システムを選択する。
 ```
 /G4M/System　　{system_name:s}
 ```

| 値表記 |　説明 |
|:---|:---|
| system_name:s  |  System name, **DynamicPort**を選択する。 |

#### システムを切り替える
一度選択したシステムを別のシステムに切り替える。
```
/G4M/ChangeSystem　{system_name:s}
```

---
## Material Definition
 物質を定義して作成します。  Geant4 NIST materialを用いる方法のほかに、ユーザ定義の物質を作成することもできます。

 ### Elementデータの指定
 　Element(元素)については、一般的にNISTが定義した元素を用いることを推奨します。これには自然混合比の同位体も含まれています。

 #### NIST database定義のElementを利用する
```
/G4M/Element/useG4Element　{flag:b}
```
| 値表記 |　説明 |
|:---|:---|
| flag:b    |  NIST Element = true,  Userdefined = false. |

 #### ユーザ定義のElementデータファイルを利用する
 ##### Elementデータファイルのパスを指定する 
 ```
 /G4M/Element/path　　{path:s}
 ```
| 値表記 |　説明 |
|:---|:---|
| path:s    |  実行場所からの相対パス. Default: ./data/commom/element |

Elementファイルの書式はこちら。 　

### Materialデータの指定と作成
　物質(Material)を作成します。
#### 　物質データファイルのパスを指定する
 ```
 /G4M/Material/path　　{path:s}
 ```
| 値表記 |　説明 |
|:--- |:--- |
| path:s | 実行場所からの相対パス. Default: ./data/commom/material/ |
 
 Materialファイルの書式はこちら。 　
 
 #### 物質を作成する
  ```
 /G4M/Material/create  {ifilename:s}
 ```
| 値表記 |　説明 |
 |:--- |:--- |
 | ifilename:s | 物質のパラメタファイル名、またはNISTの物質名 e.g/ G4_WATER |
NISTの物質を利用する場合には、パラメタファイル名の代わりに、G4_WATERのように物質名を指定します。  
例)  /G4M/Material  G4_WATER    
ユーザ定義の物質パラメタファイルを利用して物質を作成する場合は、データファイル名の拡張子は、.dat とします。作成コマンドでは、ファイル名の拡張子を除いた物質名を指定します。例えば、パラメタファイル名が、Water.datの場合は、Waterを指定します。
例) /G4M/Material  Water  

 #### 物質情報を表示する
 ```
 /G4M/Material/property  {matname:s}
 ```
| 値表記 |　説明 |
|:---|:---|
| matname:s    |  物質名. 原則、上記の{ifilename}と同じです。 |
 
 #### マテリアルファイル例
 * エレメントによる構成例：　./data/common/material/H_2O.dat
 * 物質による構成例: ./data/common/material/G10.dat

 ## Beam Module Registration
 シミュレーションで利用するビーム機器を登録します。後で登録したビーム機器を実体化してビームラインを構成して行きます。

 ### ビーム機器を登録する
 ```
 /Dynamic/Module/register　{mname:s}  {mtype:s}  {param:s} {x:d} {y:d}  {z:d} {lunit:s} {rx:d}  {ry:d} {rz:d} {runit:s}
 ```
| 値表記 |　説明 |
|:---|:---|
| mname:s | ビームモジュールにつける固有名 |
| mtype:s | ビームモジュールのクラス名 |
| param:s  | 形状データファイルパス　|
| x:d, y:d, z:d lunit:s|  配置座標(x,y,z)と単位 |
| rx:d, ry:d, rz:d, runit:s |  回転角と単位. rx->ry->rz順で回転される.|

### ビーム機器の登録を解除する
```
 /Dynamic/Module/unregister {mname:s}
```
| 値表記 |　説明 |
|:---|:---|
| mname:s |  ビームモジュール名 |

### スコアリング付きビーム機器の登録
　水ファントムなどの特別なビーム機器の登録には、専用の登録コマンドが用意されています。
#### 水ファントムの登録と解除
```
 /Dynamic/Module/WaterPhantom/register　　
 /Dynamic/Module/WaterPhantom/unregister
```

#### DICOMジオメトリの登録と解除
```
/Dynamic/Module/DICOM/register
/Dynamic/Module/DICOM/unregister
```

### 動的なビーム機器の設定

#### ワブラー電磁石を有効化する
```
/Dynamic/Module/updateEvent/wobbling {wnamex:s} {wnamey:s}  {flag:b}
```

| 値表記 |　説明 |
|:---|:---|
| wnamex:s, wnamey:s | ワブラー電磁石のビームモジュール名 |
| flag:b | 有効化=true, 無効化=false |


#### 回転モジュレータ有効化にする
```
/Dynamic/Module/updateEvent/rotating {mname:s}  {flag:b}
```

| 値表記 |　説明 |
|:---|:---|
| mnamex:s | 回転対称のビームモジュール名 |
| flag:b | 有効化=true, 無効化=false |

#### ワブラー電磁石や回転モジュレータの更新モード
　設定値の更新をイベント番号に従って順次更新する方法と、乱数によって更新する方法が選択可能です。
```
/Dynamic/Module/updateEvent/sequential {flag:b}
```
| 値表記 |　説明 |
|:---|:---|
| flag:b | sequential = true, random = false |

#### 回転モジュレータの初期角度と更新角度を設定
```
/Dynamic/Module/updateEvent/rotatingAngle {init:d} {step:d} {unit}
```

| 値表記 |　説明 |
|:---|:---|
| init:d | 初期角度 |
| step:d  | 追加する角度 |
| unit:s   |  rad, degree |

## Beam Module Installation
 登録されたビームモジュールの実体化・治療室への配置に関するコマンドです。

### ビームモジュールの実体化
```
/G4M/Module/install　{mname:s}  {pwname:s}  {layered:b}
```
| 値表記 |　説明 |
|:---|:---|
| mname:s | ビームモジュール名 |
| pwname:s | パラレルワールド名。デフォルト値 **none** (マスジオメトリに配置する場合) |
| layered:b  | Layered mass geometryである場合は**true**. デフォルト値は**false** |

### ビームモジュールを再度実体化
　既にインストールされているビームモジュールの場合には，引数の設定を基づいて再インストールします。インストールされていないビームモジュールの場合には、インストールは行われません。  
  このコマンドは，mass worldにインストールされたビームモジュールを，パラレルワールドに再配置するために用意されます。
  ```
  /G4M/Module/installIfExist {mname:s}  {pwName:s}  {layered:b}
  ```

| 値表記 |　説明 |
|:---|:---|
| mname:s | ビームモジュール名 |
| pwname:s | パラレルワールド名。デフォルト値 **none** (マスジオメトリに配置する場合) |
| layered:b  | Layered mass geometryである場合は**true**. デフォルト値は**false** |

### ビームモジュールの複製を配置す
  既にインストールされているビームモジュールがあるとき、引数の設定に基づいて論理ボリュームのコピーを作成してインストールします。
  　このコマンドは、ビームモジュールのエンベロープにコピー番号を付けて複数配置するために用意されます。
```
/G4M/Module/cloneIfExist  {mnameFrom:s} {mnameTo:s} {copyid:i} {x:d} {y:d} {z:d} {unit:s} {rx:d} {ry:d} {rz:d} {unit:s}
```
| 値表記 |　説明 |
|:---|:---|
| mnameFrom:s |  コピー元の実体化されているビームモジュール名 |
| mnameTo:s |   　コピーで複製されたビームモジュールにつけられる固有名 |
| copyid:i |   コピー先のビームモジュールのエンベロープのコピー番号 |
| x:d, y:d, z:d, unit:s |  コピー先の座標と単位　|
| rx:d, ry:d, rz:d,  unit:s |  コピー先での回転と単位 |

### ビームモジュールの実体を削除
```
/G4M/Module/uninstall　{mname:s}
```

### 全ビーム機器をインストール/アンインストール
登録済みのビームモジュール全てが対象に実体化や実体削除を行います。
```
/G4M/Module/installAll
/G4M/Module/uninstallAll
```

### 全ビームライン機器のインストール/アンインストール
水ファントムとDICOMを除くビームモジュールの実体化や実体削除を行います。
```
/G4M/Module/installBM
/G4M/Module/uninstallBM
```

### ビームモジュールの再構築
ビーム機器が既に実体化されている場合は、実体を削除して再構築し直します。ビーム機器のパラメタを更新した後の適用に用いられます。ビーム機器が実体化されていない場合は、ビーム機器のカタログパラメタに沿って、パラメタの更新のみが行われます。
```
/G4M/Module/rebuild　　{mname:s}
```
| 値表記 |　説明 |
|:---|:---|
| mname:s |  ビームモジュール名 |

### 登録ビームモジュールの状態を一覧表示
```
/G4M/Module/list  {lvl:i}  {ofilename:s}
```
| 値表記 |　説明 |
|:---|:---|
| lvl:i | 表示レベル　|
| ofilename:s | 出力ファイル名。デフォルト stdout (標準出力) |

### 実体化されているビーム機器の一覧を表示
```
/G4M/Module/listInWorld
```

### ビームモジュールを選択して操作する
ビームモジュールを選択して、選択した特定のビームモジュールに対して操作するコマンドを説明します。
#### ビーム機器を選択する
```
/G4M/Module/select  {mname:s}
```
| 値表記 |　説明 |
|:---|:---|
| mname:s |  ビームモジュール名 |

 選択されたビームモジュールに対して，次に紹介するコマンドが利用可能となる。

 #### ビーム機器情報の情報表示
```
/G4M/Module/lang   {type:i}
```
| 値表記 |　説明 |
|:---|:---|
| type:i |  表示タイプ。機器によって表示内容は異なる。 <br> DICOMモジュールの例 <br> Default=0 Comments in Japanese, <br> type=1 Comments in English, <br> type=10 dcmodify command shell script. |

#### ビーム機器情報の表示と保存
```
/G4M/Module/dump
/G4M/Module/dumpToFile   {ofilename:s}
```
| 値表記 |　説明 |
|:---|:---|
|ofilename:s |  デフォルト=stdout, またはファイル名 |

#### ビーム機器の形式パラメタを更新する
```
/G4M/Module/typeid　　　{ifilename:s}
```
| 値表記 |　説明 |
|:---|:---|
|ifilename:s|  機器形状パラメタファイル |

#### ビーム機器の中心座標を設定をする
```
/G4M/Module/translate  {X:d}  {Y:d}  {Z:d}  {unit}
```
| 値表記 |　説明 |
|:---|:---|
| X:d, Y:d, Z:d, unit:s |   x, y, z座標と単位 |

#### ビーム機器の下流z面で位置あわせする
```
/G4M/Module/downEdgeZ   {z:d}  {unit}
```
| 値表記 |　説明 |
|:---|:---|
|  z:d, unit:d |  z座標と単位　|

#### ビーム機器の上流z面で位置合わせする
```
/G4M/Module/upEdgeZ   {z:d}  {unit}
```
| 値表記 |　説明 |
|:---|:---|
|  z:d, unit:d |  z座標と単位　|

#### ビーム機器のX軸、Y軸、Z軸周りに回転する
```
/G4M/Module/rotate  {Rx:d}  {Ry:d}  {Rz:d}  {unit}
```
| 値表記 |　説明 |
|:---|:---|
|Rx:d, Ry:d, Rz:d, unit:s |  回転角度と単位 |

#### ビーム機器の-Z面に別のビーム機器を接して配置する
　このコマンドは、DICOMビームモジュールに、カウチとなるを別のビーム機器を接して配置するために用いられる。
```
/G4M/Module/attachZ　　{mname:s}
```
| 値表記 |　説明 |
|:---|:---|
| mname:s | 設置させるビーム機器の固有名　|

### デバッグフラグ
```
/G4M/Module/verbose  {lvl:i}
```
| 値表記 |　説明 |
|:---|:---|
| lvl:i | デバッグレベル |

---
## Special Beam Modules 
### [WaterPhantom commands](BeamModules/WaterPhantom.md)
### [DICOM commands](BeamModules/DICOM.md)
### [DICOM-RT commands](BeamModules/DICOMRT.md)
### [GDML commands](BeamModules/GDML.md)
### [VTK commands](BeamModules/VTK.md)
### [Dumper commands](BeamModules/Dumper.md)
### [BMField commands](BeamModules/BMField.md)

---

## Mass and Parallel World Geometry

### シミュレーション空間のデバック出力
```
/My/DetConstruction/verbose
```

### パラレルワールドジオメトリ
#### パラレルワールドジオメトリを作成する
```
/My/DetConstruction/createPW　　{pwname:s}
```
| 値表記 |　説明 |
|:---|:---|
| pwname:s | パラレルワールドの固有名 |

#### パラレルワールドの一覧を表示する
```
/My/DetConstruction/listPW
```

## Scoring and Histgraming 
 スコアリングにより粒子情報を記録するためには、論理ボリューム(LV)を選択してSD(SensitiveDetector)を有効化し、SDが蓄積している情報からNtupleファイルに出力する設定の２段階からなります。  
 WatarPhantomやDICOMなどの特別なビーム機器は、スコアリング機能が内包されているので、それぞれのセクションを参照してください。  
 ここでは一般的なスコアリング設定のコマンドを解説しています。

### Choosing Logical Volume for scoring

#### ビーム機器のLogical Volume名の一覧表示
```
/G4M/Module/dumpLV {mname:s}
```
| 値表記 |　説明 |
|:---|:---|
| mname:s | ビームモジュール名 |

 コマンド出力には、ビームモジュールを構成している論理ボリュームと、そのシリアル番号(lvid)が付けられて表示される。

#### ビームモジュール名とモジュール内での順序番号を指定して、論理ボリュームを選択する
```
/G4M/Module/selectLV　 {mname:s}   {lvid:i}
```
| 値表記 |　説明 |
|:---|:---|
| mname:s | ビームモジュール名 |
| lvid:i | 論理ボリュームのシリアル番号 |

* WaterPhantom、CylinderPhantom、 DICOM、PCTTubeDetector, PCTBoxDetectorジオメトリでは、SDは自動的に取り付けられている。下記のコマンドcreateSDやattachSDは、それら以外のジオメトリでのスコアリングを想定して用意されている。

### SensitiveDetector(DetectorSD)を作成
```
/G4M/Module/createSD {sdname:s} {colname:s} {edepFlag:b} {depx:i} {depy:i} {depz:i} {depm:i} {deps:i}
```
| 値表記 |　説明 |
|:---|:---|
| sdname:s | SD name |
| colname:s | HistCollection name (Default: HitsCollection )  |
| edepFlag:b |  true=付与エネルギーがある場合のみ記録する。ガンマ線などでは、反応時のみ記録される。<br> false=ガンマ線などは、反応が起きなくてもLVに入った時点で記録される。|
| depx:i, depy:i, depz:i, depm:i, deps:i | ジオメトリ識別番号を取得するレイヤ番号 |

### 論理ボリュームにSensitiveDetectorを取付
#### 論理ボリュームの選択
```
/G4M/Module/select  {mname:s}
```
####　SensitiveDetectorの取付け
選択中の論理ボリュームが取り付け対象になります。
```
/G4M/Module/attachSD  {sdname:s}
```
| 値表記 |　説明 |
|:---|:---|
| sdname:s | SD name |

### Geometric Trigger
selectLVコマンドで選択中の論理ボリュームに、TriggerSDを取り付ける。TriggerSDは、粒子が論理ボリュームに入るとあらかじめ設定したビットが粒子飛跡情報として記録されます。
#### TriggerSDの取り付け
選択中の論理ボリュームが取り付け対象になります。
```
/G4M/Module/attachTrgID    {tirgid:i}  {sdname:s}
```
| 値表記 |　説明 |
|:---|:---|
| trigid:i |  選択中の論理ボリュームに割り振るトリガー識別番号 |
|sdname:s |  TriggerSDの固有名, デフォルト”TreiggerSD” |
SD名で登録の有無を確認し、TriggerSDの存在を識別し、未作成の場合には自動的に生成される。原則としてTriggerSDは、１つしか作ってはいけないので注意。

#### 選択中の論理ボリュームの物質を変更
```
/G4M/Module/setMaterial    {matname:s}
```
| 値表記 |　説明 |
|:---|:---|
| matname:s | 選択中の論理ボリュームに割り当てる作成済みの物質名。<br> 
パラレルワールドでのレイヤジオメトリでは、”null”または”NULL”で不要なボリュームの物質をnullに設定することができる。|
* 関連コマンド: /G4M/Module/dump, /G4M/Module/selectLV

## Ntuples/Histgrams for output 

### 出力するファイル名を指定
```
/My/runaction/dumpfile {filename:s}
```
| 値表記 |　説明 |
|:---|:---|
| filename:s | 出力ファイル名（拡張子要）, .root,  .xml,  .csv |

### WaterPhantom/DICOMでのヒストグラムの有効化
```
/My/runaction/hist/enable {hname:s} {flag:b} {mname:s}
```

| 値表記 |　説明 |
|:---|:---|
| hname:s |  `TTree`: `DICOMCT`, `DICOM`　<br> `TH3`: `JSTEdep`, `JSTDose` |
| flag:b |   `true` / `false`（有効化 / 無効化）|
| mname:s |  モジュール名　<br>  省略可：`WaterPhantom` または `DICOM` <br> `TH3` 型で複数検出器がある場合、対象の検出器を指定 <br>`TTree` 型では、以下のコマンドでビームモジュールとの対応付けを行う |

### Ntuple作成とSensitive Detectorの対応付け
```
/My/runaction/ntuple/create {ntname:s} {colname:s}
```
| 値表記 |　説明 |
|:---|:---|
| ntname:s |  多次元ヒストグラム（ntuple）の固有名 | 
| colname:s |  記録するデータ元のヒットコレクション名（形式： `SDname/HitsCollection`） |

> **スコア付きビーム機器の場合の対応表：**

| BeamModule     | colname                     |
|----------------|-----------------------------|
| WaterPhantom   | WaterPhantom/HitsCollection |
| DICOM          | DICOM/HitsCollection        |
| TubeDetector   | TubeDetector/HitsCollection |
| BoxDetector    | BoxDetector/HitsCollection  |

### ヒストグラムの設定状態を表示
```
/My/runnaction/histList
```

### スコアリング物理量の候補を一覧表示
```
/My/runaction/ntuple/showScName
```

### Ntupleに保存する物理量を追加
```
/My/runaction/ntuple/addScColumn {nname:s} {qname:s} {type:s} {unit:s}
```
| 値表記 |　説明 |
|:---|:---|
| ntname:s |  Ntuple 名 |
| qname:s | スコアリング対象の物理量名 |
| type:s | `I`（整数）, `D`（実数）, `F`（実数） |
| unit:s |  単位（例: `MeV`, `mm`, 単位なし: `none`） |

### Ntupleに登録済みの物理量を一覧表示
```
/My/runaction/ntuple/showScColumn {ntname:s}
```
| 値表記 |　説明 |
|:---|:---|
| ntname:s |  Ntuple 名 |

---
## Event and Tracking

### MyEventActForMonitor
#### イベント番号をファイルに出力する
```
/My/{name}/eventMonitor   {nevent:i}   {filename:s}
```
- nevent:  イベントを出力する間隔
- filename:  出力ファイル名

### MyTrackingActForKillTrkMessenger
コマンドの{name}部分は、tracking1, tracking2の２つがデフォルトで使用可能。
#### 粒子生成を抑制するz領域を設定する
```
/My/{name}/killZUpStream　　{z:d} {unit}
```
- z:  z座標
- defaule unit: mm

#### 中性子の生成をしない
```
/My/{name}/killNeutral  {flag:b}
```
- flag:  true 生成しない

#### 軽粒子の生成をしない
```
/My/{name}/killLepton  {flag:b}
```
- flag:  true 生成しない

#### 二次粒子の生成をしない
```
/My/{name}/killSecondary  {flag:b}
```
- flag:  true 生成しない

---
## Digi ヒストグラム設定(試験運用)
### Digi Ntuple 作成
```
/My/runaction/ntuple/dc/create {ntname:s} {colname:s}
```

### Digiスコアリング可能な物理量を表示
```
/My/runaction/ntuple/showDcName
```

### Digi 保存情報を追加
```
/My/runaction/ntuple/addDcColumn {nname:s} {qname:s} {type:s} {unit:s}
```

### Digi 登録済み情報を表示
```
/My/runaction/ntuple/showDcSetColumn {nname:s}
```

### DigiSet Ntuple 作成
```
/My/runaction/ntuple/dcset/create {ntname:s} {colname:s}
```

### DigiSetスコアリング可能な物理量を表示
```
/My/runaction/ntuple/showDcSetName
```

### DigiSet 保存情報を追加
```
/My/runaction/ntuple/addDcSetColumn {nname:s} {qname:s} {type:s} {unit:s}
```

### DigiSet 登録済み情報を表示
```
/My/runaction/ntuple/showDcSetColumn {nname:s}
```

---

## Digitizer　（試験運用）
### 元データからデジタイズしたデータを作成する
元データのコレクションからデジタイズタイプに応じた処理を行い、処理後のデータコレクションを作成する。
```
/My/eventaction/digi/create  {name:s}  {DigiType:s} {ColFrom:s} {ColTo:s} {sdname:s}
```
| 値表記 |　説明 |
|:---|:---|
| name:s |  Digitizerの固有名 |
| DigiType:s | Digitizer処理を指定する |
| ColFrome:s | 元となるコレクション名 |
| ColTo:s | 処理後のコレクション名 |
| sdname:s | コレクション名の重複を区別するためにHitコレクションのSD名を指定する|

DigiTypeの選択肢  
* DigiVolMerger (Hit->Digi)|  
  * Hitから同じジオメトリのコピー番号をもつHitをグループ化する。ジオメトリ単位ごとに信号量としてエネルギー付与値を積算する。
* DigiCoincidence  (Digi->DigiSet)
  * PETでの時間コインシデンスをとり、設定時間幅のDigiデータをグループ化する。

### Digitizerモジュール選択
```
/My/eventaction/digi/select  {name:s}
```
| 値表記 |　説明 |
|:---|:---|
| name:s |  デジタイザーの固有名 |

### Digitizerモジュールの時間ウインドウ設定
selectで選択したdigitizerモジュールに時間ウインドウを設定する
ただし、DigiCoincidenceのときのみ有効となる。
#### 時間/エネルギーウインドウを定
```
/My/eventaction/coincidence/twin  {tw:d}  {toff:d} {unit:s}
/My/eventaction/coincidence/ewin  {emean:d}  {ew:d} {unit:s}
```
| 値表記 |　説明 |
|:---|:---|
| tw:d  | 時間ウインドウ |
| toff:d | 信号から時間ウインド開始までの時間オフセット |
| unit:s | 単位 |
| emean:d | エネルギー中心値 |
| ew:d | エネルギー幅　（emean+-ew） |
| unit:s |  単位 |

