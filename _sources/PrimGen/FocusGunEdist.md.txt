## FocusGunEdist

G4MFocusGunの機能に加えて, G4MFocusGunEdistでは，ビームのエネルギー分布を設定することが可能である。つまり，G4MFocusGunのエネルギー設定以外のコマンドが利用できる。更に，次に示すように，エネルギーと強度を入力して分布を設定することができる。

###エネルギー分布の設定をクリアする
```
/G4M/Beam/clear
```

### エネルギー値とその強度を設定する
```
/G4M/Beam/addEnergy  <Energy(MeV unit):d>  <Intensity:d>
```
 Energy:  エネルギー値
 Intensity:  強度

*エネルギーは MeV単位で与える。
このコマンドを順次，エネルギーを変えて入力してエネルギーと強度分布のデータを設定する。
 与えたエネルギーの中間値は，線形補間される。あらかじめ，線形補間を行った分布を用意することができる。
 下のコマンドは，補間するデータ点の数を指定する。
```
  /G4M/Beam/sample  <n:i>
```
   n:  Number of points

