
# General Particle Source (GPS)

  Geant4のG4GeneralParticleSourceを利用する。詳細は，Geant4マニュアルを参照のこと。
  
<!--
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
