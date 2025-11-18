## ScanBeam
スポットスキャニングを行う。３つのASCIIファイル入力が必要である。

### デバッグモードの設定
```
 /G4M/ScanBeam/verbose  {v:i}
```
  v:  デバッグフラグ

### Scanning Magnetの登録と有効化
事前にScanning Magnetがビームモジュールとして登録済みである必要がある。
```
/G4M/ScanBeam/scanMagXY   {mnameX:s}   {mnameY:s}
```			 
 mnameX, mnameY:  ビームモジュール名

### ビームエネルギーとその揺らぎ，そしてスポットサイズを記載したASCIIファイル（eidファイルと呼ぶ）を読み込み，設定を行うためのコマンド。
```
/G4M/ScanBeam/eidFile   {filename:s}
```
 eidFileName:  eidファイル名

 eidファイルの書式
```text
 data/Sample/G4MScanBeam/EIDSample.dat	Description
 3
 0 100.  0.  0.  0.
 1 200.  0.  0.  0.
 2 250.  0.  0.  0.	Number of lines (e.g. number of beams)
 ID  E(MeV)  dE(MeV) SigX(mm)  SigY(mm)
 *ID must be given in Sequential.
```

### eidファイルを与えるもうひとつのコマンド。
こちらのコマンドの場合には，エネルギー，エネルギー揺らぎ，スポットに加えて，初期角度を各BeamID毎に定義したASCIIデータファイルを読み込む。
```
/G4M/ScanBeam/eidFile2   {filename:s}
```
 eid2FileName:  eid2ファイル名

eidファイルの書式:
data/Sample/G4MScanBeam/EIDSample2.dat
```
3
0 100.  0.  0.  0. 0. 0.
1 200.  0.  0.  0. 0. 0.
2 250.  0.  0.  0. 0. 0.	Number of lines (e.g. number of beams)
ID  E(MeV)  dE(MeV) SigX(mm)  SigY(mm) AngSigX(mrad) AngSigY(mrad)
*ID must be given in Sequential.
```

### スポット座標と線量強度を与えるASCIIファイル(scanファイル)を指定する。
スポット座標は，アイソセンター面でのx,y座標である。
```
/G4M/ScanBeam/scanFile   {filename:s}
```
 scanFileName:  scanファイル名

scanファイルの書式
data/Sample/G4MScanBeam/ScanSample.dat
```text
5
0  -100.  -30.  5.
1     0.   30.  2.
1    50.  -50.  2.
2  -100. -100.  1.
2    10. -100.  2.	Number of lines (e.g. Number of spots)
ID  x(mm)  y(mm)  Gy
```

### wieght係数を記載したASCIIファイルを指定
異なるエネルギーでも同じピーク線量値となるように初期粒子数を調節する係数(weight)を指定する。
```
/G4M/ScanBeam/weightFile   {filename:s}
```
 weightFileName:  weightファイル名
 weightファイルの書式
 data/Sample/G4MScanBeam/WeightSample.dat
```text 
  3
  90.    1.
  200.   1.
  300.   1.	Number of lines
  E(MeV)  Weight
```
  異なるエネルギーだが，同じ粒子数の計算を行ったときに得られた線量の相対値を表す。

### 設定を表示
```
/G4M/ScanBeam/show  {type:s}
```
type: eid,  spot,  weight
(*) 個々のスポットの粒子発生数を表す確率は/run/beamOnが実行されたときに，自動的に計算される。

### /run/beamOnの前に粒子発生数を計算(デバッグ用）
```
/G4M/ScanBeam/calcProb
```
