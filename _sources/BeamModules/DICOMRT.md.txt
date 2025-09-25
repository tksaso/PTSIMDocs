## DICOM-RT (MyDynamicPortRTPSetting )
 現在、DICOM-RTに関するコマンドは、タグの解釈が装置によって異なる場合があるため、完全にはサポートされていません。PTSIMには、粒子線施設によってコードを開発して実装できるように、以下のコマンドを用意しています。

### RT-Planデータファイルを読む
```
/My/Dynamic/Module/RTPFile  {filename:s}
```
- filename:  RT-Planデータ名を記述したASCIIファイル名

### モジュールにRT-Planデータのパラメタを適用する
```
/My/Dynamic/Module/RTPApply
```

### RT-Planに従って治療室（Room）にビーム機器をインストールする
```
/My/Dynamic/Module/RTPInstall
```

### ビームIDを選択する
```
/My/Dynamic/DICOM/beamid  {id:i}
```
- id:  ビームID

### RT-Planに記録されている参照ビーム一覧を表示する
```
/My/Dynamic/DICOM/printRefBeam
```

### RT-Plan情報（タグや値）を表示する
```
/My/Dynamic/DICOM/printRTPInfo
```

### 参照点での線量値を出力する
```
/My/Dynamic/DICOM/printRefDose  {ofilename:s}
```
- ofilename:  出力ファイル名

### デバック用出力
```
/My/Dynamic/DICOM/RTPVerbose {lvl:i}
```
- lvl:  デバックレベル
