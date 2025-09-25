
## GDMLの利用

　GDMLジオメトリを利用する場合には、XML用のライブラリ(xercesなど)がインストールされており、Geant4ライブライも-DGEANT4_USE_GDML=ONのオプションの元でコンパイルされていなければならない。更に,
GDMLPTSToolkitならびにDynamicPortの双方を`-DUSEGDML`のオプションを付けてビルドしていなければならない。

### GDMLジオメトリファイルビームモジュールの登録
```
/Dynamic/Module/register  {mname:s}  G4MBMGdml  {gdmlFile:s} {x:d} {y:d} {z:d} {unit:s} {rx:d} {ry:d} {rz:d} {unit:s}
```
ビームモジュールを作成するクラス名の`G4MBGdml`の指定と、パラメタファイルにGDML形式ファイルを指定する点以外は、通常のビームモジュール登録と同じです。
- mname: ビームモジュールの固有名
- gdmlFile: GDML形式のジオメトリファイル
- x,y,z, unit:  中心位置とその単位
- rx,ry,rz, unit: 回転角とその単位

### GDMLファイルの物質定義をスキップする
　GDMLジオメトリ構築時に、GDMLファイルに記載されている物質定義をスキップし、既存の物質定義を適用する。
```
/Dynamic/Module/gdml/reader/skipMaterials   {flag:b}
```
- flag:  true=スキップして無視する,  false=読み込んで物質を作成する。

#### 既存ビームモジュールをGDMLジオメトリファイルに書き出す
```
/Dynamic/Module/gdml/write  {mname:s}  {bSD:b}  {refPointer:b} {filepath:s} {schemaLocation:s}
```
- bSD: true=SensitiveDetectorもGDMLに書き出す。
- bRefPointer: true=各部品名に重複を避けるハッシュコードをつける。
- filepath: GDMLの出力ファイルパス (Default = ./ )
- shemaLocation: (Default = ./data/schema/gdml.xsd)
