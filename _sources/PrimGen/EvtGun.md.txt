## EvtGun

G4MDiskDumperで保存されたトラック情報から粒子を発生する。beamOnで指定するイベント数は、トラック情報を保存した時と同じ数にすること。

### トラック情報のファイルを開く。
```
   /G4M/EvtIF/open    <filename:s> <format:s>
```
   format:  ASCII (defaut) or BINARY

### トラック情報のファイルを閉じる
```
/G4M/EvtIF/close
```

### デバッグフラグ
```
/G4M/EvtIF/verbose  <level:i>
```

### 出力フォーマット選択
```
/G4M/EvtIF/format  <ASCII or  Binary:s>
```
