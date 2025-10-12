## Root3d
   CERN ROOTのTH3形式で保存された３次元座標分布に従って粒子を発生する。粒子の位置座標が適用され、運動エネルギーはゼロに設定される。放射性同位体の発生分布を与えて崩壊後のシミュレーションを行うことを想定している。この機能を利用するためには、cmakeの際に -DPRIMROOT=ON を指定する必要がある。
   
### デバッグフラグ
```
  /G4M/Root3d/versbose    <lvl:i>
```
### ファイルとTH3の指定
```
   /G4M/Root3d/file   <RootFileName:s>  <TH3Name:s>
```

   ROOTのファイル名と記録されているTH3型３次元ヒストグラム名を指定する。ここで、TH3型ヒストグラムの各軸のスケールはミリメートルであるとする。また、粒子生成の際には、TH3::Random3を利用しているため、ビン内の座標値は一様乱数で決定される。頻度は自動的に規格化されて計算される。TH3の作成は、サンプルROOTマクロTH3OSample.Cが添付されているので参照のこと。
   
### 粒子を設定する
```
   /G4M/Root3d/particle  <particleName:s>     // デバッグ用である
   /G4M/Root3d/ion   <Z:i>  <A:i>  <Charge:i>
```

### 1イベントに発生する粒子数
```
/g4M/Root3d/nparticlesPerEvent   <n:i>
```

### 発生点の座標を移動する場合のオフセット値（この値が座標に足し算される）
```
/G4M/Root3d/vtxOffset  <ox:d>  <oy:d>  <oz:d>  <unit:s>
```
