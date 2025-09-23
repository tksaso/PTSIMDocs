# Command Reference

## コマンド引数の表記

コマンドに与える引数は、<>で囲んで表ます。  
また、その内容は、<値:型>と表記しています。  
<value:type>  
value 値  
type  型(s=string, i=int, f=float, d=double)  

例)  
/G4M/Module/select  <mname:s>  
引数として文字列型を指定します。引数名はmnameと呼びます。  

利用例）  
/G4M/Module/select  WaterPhantom  

特に以下の３つの引数名については、以降は説明なく用います。  
<mname:s>      Beam module name  
<ifilename:s>  Input file name  
<ofilename:s>  Output file name  
<unit:s>       Unit  


<!--
 ・FocusGunEdist
   ・FanBeam
     ・IAEAphsp
       ・EvtGun
         ・Root3d

 　初期粒子発生器の個々の詳細について次に説明する。
 1) ParticleGun
   Geant4のG4ParticleGunを利用する。詳細は，Geant4のマニュアルを参照すること。ここでは，代表的なコマンドを幾つか紹介する。

  粒子を設定する
  /gun/particle   <particleName:s>
   “particleName” : proton, e-, e+, gamma   etc.

  Ion beamを設定する
  /gun/particle  ion
  /gun/ion     <Z:i>   <A:i>   <Charge:i>

  運動量方向を設定する
  /gun/direction   <dirX:d>  <dirY:d>  <dirZ:d>

　発生位置を設定する
/gun/position   <x:d>  <y:d>  <z:d>  {unit:s}
2) GPS
  Geant4のG4GeneralParticleSourceを利用する。詳細は，Geant4マニュアルを参照のこと。

3) BeamGun
  BeamGunは，G4ParticleGunを使って実装されており，G4ParticleGunのコマンドが利用可能である。加えて，スポットサイズとエネルギーの揺らぎを設定することができる。

  Beam spotでの発生粒子数の分布関数を指定する
  　(Gauss distribution またはFlat distribution )
  /G4M/Beam/gaussSpot   <:b>

 スポットサイズ設定（Gauss分布であれば標準偏差値，Flatであればその範囲）
 /G4M/Beam/spotsize　　<dx:d>  <dy:d>  <dz:d>  <unit>
  Default unit:   mm

 ビームエネルギーのゆらぎをエネルギーの単位で設定する
 /G4M/Beam/energyflac　　<dE:d>  <unit>
  dE:  エネルギー揺らぎの値をガウス分布の標準偏差値で与える
  Default unit:  MeV

ビームエネルギーの揺らぎをビームエネルギーのパーセンテージで設定する
G4M/Beam/energyflacPerc　　<dEPerc:d>  <unit>
 dE:  エネルギー揺らぎの値をビームエネルギー値に対する割合（%）で与える
 Default unit:  perCent

Beam emittance (未実装)
/G4M/Beam/emittance　　<EmitX:d> <EmitY:d> <EmitZ:d>

ビームの初期角度方向の標準偏差値を与える。
/G4M/Beam/angSigma　　<SigmaX:d> <SigmaY:d>  <unit:s>
 SigmaX, SigmaY:  初期角度の値をガウス分布の標準偏差値で与える
  Default unit:  mrad
  ビームパラメタの設定をASCIIファイルから読み込む。
  /G4M/Beam/file　　<ifilename:s>

ASCIIデータファイルの書式:
./data/Sample/G4MBeamGun/150.dat	Description
150
proton 150 MeV
2212
150.0  0.00
0.0  0.0
3.0  3.0	Beam ID  [ not used. ]
Description   [ not used]
Particle ID
Beam Energy [MeV],  Energy Spread [MeV]
Angular variance, Spatial covariance [ not used]
Spatial variance in X, and in Y [mm]
 イオンの場合の粒子識別番号（Particle ID）は、+-100ZZZAAAIで定義される。

4) FocusGun
  FocusGunは，G4ParticleGunを使って実装されており，G4ParticleGunのコマンドが利用できる。加えて，焦点を持った初期粒子を発生することができる。
    ビームエネルギーのゆらぎをエネルギーの単位で設定する
    /G4M/Beam/energyfluc　<dE:d>  <Unit:s>
    dE:  エネルギー揺らぎの値をガウス分布の標準偏差値で与える
    Default unit:  MeV

 　X方向の焦点位置となるZ座標を設定する
 /G4M/Beam/FocusDistanceX　　<zX:d>　<unit:s>
  zX:  X方向の焦点座標
   Default unit:  mm

   Y方向の焦点位置となるZ座用を設定する
   /G4M/Beam/FocusDistanceY　　<zY:d>　<unit:s>
   zY:  Y方向の焦点座標
    Default unit:  mm

  初期粒子発生座標と焦点位置が同じ場合は自動計算不能のため，発生する粒子の運動量方向を与える。
  /G4M/Beam/SpatialExpanseX  <ExpanseX:d>  <unit:d>
  /G4M/Beam/SpatialExpanseY  <ExpanseY:d>  <unit:d>
  　Expanse X, Expanse Y:  角度の単位で値をガウス分布の標準偏差値で与える。
   Default unit:  mrad

 ビームパラメタの設定をASCIIファイルから読み込む。
 /G4M/Beam/file　　<filename:s>
 　ファイル形式は， G4MBeamGunと同様である。加えてG4MFocusGunに特有な設定は，コマンドで設定しなければならない。

5) FocusGunEdist
 　G4MFocusGunの機能に加えて, G4MFocusGunEdistでは，ビームのエネルギー分布を設定することが可能である。つまり，G4MFocusGunのエネルギー設定以外のコマンドが利用できる。更に，次に示すように，エネルギーと強度を入力して分布を設定することができる。

 エネルギー分布の設定をクリアする
 /G4M/Beam/clear

エネルギー値とその強度を設定する
/G4M/Beam/addEnergy　 <Energy(MeV unit):d>  <Intensity:d>
 Energy:  エネルギー値
  Intensity:  強度

*エネルギーは MeV単位で与えること。
このコマンドを順次，エネルギーを変えて入力してエネルギーと強度分布のデータを設定する。
 与えたエネルギーの中間値は，線形補間される。あらかじめ，線形補間を行った分布を用意することができる。下のコマンドは，補間するデータ点の数を指定する。
 /G4M/Beam/sample　　<n:i>
   n:  Number of points
   6) FanBeam
     G4ParticleGunを利用して実装されており，G4ParticleGunのコマンドが利用可能である。加えて，扇型の方向に粒子を発生する機能がある。

(0, r, 0)を生成点とすることを前提として，そのY座標rを設定する。
/G4M/FanBeam/radius　　<r:d>  <unit:s>
 r:  生成点のY座標
  unit:  mm etc.

 扇形の空間分布関数を指定する。
 /G4M/FanBeam/gaussSpot　　<flag:b>
   flag:  true	 ガウス分布
   false   一様分布

 ビーム発生のZ座標を設定する。（未実装）
 /G4M/FanBeam/z　　<z:d> <unit:s>

  生成する扇型の角度を設定する。（未実装）
  /G4M/FanBeam/theta　　<theta:d>  <unit:s>

　生成する扇型の角度を設定する。
/G4M/FanBeam/phi  <pho:d>  <unit:s>
 phi:  角度値。ガウス分布の標準偏差。
  unit:  deg, rad etc.

  エネルギー揺らぎを設定する。
  /G4M/Beam/energyfluc  <energyflac:d>  <unit:s>
   energyflac:  エネルギー揺らぎ値をガウス分布の標準偏差で与える
    unit:  MeV

 
7) IAEAphsp
  IAEAphsp形式のデータの読み込み，初期粒子を発生するためのコマンド。

IAEA phspのためのメンバ変数を初期化する
/G4M/IAEAphsp/init

  読み込むIAEA phspのデータファイルを設定する。
  /G4M/IAEAphsp/file　　<fileBase:s>  <fileStopID:s>
  　fileBase:  　Base file name
  　fileStopID:  ID of the file name
    fileStopIDは，同じRunで異なるZ座標で記録されたデータの場合の識別子である。

  IAEA phsp データの読み込みを終了してファイルを閉じる。
  /G4M/IAEAphsp/close

  デバッグレベルを設定する。
  /G4M/IAEAphsp/verbose　　<v:i>
  v: debug level

  リサイクルフラグ。１つの粒子データを指定した回数再利用する
  /G4M/IAEAphsp/recycle  <nrecycle:i>
  nrecycle: number of recycle times.

  並列処理の場合に, データファイルを複数の論理的なセグメントに分割して，それぞれの並列処理に割り当てる。
  /G4M/IAEAphsp/totalParaRun　　<npara:i>
   npara:  Parallel runの個数を設定する　

  並列処理でのRun識別番号を設定する。
  /G4M/IAEAphsp/paraRunId <runid:i>
   runid:  Runの識別番号

ヒストリ情報を含まないデータの場合、１イベントで実行する粒子数を指定する。
/G4M/IAEAphsp/nparticlesPerEvent <np:i>
 np:  Number of particles simulated in one event.
  (Number of beamOn need to be estimated as Number of total particles in header file divided by np.)
  　
    記録された情報の発生座標を変更する。(mm)
    /G4M/IAEAphsp/translate　<x:d>  <y:d>  <z:d>
     x, y, z:  発生点の移動量
      単位は，mmで固定。

回転の座標変換を行うときの，各座標軸周りでの回転の順番を設定する。
/G4M/IAEAphsp/rotateOrder　　<iorder:i>
iorder:  123 (X->Y->Z),  213 (Y->X->Z)

  回転角度を設定する。
  /G4M/IAEAphsp/rotateX　　<angle:d>  <unit>
  /G4M/IAEAphsp/rotateY　　<angle:d>  <unit>
  /G4M/IAEAphsp/rotateZ　　<angle:d>  <unit>
   angle:  角度
    unit:  mrad etc.

  等方的な放射線発生のデータにおいて，アイソセンターの位置を設定する。
  /G4M/IAEAphsp/isoPos　<x:d>  <y:d>  <z:d>  <unit:s>
   x, y, z:  アイソセンターの座標
    unit:  mm etc.

 ガントリー角度とその回転軸を設定する。
 /G4M/IAEAphsp/gantryAxis　　<xdir:d> <ydir:d>  <zdir:d>
 /G4M/IAEAphsp/gantryAngle　　<angle>  <unit:s>
 xdir, ydir, zdir:  回転軸
 angle:  回転角
 unit:  deg etc.
 トリートメントヘッドの回転とその回転軸を設定する。
 /G4M/IAEAphsp/headAxis　<xdir:d> <ydir:d> <zdir:d>
 /G4M/IAEAphsp/headAngle　　<angle:d> <unit:s>
 xdir, ydir, zdir:  回転軸
 angle:  回転角
 unit:  deg etc.

8) ScanBeam
　スポットスキャニングを行う。３つのASCIIファイル入力が必要である。
 デバッグモードの設定
 /G4M/ScanBeam/verbose  {v:i}
  v:  デバッグフラグ

Scanning Magnetの登録と有効化
事前にScanning Magnetがビームモジュールとして登録済みである必要がある。
/G4M/ScanBeam/scanMagXY   {mnameX:s}   {mnameY:s}
 mnameX, mnameY:  ビームモジュール名

ビームエネルギーとその揺らぎ，そしてスポットサイズを記載したASCIIファイル（eidファイルと呼ぶ）を読み込み，設定を行うためのコマンド。
/G4M/ScanBeam/eidFile   {filename:s}
 eidFileName:  eidファイル名

 eidファイルの書式
 data/Sample/G4MScanBeam/EIDSample.dat	Description
 3
 0 100.  0.  0.  0.
 1 200.  0.  0.  0.
 2 250.  0.  0.  0.	Number of lines (e.g. number of beams)
 ID  E(MeV)  dE(MeV) SigX(mm)  SigY(mm)
 *ID must be given in Sequential.


eidファイルを与えるもうひとつのコマンド。
こちらのコマンドの場合には，エネルギー，エネルギー揺らぎ，スポットに加えて，初期角度を各BeamID毎に定義したASCIIデータファイルを読み込む。
/G4M/ScanBeam/eidFile2   {filename:s}
 eid2FileName:  eid2ファイル名

eidファイルの書式:
data/Sample/G4MScanBeam/EIDSample2.dat	Description
3
0 100.  0.  0.  0.　0. 0.
1 200.  0.  0.  0. 0. 0.
2 250.  0.  0.  0. 0. 0.	Number of lines (e.g. number of beams)
ID  E(MeV)  dE(MeV) SigX(mm)  SigY(mm) AngSigX(mrad) AngSigY(mrad)
*ID must be given in Sequential.

スポット座標と線量強度を与えるASCIIファイル(scanファイル)を指定する。スポット座標は，アイソセンター面でのx,y座標である。
/G4M/ScanBeam/scanFile   {filename:s}
 scanFileName:  scanファイル名

　scanファイルの書式
data/Sample/G4MScanBeam/ScanSample.dat	Description
5
0  -100.  -30.  5.
1     0.   30.  2.
1    50.  -50.  2.
2  -100. -100.  1.
2    10. -100.  2.	Number of lines (e.g. Number of spots)
ID  x(mm)  y(mm)  Gy


異なるエネルギーでも，同じピーク線量値とするように初期粒子数を調節するための係数(weight)を記載したASCIIファイルを指定する。
/G4M/ScanBeam/weightFile   {filename:s}
 weightFileName:  weightファイル名
  weightファイルの書式
  data/Sample/G4MScanBeam/WeightSample.dat	Description
  3
  90.    1.
  200.   1.
  300.   1.	Number of lines
  E(MeV)  Weight
  異なるエネルギーだが，同じ粒子数の計算を行ったときに得られた線量の相対値を表す。

設定を表示する。
/G4M/ScanBeam/show  {type:s}
type: eid,  spot,  weight
(*) 個々のスポットの粒子発生数を表す確率は/run/beamOnが実行されたときに，自動的に計算される。

　デバッグの目的で，/run/beamOnの前に粒子発生数を計算する。
/G4M/ScanBeam/calcProb

9) EvtGun
   G4MDiskDumperで保存されたトラック情報から粒子を発生する。beamOnで指定するイベント数は、トラック情報を保存した時と同じ数にすること。
   トラック情報のファイルを開く。
   /G4M/EvtIF/open    <filename:s>　<format:s>
   format:  ASCII (defaut) or BINARY

トラック情報のファイルを閉じる
/G4M/EvtIF/close
デバッグフラグ
/G4M/EvtIF/verbose  <level:i>
出力フォーマット
/G4M/EvtIF/format  <ASCII or  Binary:s>

10) Root3d
   CERN ROOTのTH3形式で保存された３次元座標分布に従って粒子を発生する。粒子の位置座標が適用され、運動エネルギーはゼロに設定される。放射性同位体の発生分布を与えて崩壊後のシミュレーションを行うことを想定している。この機能を利用するためには、cmakeの際に -DPRIMROOT=ON を指定する必要がある。
   デバッグフラグ
   /G4M/Root3d/versbose    <lvl:i>　
   ファイルとTH3の指定
   /G4M/Root3d/file   <RootFileName:s>  <TH3Name:s>
   ROOTのファイル名と記録されているTH3型３次元ヒストグラム名を指定する。ここで、TH3型ヒストグラムの各軸のスケールはミリメートルであるとする。また、粒子生成の際には、TH3::Random3を利用しているため、ビン内の座標値は一様乱数で決定される。頻度は自動的に規格化されて計算される。TH3の作成は、サンプルROOTマクロTH3OSample.Cが添付されているので参照のこと。
   粒子を設定する
   /G4M/Root3d/particle  <particleName:s>     // デバッグ用である
   /G4M/Root3d/ion   <Z:i>  <A:i>  <Charge:i>
   1イベントに発生する粒子数
   /g4M/Root3d/nparticlesPerEvent   <n:i>
   発生点の座標を移動する場合のオフセット値（この値が座標に足し算される）
   /g4M/Root3d/vtxOffset  <ox:d>  <oy:d>  <oz:d>  <unit:s>

(補足)
　Geant4-v11.2より、デフォルト設定では、放射性同位体の崩壊時刻が長いものは発生させない設定になっている。これまで通り、崩壊時刻によらず壊変させたい場合には、次のコマンドを用いる。
/process/had/rdm/thresholdForVeryLongDecayTime 　1.0e+60 year

 
II) Physics List
 物理コンストラクタを登録する
 /My/physics/register　<physName:s>
 physName:  物理コンストラクタの名称

指定された領域にsteplimiterを設定する
/My/physics/stepLimitForRegion　<Region:s>  <stepsize:d>  <unit>
Region:  リージョン名
stepsize:  ステップリミットを行うステップ長
default unit : mm

 パラレルワールド用のプロセスを定義する
 /My/physics/pwProcess　<pwname:s> <layered:b> <ProcessName:s> <verbose:d>
  pwname:  パラレルワールド名
   layered:   true (Layered geometry )
    ProcessName: このプロセスに付けるプロセス名

 
III) System Geometry
システムを選択する
/G4M/System　　<system_name:s>
Systemname:  “DynamicPort”とすること

（システムを切り替える）
/G4M/ChangeSystem　<system_name:s>

 PTS toolkit versionを表示する
 /G4M/PTS/TKVersion　<ofilename:s>
 　　default ofilename:  stdout

Gesnt4 versionを表示する
/G4M/PTS/G4Version　<ofilename:s>
　　default ofilename:  stdout

PTS application version numberを表示する
/G4M/PTS/AppVersion　<ofilename:s>
　　default ofilename:  stdout

 Run summaryを表示する (未実装)
 /G4M/PTS/RunSummary　<ofilename:s>
 　　default ofilename:  stdout

 
IV) Material Definition
Elementデータファイルのパスを指定する
/G4M/Element/path　　<path:s>

物質データファイルのパスを指定する
/G4M/Material/path　　<path:s>

NIST databaseで定義されているElementを利用する
/G4M/Element/useG4Element　　<flag:b>
 flag:  true (NISTのElementを用いる)
 　　   false (ユーザー定義のElementを用いる)

物質データファイルを読み込んで物質を作成する
/G4M/Material/create  <ifilename:s>
 ifilename:  物質のパラメタファイル

物質情報を表示する
/G4M/Material/property  <matname:s>
 matname:  マテリアル名

マテリアルファイル例
　1)エレメントによる構成例：　　./data/common/material/H_2O.dat
  2)物質による構成例: ./data/common/material/G10.dat

 
V) Beam module Registration
ビーム機器の登録
/Dynamic/Module/register　<mname:s>  <mtype:s>  <param:s>
                         <x:d>  <y:d>  <z:d> <unit>
			                          <rx:d>  <ry:d> <rz:d> <unit>
						  mtype:  ビームモジュールのクラス名
						   para:  形状データファイル名
						    x, y, z: 配置座標と単位
						     rx, ry, rz, unit:  回転角と単位

 ビーム機器の登録解除
 /Dynamic/Module/unregister <mname:s>

 水ファントムの登録/解除
 /Dynamic/Module/WaterPhantom/register
 /Dynamic/Module/WaterPhantom/unregister

DICOMジオメトリの登録/解除
/Dynamic/Module/DICOM/register
/Dynamic/Module/DICOM/unregister

ワブラー電磁石を有効にする
/Dynamic/Module/updateEvent/wobbling <wnamex:s> <wnamey:s>  <flag:b>
 wnamex, wnamey:  ビームモジュール名
  flag:  true=有効，false=無効

回転モジュレータを有効にする
/Dynamic/Module/updateEvent/rotating <mname:s>  <flag:b>
flag:  true=有効，false=無効

回転モジュレータ / ワブラー電磁石の更新を，イベント番号毎に順次更新するか乱数による更新を行うか設定する。
/Dynamic/Module/updateEvent/sequential <flag:b>
flag:  true=順次，false=乱数

イベントに従う順次更新の場合の，回転モジュレータの初期角度と更新角度を設定
/Dynamic/Module/updateEvent/rotatingAngle <init:d> <step:d> <unit>
init:  初期角度
 step:  追加する角度
  unit:  rad etc.

 
VI) Beam module Operation
ビームモジュールの実体化・治療室への配置
/G4M/Module/install　<mname:s>  <pwname:s>  <layered:b>
　　pwname:  デフォルト値　none  (マスジオメトリへの配置)
    layered : デフォルト値　false (Layeredジオメトリではない)

既にインストールされているビームモジュールの場合には，引数の設定を基づいて再インストールする。インストールされていないビームモジュールであれば、インストールは行わない。
このコマンドは，mass worldにインストールされたモジュールを，パラレルワールドに再配置するために用意されている。
/G4M/Module/installIfExist <mname:s>  <pwName:s>  <layered:b>
　　pwname:  デフォルト値　none  (マスジオメトリへの配置)
    layered : デフォルト値　false (Layeredジオメトリではない)

  既にインストールされているビームモジュールがあるとき、引数の設定に基づいてコピーを作成してインストールする。
  　このコマンドは、ビームモジュールのエンベロープにコピー番号を付けて複数配置するために用意されている。
  /G4M/Module/cloneIfExist  <mnameFrom:s> <mnameTo:s> <copyid:i>
                                 <x:d> <y:d> <z:d> <unit:s>
				                                 <rx:d> <ry:d> <rz:d> <unit:s>
								   mnameFrom: コピー元の実体化されているビームモジュール名
								   　mnameTo:   コピー先のビームモジュール名
								   　copyid:    コピー先のビームモジュールのエンベロープのコピー番号
								   　x, y, z, unit: コピー先の座標位置
								   　rx.ry.rz unit: コピー先での回転

ビームモジュールの実体削除（ジオメトリの削除）
/G4M/Module/uninstall　<mname:s>

登録されたビーム機器すべてのインストール/アンインストール
/G4M/Module/installAll
/G4M/Module/uninstallAll

水ファントムとDICOMを除いた登録されたビーム機器すべてのインストール/アンインストール
/G4M/Module/installBM
/G4M/Module/uninstallBM

ビーム機器を再構築する（ジオメトリが存在する場合、まずジオメトリを削除して作り直す。ジオメトリが存在しない場合、カタログパラメタだけを更新する）
/G4M/Module/rebuild　　<mname:s>

ビーム機器の一覧と状態を表示する
/G4M/Module/list  <lvl:i>  <ofilename:s>
  lvl:  1 (default)
    ofilename: stdout 又は，ファイル名

World Volumeに配置されたビーム機器の一覧を表示する
/G4M/Module/listInWorld

dump commandでの表示モードを設定する
/G4M/Module/lang   <type:i>
   Default type: 0  (Comments in Japanese)
               type: 1  (Comments in English)
	                   type: 10 (dcmodify command shell script)

ビーム機器を選択するためのコマンド。
/G4M/Module/select  <mname:s>
 選択されたビームモジュールに対して，次に紹介するコマンドが適用される。

ビーム機器情報を表示する
/G4M/Module/dump
/G4M/Module/dumpToFile   <ofilename:s>
ofilename:  stdout またはファイル名

選択されたビーム機器の形式パラメタを更新する
/G4M/Module/typeid　　　<ifilename:s>
 ifilename:  形状ファイル

ビーム機器の中心で位置設定をする
/G4M/Module/translate  <X:d>  <Y:d>  <Z:d>  <unit>
X, Y, Z:  x, y, z座標
default unit: mm

ビーム機器の下流z面で位置設定する
/G4M/Module/downEdgeZ   <z:d>  <unit>
  z:  z座標
  unit: mm etc.

ビーム機器の上流z面で位置設定する
/G4M/Module/upEdgeZ   <z:d>  <unit>
  z:  z座標
  unit: mm etc.

ビーム機器のX軸、Y軸、Z軸周りの回転角を指定する
/G4M/Module/rotate  <Rx:d>  <Ry:d>  <Rz:d>  <unit>
Rx, Ry, Rz:  回転角度
default unit :  deg.

DICOMジオメトリの下流側に指定したモジュールを取り付ける。
このコマンドは、カウチジオメトリをDICOMジオメトリに取り付けるためのコマンドとして用いられる。
/G4M/Module/attachZ　　<mname:s>

選択されたビーム機器でのデバックフラグ
/G4M/Module/verbose  <lvl:i>
 lvl:  デバッグフラグ

ビーム機器のLogical Volume名の一覧を表示する。
/G4M/Module/dumpLV <mname:s>
 論理ボリュームごとに順序番号(lvid)が付けられて表示される。

Logical Volume名とモジュール内での順序番号を指定して、論理ボリュームを選択する。
/G4M/Module/selectLV　 <mname:s>   <lvid:i>
lvid:　論理ボリュームの順序番号。dumpLVコマンドで表示される。

*WaterPhantom、CylinderPhantom、 DICOM、PCTTubeDetector, PCTBoxDetectorジオメトリでは、SDは自動的に取り付けられている。下記のコマンドcreateSDやattachSDは、それら以外のジオメトリでのスコアリングを想定して用意されている。

SensitiveDetector(DetectorSD)を作成する。
/G4M/Module/createSD <sdname:s> <colname:s> <edepFlag:b>
                          <depx:i> <depy:i> <depz:i> <depm:i> <deps:i>
			   sdname:  SD name
			     colname: collection name
			        edepFlag: trueのときは付与エネルギーがある場合のみ記録する。ガンマなどは、反応時のみ記録される。
				　　depx, depy, depz, depm, deps: ジオメトリ識別番号を取得する階層

selectLVコマンドで選択中の論理ボリュームに、SensitiveDetectorを取り付ける。
/G4M/Module/attachSD  <sdname:s>
selectLVコマンドで選択中の論理ボリュームに、TriggerSDを取り付ける。

/G4M/Module/attachTrgID    {tirgid:i}  {sdname:s}
trigid: 選択中の論理ボリュームに割り振るトリガー識別番号
sdname:  TriggerSDの固有名(デフォルト”TreiggerSD”)
SD名での登録の有無でTriggerSDの存在を識別し、未作成の場合には自動的に生成される。原則としてTriggerSDは、１つしか作ってはいけないので注意。

selectLVコマンドで選択中の論理ボリュームの物質を変更する。
/G4M/Module/setMaterial    {matname:s}
matname: 選択中の論理ボリュームに割り当てる物質名。既に定義されている必要がある。パラレルワールドでのレイヤジオメトリでは、”null”または”NULL”で物質をヌルに設定することができる。

物理に関する更新を手動で行う。
/G4M/Module/physicsModified

GDMLの利用
　GDMLジオメトリを利用する場合には、XML用のライブラリ(xercesなど)がインストールされており、Geant4ライブライも-DGEANT4_USE_GDML=ONのオプションの元でコンパイルされていなければならない。更に、GDMLPTSToolkitならびにDynamicPortの双方を-DUSEGDMLのオプションをつけてビルドしていなければならない。

ビームモジュールをGDML形式のジオメトリファイルに書き出す。ファイル名はモジュール名となる。
/Dynamic/Module/gdml/write  {mname:s}  {bSD:b}  {bRefPointer:b}
                                 {filepath:s} {schemaLocation:s}
				 bSD: SDをGDMLに記載する。 (Default=true)
				 bRefPointer: 各部品に重複を避けるハッシュコードをつける。(Default=true)
				 filepath: GDMLの出力ファイルパス (Default = ./ )
				 shemaLocation: (Default = ./data/schema/gdml.xsd)

GDML形式のジオメトリファイルからビームモジュールを構築する。
G4MBMGdmlビームモジュールとして登録する。
/Dynamic/Module/register  {mname:s}  G4MBMGdml  {gdmlFile:s}
                               {x:d} {y:d} {z:d} {unit:s}
			                                      {rx:d} {ry:d} {rz:d} {unit:s}
							      gdmlFile: GDML形式のジオメトリファイル
							      x,y,z, unit:  中心位置とその単位
							      rx,ry,rz, unit: 回転角とその単位

GDMLジオメトリ構築時に、GDMLファイルに記載されている物質定義をスキップし、既存の物質定義を適用する。
/Dynamic/Module/gdml/reader/skipMaterials   {flag:b}
flag:  true=スキップして無視する,  false=読み込んで物質を作成する。

ジオメトリスキャン
ジオメトリのボクセル化/ParaView可視化用VTK Legacy formatでの出力する。
指定するビームモジュールまたは、指定する空間領域を分割して、各点の質量密度を取得してボクセルジオメトリデータとして出力する。出力データ形式は、VTK Legacy formatである。

ビームモジュール名を指定して空間領域をボクセルデータで出力する。
/Dynamic/Module/geomScan   {mname:s}  {nx:i} {ny:i} {nz:i}
                                {binaryFlag:b}
				nx, ny, nz: 分割数
				binaryFlag: 出力データ型（Default = false, ASCII形式）
				出力ファイルは、mname.vtk

空間領域を指定してボクセル化する場合
(a)空間領域を指定する。
/Dynamic/GeomScan/setVolume  {nx:i} {xlow:d} {xup:d}
                                   {ny:i} {ylow:d}  {yup:d}
				                                      {nz:i} {zlow:d} {zup:d}
								                                         {unit:s}
													 nx, ny, nz:  分割数
													 xlow, ylow, zlow: 座標値
													 xup, yup, zup: 座標値
													 unit: 座標値の単位
													 (b)設定した空間領域をスキャンしてファイル出力する。
													 /Dynamic/GeomScan/voxelScan   {filename:s}
													 {binaryFlag:b}
													 {byteswap:b}
													 filename:  出力ファイル名
													 binaryFlag: default=false
													 byteswap:   default=false

 
VII) 特殊機能を持つビーム機器

1)G4MBoxField, G4DiskField
空間磁場を設定するためのモジュールとなっており、サイズはG4MBoxやG4MDiskと同じ方法で設定する。
磁場設定コマンド
以下<module_name>は、空間磁場のビームモジュール名を表す。
/G4M/Module/<module_name>/field  <Bx:d> <By:d> <Bz:d>  <unit:s>
Bx, By, Bz:  磁場値
 unit:  T etc.

2)G4ScanningMagnet
電磁石のモジュールへの磁場値を設定する。
＊磁場値は、デフォルトでは電磁石の位置と磁場の長さ、粒子エネルギーと指定されたアイソセンター面での座標から、近似式で計算されて用いられる。近似式の補正を行うために、(1)手動で磁場値を設定するコマンド、(2)磁場計算の近似式に補正係数を設定するコマンドが用意されている。これらのコマンドは、各電磁石において用いる。

(2-1)手動で磁場値を設定するコマンド
以下<module_name>は、電磁石のビームモジュール名を表す。
/G4M/Module/<module_name>/fiedvalue  <field_value:d>  <unit:s>
field_value:  磁場値
 unit:  T etc.

アイソセンターから電磁石モジュールまでの距離を設定する。この距離がスキャニングのための磁場値を計算する際に用いられる。
/G4M/Module/<module_name>/distance  <dist:d>  <unit:s>
 dist: 距離
  unit:  m etc.

次の2つのコマンドで、スポットの位置とビームの運動エネルギーの入力データ点を手動で指定する
アイソセンター面でのスポット位置を与える
/G4M/Module/<module_name>/putXFieldMap <idx:i>  <xval:d> <unit:s>
idx:  インデックス番号
xval:  スポットの位置（座標）
unit:  mm etc.

ビームの運動エネルギーを与える
/G4M/Module/<module_name>/putYFieldMap <idy:i>  <yval:d> <unit:s>
idy:  インデックス番号
yval:  ビームの運動エネルギー
unit:  MeV etc.

idx, idyで指定したスポット位置とビーム運動エネルギーでの磁場値を指定する
/G4M/Module/<module_name>/putVaueFieldMap <idx:i> <idy:i> <value:d> <unit:s>
idx:  スポット位置のインデックス番号
idy:  ビームの運動エネルギーのインデックス番号
value:  磁場値
unit:  T etc.

磁場の極性を設定する。
/G4M/Module/<module_name>/sign  <sign:i>
 sign:  +1 又は -1

アイソセンター面でのスポットの位置とビームの運動エネルギーで関連付けされる磁場値の表を作成する
/G4M/Module/<module_name>/createFieldMap  <interpolation:b>
interpolation:  データ間を補間（true）

磁場値の対応表を削除する
/G4M/Module/<module_name>/deleteFieldMap

磁場値をASCIIファイルに保存する
/G4M/Module/<module_name>/storeFieldMap  <file_name:s>
file_name:  出力ファイル名

ASCIIファイルから磁場値の表を読み込む
/G4M/Module/<module_name>/retrieveFieldMap  <file_name:s>

(2-2)磁場計算の近似式に補正係数を設定するコマンド
磁場の補正係数表のスポット位置を与える
/G4M/Module/<module_name>/putXFieldCoeff <idx:i>  <xval:d> <unit:s>
idx:  インデックス番号,　　xval:  スポット位置
unit:  mm etc.

磁場補正係数表のビームの運動エネルギー値を与える
/G4M/Module/<module_name>/putYFieldCoeff <idy:i>  <yval:d> <unit:s>
idy:  インデックス番号
yval:  ビームの運動エネルギー
unit:  MeV etc.

磁場補正係数表のidx, idyに対応する補正係数を与える
/G4M/Module/<module_name>/putVaueFieldCoeff <idx:i> <idy:i> <value:d>
idx:  スポット位置のインデックス番号
idy:  ビームの運動エネルギーのインデックス番号
value:  補正係数値

スポット位置とビームの運動エネルギーに対応する磁場値への補正係数表を作成する
/G4M/Module/<module_name>/createFieldCoeff  <interpolation:b>
interpolation:  true 補間

磁場値の補正係数表を削除する
/G4M/Module/<module_name>/deleteFieldCoeff


ASCIIファイルに磁場補正係数表を保存する
/G4M/Module/<module_name>/storeFieldCoeff  <file_name:s>
file_name:  出力ファイル名

ASCIIファイルから磁場補正係数表を読み込む。
/G4M/Module/<module_name>/retrieveFieldCoeff  <file_name:s>
file_name:  ファイル名

3)WaterPhantom
 　WaterPhantomの次に示すコマンド群においては、<module_name>の部分は、ビームモジュール名をWaterPhantomと指定した場合、次のようにPhantomと置き換えたコマンド・パスとなる。一方、ビームモジュール登録時に、異なるビームモジュール名を付けた場合には、登録時のビームモジュール名が使われる。(104-002-000以降)
 　例）　モジュール名　WaterPhantomの場合
 　　　　/G4M/Module/Phantom/size  ....
   例）　モジュール名　WPの場合
   　　　　/G4M/Module/WP/size  ....

水ファントムのハーフサイズを指定する
/G4M/Module/<module_name>/size　 <dX:d>  <dY:d> <dZ:d> <unit>
　　dX, dY, dZ:  ハーフサイズ
default unit : mm

水ファントム内のスコアリング領域のハーフサイズを指定する
/G4M/Module/<module_name>/sdsize <dxSD:d> <dySD:d> <dzSD:d> <unit>
　　dxSD, dySD, dzSD:  ハーフサイズ
default unit : mm

水ファントム内のスコアリング領域の配置を設定する
/G4M/Module/<module_name>/sdoffset　 <x:d>  <y:d> <z:d> <unit>
　　x, y, z:  水ファントム内座標での位置
default unit : mm
スコアリング領域のセグメント数
/G4M/Module/<module_name>/dim　 <Nx:d>  <Ny:d> <Nz:d>
 Nx, Ny, Nz:  セグメント数

ファントムの物質を指定する
/G4M/Module/<module_name>/material  <matname:s>
matname:  物質名

エネルギー付与を与えたトラックのみスコアリングするか、全てのトラックをスコアするかを設定
/G4M/Module/<module_name>/edep  <flag:b>
    flag = true: エネルギー付与があるときのみ記録
        flag = false: エネルギー付与がなしでの記録

4)CylinderPhantom
 <module_name>部分は、登録時のビームモジュール名が使われる。
 　例）　モジュール名　CYLの場合
 　　　　/G4M/Module/SYL/size  ....
   例）　モジュール名　WPの場合
   　　　　/G4M/Module/WP/size  ....

ファントムのハーフサイズを指定する
/G4M/Module/<module_name>/size　 <R:d>  <Phi:d> <dZ:d>
                                       <R_unit> <Phi_unit> <dZ_unit>
				       　　R, Phi, dZ:  半径、角度範囲、長さ（dZはハーフサイズ）
				       default: R_unit (mm), Phi_unit (degree), dZ_unit (mm).

ファントム内のスコアリング領域のハーフサイズを指定する
/G4M/Module/<module_name>/sdsize <RSD:d> <PhiSD:d> <dzSD:d>
                                       <R_unit> <Phi_unit> <dZ_unit>
				       　　RSD, PhiSD, dzSD:
				       default unit : R_unit (mm), Phi_unit (degree), dZ_unit (mm).
				       ファントム内のスコアリング領域の配置を設定する
				       /G4M/Module/<module_name>/sdoffset　 <r:d>  <phi:d> <z:d>
				                                                  <R_unit> <Phi_unit> <dZ_unit>
										  　　r, phi, z:  ファントム内座標でのオフセット位置
										  default unit : R_unit (mm), Phi_unit (degree), dZ_unit (mm).

スコアリング領域のセグメント数
/G4M/Module/Phantom/dim　 <Nx:d>  <Ny:d> <Nz:d>
 Nr, Nphi, Nz:  セグメント数

ファントムの物質を指定する
/G4M/Module/<module_name>/material  <matname:s>
matname:  物質名

エネルギー付与を与えたトラックのみスコアリングするか、全てのトラックをスコアするかを設定
/G4M/Module/<module_name>/edep  <flag:b>
    flag = true: エネルギー付与があるときのみ記録
        flag = false: エネルギー付与がなしでの記録

5)DICOM geometry
DICOMジオメトリを選択する
/G4M/DICOM/select   <mname:s>
mname:   DICOM

/G4M/DICOM/file 　<ifilename:s>
 ifilename:  CT画像ファイルの一覧を記述したASCIIファイル

CT座標系でのアイソセンターを指定する
/G4M/DICOM/isocente　　<x:d>  <y:d>  <z:d>  <unit>
 x, y, z:  座標値， unit:  mm etc.

CTデータで欠けているスライスを補償する
/G4M/DICOM/complement　 <flag:b>
 flag:  true（補間する）

ボクセルサイズを設定する
/G4M/DICOM/mesh  <size:d>  <unit>
   size:  立方体の辺の長さ
   default unit :  mm

CTのピクセルをxy方向で結合する。
G4M/DICOM/mesh/bin  <n:i>
n:  結合ピクセル数

有効な画像領域を設定し、画像領域外のボクセルの物質を空気に置き換える
（トリミングの追加コマンドがあれば、設定した外側は削除される。）
/G4M/DICOM/winXmin　　<min:d>  <unit>
/G4M/DICOM/winXmax　　<max:d>  <unit>
/G4M/DICOM/winYmin　　<min:d>  <unit>
/G4M/DICOM/winYmax　　<max:d>  <unit>
/G4M/DICOM/winZmin　　<min:d>  <unit>
/G4M/DICOM/winZmax　　<max:d>  <unit>
min, max:  最小値、最大値,    unit:  mm etc.

空気のHU値を設定する。このHU値は有効な画像領域が指定されたとき、その領域の外側のHU値として利用される。
/G4M/DICOM/ctair <ct:double>
 ct:  空気のHU値

有効な画像領域をトリミングするためのフラグを設定する
/G4M/DICOM/trim　　<flag:b>
 flag:  true　トリミング実効

 HU値の範囲を設定する。
 範囲外のHU値は指定された最小値または最大値に値が丸められる
 /G4M/DICOM/minvalue　　<minvalue:d>
 /G4M/DICOM/maxvalue　　<maxvalue:d>
 minvalue, maxvalue:  最小、最大のHU値

ラベリングアルゴリズムによって患者ジオメトリの輪郭抽出をするためのフラグ
/G4M/DICOM/label　　<flag:b>
flag: true　輪郭抽出を実行

輪郭抽出を実行する際のHU値の閾値
/G4M/DICOM/cutoff　<ct:d>
 ct:  HU値の閾値

HU-密度変換表ファイルを指定する
/G4M/DICOM/ct2density 　<ifilename:s>

CT2Densityファイルの書式:
data/Sample/Dicom/CT2Density.dat	Description
10
-5000   0.0
-1000   1.21e-3
-98     0.93
-97     0.930486
14      1.03
23      1.031
100     1.119900
101     1.076200
1600    1.964200
3000    2.8	Number of lines
{HU}   {Density in g/cm3}
...



HU値の範囲と物質との対応表ファイルを指定する
/G4M/DICOM/paramtype   <paramtype_ifile:s>

paramtypeファイルの書式:
data/Sample/Dicom/CT2Water.dat	Description
1
-2048   4096    H_2O    1.00	Number of lines
{HU_L}  {HU_H} {Mat} {Corr.}

data/Sample/Dicom/CT2Tissue.dat	Description
27
-2048	-951	Group01	1.051
-950	-121	Group02	0.977
-120	-83	Group03	0.948
-82	-53	Group04	0.958
-52	-23	Group05	0.968
-22	7	Group06	0.976
8	18	Group07	0.983
19	79	Group08	0.993
80	119	Group09	0.971
120	199	Group10	1.002
200	299	Group11	1.005
300	399	Group12	1.010
400	499	Group13	1.014
500	599	Group14	1.018
600	699	Group15	1.021
700	799	Group16	1.025
800	899	Group17	1.030
900	999	Group18	1.033
1000	1099	Group19	1.035
1100	1199	Group20	1.038
1200	1299	Group21	1.041
1300	1399	Group22	1.043
1400	1499	Group23	1.046
1500	1599	Group24	1.048
1600	1999	Group25	1.042
2000	3060	Group26	1.049
3061	4096	Group27	1.000	Number of lines
{HU_L} {HU_H} {Mat} {Corr.}

物質を低減するためのHU値のサンプリングを設定する
/G4M/DICOM/ctsamp  <bit:i>
bit:  HU値の間引き数

HU値と密度g/cm3の対応表での密度の分解能を決定する
/G4M/DICOM/densityResol  <reso:d>
 reso:  密度の分解能（例　0.01）

ガントリー角を指定する
/G4M/DICOM/gantry  <angle:d>  <unit>
   angle:  ガントリー角度
   default unit:  deg

カウチ角を指定する
/G4M/DICOM/couchAngle  <angle:d>  <unit>
  angle:  角度
  default unit: deg

CTデータのデータオリエンテーションを設定する
/G4M/DICOM/dataOrientation　 <type:s>
    type:  NONE.  HFS,   FSS, HFP, FFP

DICOMジオメトリのためのパラメタを更新する
/G4M/DICOM/update


ボクセルの可視化フラグを設定する
/G4M/DICOM/vis   <flag:b>
flag: true = ボクセルが可視化される.  false = エンベロープのみ可視化

物質名を変更する
/G4M/DICOM/renameMaterial　<str1:s>   <str2:s>
 str1:  元の物質名
  str2:  新しい物質名

DICOMジオメトリに使用される物質の一覧を表示する
/G4M/DICOM/showMatList　　<ofilename:s>  <level:i>
　　　default ofilename:  stdout　出力ファイル名
      defaule level: 1　出力レベル

RT-Structureの処理を有効化する。
/G4M/DICOM/rts  <flag:b>

RT-Structureを読む（実験）
/G4M/DICOM/RTS/file  <filename:s>
filename:  RT-Structureデータのファイル名を記述したASCIIファイル名

デバック用フラグ
/G4M/DICOM/RTS/verbose   <level:i>
lvl:  デバックレベル

それぞれのROI-IDに優先順位を設定する。
ROI-IDは、PTSIMでのROI領域をラベリングしたときのラベル番号に使われる。
/G4M/DICOM/RTS/setROIPriority  roi0  roi1 .... roiN
roi0-roiN:  優先順位低→高（上書き）



CTでの患者台の削除やデバッグ目的のために指定ROI番号の領域のHU値を指定のHU値に置き換える。rtsコマンドがtrueの場合、このコマンドが適用される。
/G4M/DICOM/roiCtValReplacer　<ifile:s>
ifile:  ROI番号と置き換えるHU値の対応表が記述されたファイル

6)DICOM geometry and Couch geometry
DICOMモジュール直下流にカウチモジュールを配置する手順
/G4M/Module/install Couch
/G4M/Module/select  Couch
/G4M/Module/attachZ DICOM

7)G4MDiskDumper
 このモジュールには、円盤形状でG4MDumpInfoSDが取り付けられており、この形状に入射したトラックの情報を記録する。記録したトラック番号はイベント実行中は保持されており、同じトラック番号のトラックが再度入射しても記録されることはない。
  トラック情報の記録ファイルは、beamOnの前後でファイルのオープンとクローズを手動で行う必要がある。

　Dumperのトラック情報を記録するファイルをオープンする
/G4M/DumpInfo/open   <filename:s> <format:s>
/G4M/DumpInfo/{mname}/open <filename:s> <format:s>
 format:  ASCII,  BINARY, IAEAPHSP.  (Default = ASCII)

  Dumperのトラック情報を記録するファイルをクローズする
  /G4M/DumpInfo/close
  /G4M/DumpInfo/{mname}/close
    出力ファイル形式は、以下の通り。イベント毎にトラック数とそのトラック情報が保存される。トラック情報は、スペースで区切られている。アスキー形式である。
    　/mmは単位を表す。
    <Number of track>　(改行)
    <x/mm>  <y/mm> <z/mm> <time/ns> <mass/MeV> <PDG> <Charge>　<Px/MeV>　<Py/MeV> <Pz/MeV> <Polarx> <Polary> <Polarz>
    出力フォーマット
    /g4M/DumpInfo/format  <ASCII or  Binary:s>
    但し、Binary形式の場合には、Polarは保存していない。
    VIII) Parallel World geometry (MyDetectorConstruction)
    パラレルワールドジオメトリ(Parallel world geometry)を作成する
    /My/DetConstruction/createPW　　<pwname:s>

パラレルワールドの一覧を表示する
/My/DetConstruction/listPW

シミュレーション空間のデバック出力
/My/DetConstruction/verbose

 
IX) Event and Tracking (UserAction)
1)MyEventActForMonitor
イベント番号を出力するためのファイルを指定する
/My/<name>/eventMonitor   <nevent:i>   <filename:s>
nevent:  出力するイベント数のくりかえし数
filename:  出力ファイル

2)MyTrackingActForKillTrkMessenger
(<name>は、tracking1, tracking2の２つをデフォルトで用意)
以下のコマンドにより、粒子生成を抑制するz領域を設定する
/My/<name>/killZUpStream　　<z:d> <unit>
  z:  z座標
  defaule unit: mm

中性子の生成をしない
/My/<name>/killNeutral  <flag:b>
flag:  true 生成しない

軽粒子の生成をしない
/My/<name>/killLepton  <flag:b>
flag:  true 生成しない

二次粒子の生成をしない
/My/<name>/killSecondary  <flag:b>
flag:  true 生成しない

 
X) DICOM-RT (MyDynamicPortRTPSetting )
現在、DICOM-RTに関するコマンドは、タグの解釈が装置によって異なる場合があるため、完全にはサポートされていません。

RT-Planデータファイルを読む
/My/Dynamic/Module/RTPFile  <filename:s>
filename:  RT-Planデータ名を記述したASCIIファイル名

モジュールにRT-Planデータのパラメタを適用する
/My/Dynamic/Module/RTPApply

RT-Planに従って治療室（Room）にビーム機器をインストールする
/My/Dynamic/Module/RTPInstall

ビームIDを選択する
/My/Dynamic/DICOM/beamid  <id:i>
id:  ビームID

RT-Planに記録されている参照ビーム一覧を表示する
/My/Dynamic/DICOM/printRefBeam

RT-Plan情報（タグや値）を表示する
/My/Dynamic/DICOM/printRTPInfo

参照点での線量値を出力する
/My/Dynamic/DICOM/printRefDose  <ofilename:s>
　ofilename:  出力ファイル名

デバック用出力
/My/Dynamic/DICOM/RTPVerbose <lvl:i>
lvl:  デバックレベル

XI) Histograms
ヒストグラムを出力するファイル名を指定する
/My/runaction/dumpfile  <filename:s>
filename:  出力ファイル名（拡張子不要）

ヒストグラムを有効にする
/My/runaction/hist/enable {hname:s} {flag:b}  {mname:s}
hname:  TTree:  DICOMCT, DICOM
TH3: JSTEdep, JSTDose
flag:  true/false　有効化/無効化
 mname: モジュール名(SD名に同じ) 省略可：WaterPhantomまたはDICOMとなる。
  これは、複数の検出器をインストールした場合に、TH3型のJSTEdep、JSTDoseをどちらの検出器で有効化するかを指定するための引数である。TTree型の場合には、次に示すコマンドでビームモジュールとTTreeの対応付けを行う。

ヒストグラム(多次元)を作成し、SensitiveDetectorと対応付けする。
/My/runaction/ntuple/create  {ntname:s} {colname:s}
  ntname: 多次元ヒストグラム(ntuple)の固有名
  　colname:  記録するデータ元となるコレクションの名前
               (SDname/HitsCollection) （補足）SDnameはmnameと同じ。

　(補足)colnameとsdnameの対応表
BeamModule	colname
WaterPhantom	WaterPhantom/HitsCollection
DICOM		DICOM/HitsCollection
TubeDetector	TubeDetector/HitsCollection
BoxDetector	BoxDetector/HitsCollection

ヒストグラムの設定の状態を表示する
/My/runnaction/histList


Hitスコアリング可能な物理量を表示する
/My/runaction/ntuple/showScName

HitのNtupleに保存する情報を登録する
/My/runaction/ntuple/addScColumn  {nname:s}  {qname:s} {type:s} {unit:s}
name:  ntuple 名で指定する
qname: スコアリングする量の名称を指定する
type:  I（整数）, D（実数），F（実数）
unit:  物理量の単位  (arbitral unit = “none”)

HitのNtupleに設定された登録情報を表示する
/My/runaction/ntuple/showScColumn   {nname:s}
nname:  ntuple名

Digiヒストグラム(多次元)を作成する。
/My/runaction/ntuple/dc/create {ntname:s} {colname:s}
  ntname: 多次元ヒストグラム(ntuple)の固有名
  　colname:  記録するデータ元となるコレクションの名前

Digiでスコアリング可能な物理量を表示する
/My/runaction/ntuple/showDcName

DigiのNtupleに保存する情報を登録する
/My/runaction/ntuple/addDcColumn  {nname:s}  {qname:s} {type:s} {unit:s}
name:  ntuple 名で指定する
qname: スコアリングする量の名称を指定する
type:  I（整数）, D（実数），F（実数）
unit:  物理量の単位  (arbitral unit = “none”)

DigiのNtupleに設定された登録情報を表示する
/My/runaction/ntuple/showDcSetColumn   {nname:s}
nname:  ntuple名
DigiSetヒストグラム(多次元)を作成する。
/My/runaction/ntuple/dcset/create {ntname:s} {colname:s}
  ntname: 多次元ヒストグラム(ntuple)の固有名
  　colname:  記録するデータ元となるコレクションの名前

DigiSetでスコアリング可能な物理量を表示する
/My/runaction/ntuple/showDcSetName

DigiSetのNtupleに保存する情報を登録する
/My/runaction/ntuple/addDcSetColumn  {nname:s}  {qname:s} {type:s} {unit:s}
name:  ntuple 名で指定する
qname: スコアリングする量の名称を指定する
type:  I（整数）, D（実数），F（実数）
unit:  物理量の単位  (arbitral unit = “none”)

DigiSetのNtupleに設定された登録情報を表示する
/My/runaction/ntuple/showDcSetColumn   {nname:s}
nname:  ntuple名

 
XII) Digitizer　（試験運用）
元データとなるコレクションから、デジタイズタイプに応じた処理を行い、処理後のデータコレクションを作成する。
/My/eventaction/digi/create  {name:s}  {DigiType:s}
                             {ColFrom:s} {ColTo:s} {sdname:s}
			     name: Digitizerの固有名
			     DigiType: Digitizer処理を指定する。
			     ColFrame: 元となるコレクション名
			     ColTo: 処理後のコレクション
			     sdname: Hitコレクションが元データとなる場合は、コレクション名の重複を区別するためにHitコレクションのSD名を指定する。

DigiTypeについて:
・DigiVolMerger (Hit->Digi)
Hitから同じジオメトリのコピー番号をもつHitをグループ化する。ジオメトリ単位ごとに信号量としてエネルギー付与値を積算する。
・DigiCoincidence  (Digi->DigiSet)
PETでの時間コインシデンスをとり、設定時間幅のDigiデータをグループ化する。

Digitizerモジュールを選択する。
/My/eventaction/digi/select  {name:s}
name: デジタイザーの固有名

selectで選択したdigitizerモジュールに時間ウインドウを設定する
ただし、DigiCoincidenceのときのみ有効。
時間ウインドウを設定
/My/eventaction/coincidence/twin  {tw:d}  {toff:d} {unit:s}
/My/eventaction/coincidence/twin  {emean:d}  {ew:d} {unit:s}
tw : 時間ウインドウ
toff: 信号から時間ウインド開始までの時間オフセット
unit: 単位
emean: エネルギー中心値
ew: エネルギー幅　（emean+-ew）
unit: 単位


以上
-->