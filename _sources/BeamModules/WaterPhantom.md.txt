# WaterPhantom Commands
ビームモジュールのG4MWaterPhantomは、スコアリング機能付きのボクセル構造を持っています。  
ビームモジュール名を`WaterPhantom` とした場合、以下のコマンドで`{module_name}`と表記している部分は、`Phantom` となります。    
一方、ビームモジュール名を変えている場合には、登録したビームモジュール名が `{module_name}` に使われます。  

例：  
- モジュール名が `WaterPhantom` の場合  
  `/G4M/Module/Phantom/size ...`  
- モジュール名が `WP` の場合  
  `/G4M/Module/WP/size ...`
となります。

### ファントムの物質を変更する
```
/G4M/Module/{module_name}/material {matname:s}
```
| 値表記 |　説明 |
|:---|:---|
| matname:s | 物質名 |

### 水ファントムの大きさを指定する
```
/G4M/Module/{module_name}/size {dX:d} {dY:d} {dZ:d} {unit}
```
| 値表記 |　説明 |
|:---|:---|
| dX:d, dY:d, dZ:d, unit:s |  ハーフサイズ長と単位　|

### 水ファントム内のスコアリング領域の大きさを指定する
```
/G4M/Module/{module_name}/sdsize {dxSD:d} {dySD:d} {dzSD:d} {unit}
```
| 値表記 |　説明 |
|:---|:---|
| dxSD:d, dySD:d, dzSD:d, unit:s |  ハーフサイズ長と単位　|


### 水ファントム内のスコアリング領域の配置を設定する
```
/G4M/Module/{module_name}/sdoffset {x:d} {y:d} {z:d} {unit}
```
| 値表記 |　説明 |
|:---|:---|
| x:d, y:d, z:d, unit:s |水ファントム内での配置中心座標と単位 |

### スコアリング領域をセグメントに分割する
```
/G4M/Module/{module_name}/dim {Nx:d} {Ny:d} {Nz:d}
```
| 値表記 |　説明 |
|:---|:---|
| Nx:i, Ny:i, Nz:i |  分割数 |

### スコアリングオプション
#### エネルギー付与のあるトラックのみ記録するかを指定する
```
/G4M/Module/{module_name}/edep {flag:b}
```

| 値表記 |　説明 |
|:---|:---|
| flag:b |  true: エネルギー付与があるときのみ記録<br> false: 全トラックを記録 |

---

## CylinderPhantom Commands
CylinderPhantomの以下のコマンドで `{module_name}` の部分には、登録時のモジュール名が使われます。
例:  
 - モジュール名が `CYL` の場合  
   - `/G4M/Module/CYL/size ...`
- モジュール名が `WP` の場合  
   - `/G4M/Module/WP/size ...`
---

### シリンダファントムの大きさを指定する
```
/G4M/Module/{module_name}/size {R:d} {Phi:d} {dZ:d} {R_unit} {Phi_unit} {dZ_unit}
```

| 値表記 |　説明 |
|:---|:---|
| R:d, Phi:d, dZ:d  | 半径、角度範囲、長さ（dZはハーフサイズ） |  
| R_unit:s, Phi_unit:s, dZ_unit:s |  Default: mm,  degree,  mm |

---
### シリンダスコアリング領域の大きさを指定する
```
/G4M/Module/{module_name}/sdsize {RSD:d} {PhiSD:d} {dzSD:d} {R_unit} {Phi_unit} {dZ_unit}
```
| 値表記 |　説明 |
|:---|:---|
| RSD:d, PhiSD:d, dzSD:d |  スコアリング領域の半径、角度範囲、長さ（dZはハーフサイズ） |
| R_unit:s, Phi_unit:s, dZ_unit:s |  Default: mm,  degree,  mm |

---

### シリンダスコアリング領域の配置を設定する
```
/G4M/Module/{module_name}/sdoffset {r:d} {phi:d} {z:d} {R_unit} {Phi_unit} {dZ_unit}
```
| 値表記 |　説明 |
|:---|:---|
| r:d, phi:d, z:d |  ファントム内座標での中心座標位置　|  
| R_unit:s, Phi_unit:s, dZ_unit:s |  Default: mm,  degree,  mm |m

---

### シリンダスコアリング領域のセグメント数を指定する
```
/G4M/Module/{module_name}/dim {Nr:d} {Nphi:d} {Nz:d}
```

| 値表記 |　説明 |
|:---|:---|
| Nf:i, Nphi:i, Nz:i |  分割数 |

