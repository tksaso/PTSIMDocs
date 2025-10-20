## ParticleGun

   Geant4のG4ParticleGunを利用する。詳細は，Geant4のマニュアルを参照すること。ここでは，代表的なコマンドを幾つか紹介する。

### 粒子を設定する
```
/gun/particle   <particleName:s>
```
particleNam” : proton, e-, e+, gamma   etc.

### Ion beamを設定する
```
/gun/particle  ion
/gun/ion     <Z:i>   <A:i>   <Charge:i>
```

### 運動量方向を設定する
```
/gun/direction   <dirX:d>  <dirY:d>  <dirZ:d>
```

### 発生位置を設定する
```
/gun/position   <x:d>  <y:d>  <z:d>  {unit:s}
```
