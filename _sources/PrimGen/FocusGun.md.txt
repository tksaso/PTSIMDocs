## FocusGun
  FocusGunは，G4ParticleGunを使って実装されており，G4ParticleGunのコマンドが利用できる。加えて，焦点を持った初期粒子を発生することができる。
  
### ビームエネルギーのゆらぎをエネルギーの単位で設定する
```
/G4M/Beam/energyfluc <dE:d>  <Unit:s>
```
  dE:  エネルギー揺らぎの値をガウス分布の標準偏差値で与える
  Default unit:  MeV

### X方向の焦点位置となるZ座標を設定する
```
 /G4M/Beam/FocusDistanceX  <zX:d> <unit:s>
```
 zX:  X方向の焦点座標
 Default unit:  mm

### Y方向の焦点位置となるZ座用を設定する
```
 /G4M/Beam/FocusDistanceY  <zY:d> <unit:s>
```
 zY:  Y方向の焦点座標
 Default unit:  mm

### 初期粒子発生座標と焦点位置が同じ場合は自動計算不能のため，発生する粒子の運動量方向を与える。
```
  /G4M/Beam/SpatialExpanseX  <ExpanseX:d>  <unit:d>
  /G4M/Beam/SpatialExpanseY  <ExpanseY:d>  <unit:d>
```
   Expanse X, Expanse Y:  角度の単位で値をガウス分布の標準偏差値で与える。
   Default unit:  mrad

### ビームパラメタの設定をASCIIファイルから読み込む。
```
 /G4M/Beam/file  <filename:s>
```
ファイル形式はG4MBeamGunと同様である。
加えてG4MFocusGunに特有な設定は，コマンドで設定しなければならない。
