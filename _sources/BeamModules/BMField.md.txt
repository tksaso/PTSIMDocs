# G4MBoxField, G4DiskField
 空間磁場を設定するためのモジュールとなっており、サイズはG4MBoxやG4MDiskと同じ方法で設定する。

## 磁場設定コマンド
 以下{module_name}は、空間磁場のビームモジュール名を表す。
```
/G4M/Module/{module_name}/field  {Bx:d} {By:d} {Bz:d}  {unit:s}
```
- Bx, By, Bz:  磁場値
- unit:  tesla etc.

## ジオメトリ下部構造への磁場適用の有無
 以下{module_name}は、空間磁場のビームモジュール名を表す。
```
/G4M/Module/{module_name}/forceToAllDaughters {flag:b}
```
- flag: true or false


