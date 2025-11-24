## G4Roomのコマンド

G4RoomはGeant4のワールドボリュームであり、そのパラメタは基本的に固定であることを想定しています。  
 しかしながら、パラメタファイルを編集することなしに、以下のコマンドによりパラメタを変更することが可能です。

### Roomのサイズ
```
/G4M/Room/sizeX  {value:d}  {unit:s}
/G4M/Room/sizeY  {value:d}  {unit:s}
/G4M/Room/sizeZ  {value:d}  {unit:s}
```
- value, unit:  それぞれのhalf-length

### Roomの物質
```
/G4M/Room/material  {matName:s}
```
- matName : 物質名

### Rootの可視化
```
/G4M/Room/vis  {flag:b}
```
- flag: true=On

