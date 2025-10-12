## FanBeam
 G4ParticleGunを利用して実装されており，G4ParticleGunのコマンドが利用可能である。加えて，扇型の方向に粒子を発生する機能がある。

### (0, r, 0)を生成点とすることを前提として，そのY座標rを設定する。
```
/G4M/FanBeam/radius  <r:d>  <unit:s>
```
 r:  生成点のY座標
 unit:  mm etc.

### 扇形の空間分布関数を指定する。
```
 /G4M/FanBeam/gaussSpot  <flag:b>
```
   flag:  true	 ガウス分布
   false   一様分布

### ビーム発生のZ座標を設定する。（未実装）
```
 /G4M/FanBeam/z  <z:d> <unit:s>
```

### 生成する扇型の角度を設定する。（未実装）
```
  /G4M/FanBeam/theta  <theta:d>  <unit:s>
```

### 生成する扇型の角度を設定する。
```
/G4M/FanBeam/phi  <pho:d>  <unit:s>
```
 phi:  角度値。ガウス分布の標準偏差。
 unit:  deg, rad etc.

### エネルギー揺らぎを設定する。
```
  /G4M/Beam/energyfluc  <energyflac:d>  <unit:s>
```
   energyflac:  エネルギー揺らぎ値をガウス分布の標準偏差で与える
   unit:  MeV

