## IAEAphsp
  IAEAphsp形式のデータの読み込み，初期粒子を発生するためのコマンド。

### IAEA phspのためのメンバ変数を初期化する
```
/G4M/IAEAphsp/init
```

### 読み込むIAEA phspのデータファイルを設定する。
```
/G4M/IAEAphsp/file  <fileBase:s>  <fileStopID:s>
```
   fileBase:   Base file name
   fileStopID:  ID of the file name
   fileStopIDは，同じRunで異なるZ座標で記録されたデータの場合の識別子である。

### IAEA phsp データの読み込みを終了してファイルを閉じる。
```
/G4M/IAEAphsp/close
```

### デバッグレベルを設定する。
```
  /G4M/IAEAphsp/verbose  <v:i>
```
  v: debug level

### リサイクルフラグ。１つの粒子データを指定した回数再利用する
```
/G4M/IAEAphsp/recycle  <nrecycle:i>
```
nrecycle: number of recycle times.

### 並列処理の場合に, データファイルを複数の論理的なセグメントに分割して，それぞれの並列処理に割り当てる。
```
/G4M/IAEAphsp/totalParaRun  <npara:i>
```
 npara:  Parallel runの個数を設定する 

### 並列処理でのRun識別番号を設定する。
```
/G4M/IAEAphsp/paraRunId <runid:i>
```
   runid:  Runの識別番号

### ヒストリ情報を含まないデータの場合、１イベントで実行する粒子数を指定する。
```
/G4M/IAEAphsp/nparticlesPerEvent <np:i>
```
 np:  Number of particles simulated in one event.
 (Number of beamOn need to be estimated as Number of total particles in header file divided by np.)
   
### 記録された情報の発生座標を変更する。(mm)
```
    /G4M/IAEAphsp/translate <x:d>  <y:d>  <z:d>
```
     x, y, z:  発生点の移動量
     単位は，mmで固定。

### 回転の座標変換を行うときの，各座標軸周りでの回転の順番を設定する。
```
/G4M/IAEAphsp/rotateOrder  <iorder:i>
```
iorder:  123 (X->Y->Z),  213 (Y->X->Z)

### 回転角度を設定する。
```
  /G4M/IAEAphsp/rotateX  <angle:d>  <unit>
  /G4M/IAEAphsp/rotateY  <angle:d>  <unit>
  /G4M/IAEAphsp/rotateZ  <angle:d>  <unit>
```
   angle:  角度
   unit:  mrad etc.

### 等方的な放射線発生のデータにおいて，アイソセンターの位置を設定する。
```
  /G4M/IAEAphsp/isoPos <x:d>  <y:d>  <z:d>  <unit:s>
```
  x, y, z:  アイソセンターの座標
  unit:  mm etc.

### ガントリー角度とその回転軸を設定する。
```
 /G4M/IAEAphsp/gantryAxis  <xdir:d> <ydir:d>  <zdir:d>
 /G4M/IAEAphsp/gantryAngle  <angle>  <unit:s>
```
 xdir, ydir, zdir:  回転軸
 angle:  回転角
 unit:  deg etc.

### トリートメントヘッドの回転とその回転軸を設定する。
```
/G4M/IAEAphsp/headAxis <xdir:d> <ydir:d> <zdir:d>
/G4M/IAEAphsp/headAngle  <angle:d> <unit:s>
```
 xdir, ydir, zdir:  回転軸
 angle:  回転角
 unit:  deg etc.
 
