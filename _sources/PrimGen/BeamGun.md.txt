## BeamGun

BeamGunは，G4ParticleGunを使って実装されており，G4ParticleGunのコマンドが利用可能である。加えて，スポットサイズとエネルギーの揺らぎを設定することができる。

### Beam spotでの発生粒子数の分布関数を指定する
Gauss distribution またはFlat distributionとなる。
```
  /G4M/Beam/gaussSpot   <flg:b>
```

### スポットサイズ設定
Gauss分布であれば標準偏差値，Flatであればその範囲.
```
 /G4M/Beam/spotsize  <dx:d>  <dy:d>  <dz:d>  <unit>
  Default unit:   mm
```

### ビームエネルギーのゆらぎをエネルギーの単位で設定する
```
 /G4M/Beam/energyflac  <dE:d>  <unit>
```
dE:  エネルギー揺らぎの値をガウス分布の標準偏差値で与える
Default unit:  MeV

### ビームエネルギーの揺らぎをパーセンテージで設定する
```
G4M/Beam/energyflacPerc  <dEPerc:d>  <unit>
```
 dE:  エネルギー揺らぎの値をビームエネルギー値に対する割合（%）で与える
 Default unit:  perCent

### Beam emittance (未実装)
```
/G4M/Beam/emittance  <EmitX:d> <EmitY:d> <EmitZ:d>
```

### ビームの初期角度方向の標準偏差値を与える。
```
/G4M/Beam/angSigma  <SigmaX:d> <SigmaY:d>  <unit:s>
```
SigmaX, SigmaY:  初期角度の値をガウス分布の標準偏差値で与える
Default unit:  mrad

### ビームパラメタの設定をASCIIファイルから読み込む。
```
/G4M/Beam/file  <ifilename:s>
```
[ASCIIデータファイルの書式例](G4MBeamFile.md)
