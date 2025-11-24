# Example A8(Scan/Raster-Beam）

スポットスキャニングビームやラスタースキャンビームの設定方法を解説します。
 - パラメタファイル準備
 - スキャニングマグネット設置

スポットスキャニングやラスタースキャニングは、ビームを電磁石で走査して照射対象内のある深度に線量面を構成します。エネルギーを変えて同様の線量面を層状に重ね合わせることで、腫瘍領域に必要な線量を照射します。
これらの設定を行うパラメタファイルには、次の３つが必要です。

1. ビームエネルギー定義（EIDSample.dat ）
2. ビーム強度補正定義(WeightSample.dat)
3. ビームスポット設定（ScanSample.dat）

ここでは、スポットスキャニングを中心に、例題マクロファイルに沿って使用法を解説します。

以下、PTSIMの実行ディレクトリ(例: ~/PTSproject-install/PTSapps/DynamicPort)で作業します。

はじめに、パラメタファイルは編集することもあるので、サンプルのパラメタファイルを実行ディレクトリにコピーしましょう。
```
$ cp ./data/Sample/G4MScanBeam/EIDSample.dat  .
$ cp ./data/Sample/G4MScanBeam/WeightSample.dat  .
$ cp ./data/Sample/G4MScanBeam/ScanSample.dat  .
```

## 例題マクロファイル
PTSIMコードに付属するマクロファイル`exampleA8.mac`をコピーして用います。
```
$ cp  ./macros/tut/exampleA8.mac  .
```

実行
```
$ ./bin/PTSdemo  -i  exampleA8.mac
```

![exampleA81](../images/exampleA81.png)


陽子線を少し照射してみます。
```
Session: /run/beamOn 10
```
マグネットの磁場で陽子線の軌道が変わっているのが確認できます。

![exampleA82](../images/exampleA82.png)


終了
```
Session: exit
```

### マクロファイルの解説
解説するコマンド部分のみを抜粋して説明します。
マクロファイルでは、治療室にスキャニングマグネットを配置してビームを走査します。照射結果の線量分布を確認するために水ファントムを配置してスコアします。

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
#
# Scanning Magnets Registration
/Dynamic/Module/register ScanMagnetX G4MScanningMagnet  ./data/Sample/G4MScanningMagnet/SCMX.dat 0. 0. 2254.0 mm 
/Dynamic/Module/register ScanMagnetY G4MScanningMagnet  ./data/Sample/G4MScanningMagnet/SCMY.dat 0. 0. 1859.0 mm 
#
# Run Initialize
/run/initialize
#
# ScanBeam and Scanning Magnets
/My/PrimaryGenerator/select  ScanBeam
#
# Scanning Magnets installation
/G4M/ScanBeam/scanMagXY ScanMagnetX ScanMagnetY
/G4M/Module/install ScanMagnetX
/G4M/Module/install ScanMagnetY
#
# Beam default parameters
/gun/particle  proton
/gun/energy    150.04 MeV
/gun/position  0. 0.  3231.55 mm
/gun/direction 0.  0.  -1.
#
# Set Beam Parameter files
/G4M/ScanBeam/eidFile    ./EIDSample.dat
/G4M/ScanBeam/weightFile ./WeightSample.dat
/G4M/ScanBeam/scanFile   ./ScanSample.dat
#
#
# Phantom settings and installation
/G4M/Module/Phantom/size 150. 150. 250.0 mm
/G4M/Module/Phantom/dim 150. 150. 250.
/G4M/Module/Phantom/material G4_WATER
/G4M/Module/install Phantom
#
# Scoring
/My/runaction/dumpfile      A8.root
/My/runaction/ntuple/merge  true
#
# Track analysis
/My/runaction/ntuple/create    NT Phantom/HitsCollection 
/My/runaction/ntuple/addColumn NT ix  I
/My/runaction/ntuple/addColumn NT iy  I
/My/runaction/ntuple/addColumn NT iz  I
/My/runaction/ntuple/addColumn NT de  F  keV
/My/runaction/ntuple/showScColumn NT
#
# BeamOn
#/run/beamOn 100
#
```

#### Scanning Magnetを登録する
スキャニングマグネットのビームモジュールを登録します。
```
PreInit> /Dynamic/Module/register ScanMagnetX G4MScanningMagnet  ./data/Sample/G4MScanningMagnet/SCMX.dat 0. 0. 2254.0 mm
PreInit> /Dynamic/Module/register ScanMagnetY G4MScanningMagnet  ./data/Sample/G4MScanningMagnet/SCMY.dat 0. 0. 1859.0 mm
```

#### 初期粒子発生器としてScanBeamを選択する
```
Idle> /My/PrimaryGenerator/select  ScanBeam
```

#### X-Y方向のScanning Magnetを連携させる
２つのスキャニングマグネットが協調してビームを走査できるようにします。
```
Idle> /G4M/ScanBeam/scanMagXY ScanMagnetX ScanMagnetY
```

#### スキャニングマグネットを配置する
```
Idle> /G4M/Module/install ScanMagnetX
Idle> /G4M/Module/install ScanMagnetY
```

#### 初期粒子のデフォルト値を設定する
ビームのデフォルト値として、粒子種、発生位置、方向を設定する。
エネルギー値以外は、３つのパラメタファイルでは定義されていないので設定は必須である。
```
/gun/particle  proton
/gun/energy    150.04 MeV
/gun/position  0. 0.  3231.55 mm
/gun/direction 0.  0.  -1.
```

#### 3つのパラメタファイルを指定する

1. ビームエネルギーを定義する
```
/G4M/ScanBeam/eidFile    ./EIDSample.dat
```
`EIDSample.dat`の内容は次のようになっています。
```{code-block}
:linenos:
3
0 100.  0.  0.  0.
1 200.  0.  0.  0.
2 250.  0.  0.  0.
```
1行目がデータ数、２行目以降がビームパラメタです。ビームパラメタは、順番に、ビームID番号、ビームエネルギー\(MeV\)、エネルギー値の標準偏差\(MeV\)、X軸方向の位置の標準偏差\(mm\)、Y軸方向の位置の標準偏差\(mm\)を入力します。揺らぎは、それぞれガウス分布の標準偏差です。

2. ビーム強度を定義する
```
/G4M/ScanBeam/weightFile ./WeightSample.dat
```
同じ強度(つまり、同じ数だけ）で照射した場合、エネルギーが小さいビームは浅い位置で高いピーク線量となり、、エネルギーが大きいビームは深い位置まで届いてピーク線量は低くなります。そのため、エネルギーによらず同じ線量を与えるための照射粒子数を求めるための係数を記述します。この係数は、例題では調整前の値として1.を設定していますが、具体的な調整値としては同じ照射粒子数で照射した時のピーク線量値に相当します。
`WeightSample.dat`の記載例は次の通りです。
```{code-block}
:linenos:
3
100.    1.
200.   1.
250.   1.
```
1行目はデータ数、２行目以降が強度情報であり、順番に、ビームエネルギー\(MeV\)、強度係数です。データ点間のエネルギーが使用された場合、データ点の補間を行い強度係数が求められます。

3. スキャンするスポットを定義する
```
/G4M/ScanBeam/scanFile   ./ScanSample.dat
```
実際に照射するスポット位置を記載したファイルです。スポット位置はアイソセンター平面でのX-Y座標です。
`ScanSample.dat`の内容は次のようになっています。
```{code-block}
:linenos:
5
0  -100.  -100.  5.
1  -100.  +100.  5.
1  +100.  -100.  5.
0  +100.  +100.  5.
2     0.     0.  2.
```
1行目はデータ数、２行目以降がスポット座標です。スポット情報は、順にビームID番号(`EIDSample.dat`参照)、X座標\(mm\)、Y座標\(mm\)、付与線量強度です。付与線量強度は、`WightSample.dat`の係数を適切に設定した後であれば、同じ値のときに同じピーク線量が得られることになります。

*ラスタースキャンでも`EIDSample.dat`と`WeightSample.dat`は同じく利用可能です。
ラインスキャンを利用する場合は、初期粒子発生器の選択を変更することと、スキャンするスポットの指定方法の２つが異なります。
一連のコマンドをラインスキャン用に書き出すと次のようになります。
```
Idle> /My/PrimaryGenerator/select  LineScanBeam
Idle> /control/execute macros/DynamicPort/scanBeamBase.mac
Idle> /G4M/ScanBeam/scanMagXY ScanMagnetX ScanMagnetY
Idle> /G4M/Module/install ScanMagnetX
Idle> /G4M/Module/install ScanMagnetY
Idle> /G4M/ScanBeam/eidFile    ./EIDSample.dat
Idle> /G4M/ScanBeam/weightFile ./WeightSample.dat
Idle> /G4M/ScanBeam/scanFile   ./LineScanSample.dat
```
このときの`LineScanSample.dat`は、次のようになります。
```{code-block}
:linenos:
3
0  -100.  -100. +100 -100.  5.
1  -100.  +100. +100 +100   5.
2  -100.  -100. +100 +100   5.
```
1行目はデータ数、２行目以降がラインデータです。ラインデータは、EID番号、ラインの始点X-Y座標\(mm\)、ラインの終点X-Y座標\(mm\)、付与線量強度です。付与線量強度は、ラインの長さ全体での強度です。

ラインスキャンについては、`./macros/DynamicPort/SampleLineScan.mac`を参考にしてください。

以上


