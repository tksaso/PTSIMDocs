## G4MDiskDumper commands
　このモジュールには、円盤形状でG4MDumpInfoSDが取り付けられており、この形状に入射したトラックの情報を記録する。記録したトラック番号はイベント実行中は保持されており、同じトラック番号のトラックが再度入射しても記録されることはない。 
　トラック情報の記録ファイルは、beamOnの前後でファイルのオープンとクローズを手動で行う必要がある。

### Dumperのトラック情報を記録するファイルを開く
```
/G4M/DumpInfo/open   {filename:s} {format:s}
/G4M/DumpInfo/{mname}/open {filename:s} {format:s}
```
- format:  ASCII,  BINARY, IAEAPHSP.  (Default = ASCII)

### Dumperのトラック情報を記録するファイルをクローズする
```
/G4M/DumpInfo/close
/G4M/DumpInfo/{mname}/close
```
出力ファイル形式は、以下の通り。イベント毎にトラック数とそのトラック情報が保存される。トラック情報は、スペースで区切られている。アスキー形式である。以下の書式で保存される。/mmは値の単位を表す。
```
    {Number of track}　(改行)
    {x/mm}  {y/mm} {z/mm} {time/ns} {mass/MeV} {PDG} {Charge}　{Px/MeV}　{Py/MeV} {Pz/MeV} {Polarx} {Polary} {Polarz}
```

### Dumperのファイルタイプ
```
/g4M/DumpInfo/format  {ASCII or  Binary:s}
```
但し、Binary形式の場合には、Polarは保存していない。
