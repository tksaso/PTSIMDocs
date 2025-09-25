## VTK Legacy formatの利用

### ジオメトリスキャン
 ジオメトリのボクセル化/ParaView可視化用VTK Legacy formatでの出力する。  
指定するビームモジュールまたは、指定する空間領域を分割して、各点の質量密度を取得してボクセルジオメトリデータとして出力する。出力データ形式は、VTK Legacy formatである。

#### 指定したビームモジュールをボクセルデータで出力
```
/Dynamic/Module/geomScan {mname:s}  {nx:i} {ny:i} {nz:i}　{binaryFlag:b}
```
- nx, ny, nz: 分割数
- binaryFlag: 出力データ型（Default = false, ASCII形式）
- 出力ファイル名は、自動的に{mname}.vtkとなる

#### 指定した空間領域をボクセル化する
**(a) 空間領域を指定する**
```
/Dynamic/GeomScan/setVolume  {nx:i} {xlow:d} {xup:d} {ny:i} {ylow:d}  {yup:d} {nz:i} {zlow:d} {zup:d} {unit:s}
```
- nx, ny, nz:  分割数
- xlow, ylow, zlow: 座標値
- xup, yup, zup: 座標値
- unit: 座標値の単位

**(b)設定した空間領域をスキャンしてファイル出力**
```
/Dynamic/GeomScan/voxelScan   {filename:s} {binaryFlag:b} {byteswap:b}
```
- filename:  出力ファイル名
- binaryFlag: default=false
- byteswap:   default=false
