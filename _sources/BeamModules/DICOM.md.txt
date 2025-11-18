## DICOM geometry

### DICOMジオメトリを選択する
```
/G4M/DICOM/select   {mname:s}
```
- mname:   DICOM

### DICOMデータのファイルリストを指定する
```
/G4M/DICOM/file 　{ifilename:s}
```
- ifilename:  CT画像ファイルの一覧を記述したASCIIファイル

### CT座標系でのアイソセンターを指定する
```
/G4M/DICOM/isocente　　{x:d}  {y:d}  {z:d}  {unit}
```
 - x, y, z:  座標値， unit:  mm etc.

### Tデータで欠けているスライスを補償する
```
/G4M/DICOM/complement　 {flag:b}
```
- flag:  true（補間する）

### ボクセルサイズを設定する
```
/G4M/DICOM/mesh  {size:d}  {unit}
```
- size:  立方体の辺の長さ
- default unit :  mm

### CTのピクセルをxy方向で結合する。
```
/G4M/DICOM/mesh/bin  {n:i}
```
- n:  結合ピクセル数

### 有効な画像領域を設定する。
有効な画像領域外のボクセルの物質は空気に置き換えられる。
（トリミングするコマンドが追加実行されれば、設定した外側は削除される。）
```
/G4M/DICOM/winXmin　　{min:d}  {unit}
/G4M/DICOM/winXmax　　{max:d}  {unit}
/G4M/DICOM/winYmin　　{min:d}  {unit}
/G4M/DICOM/winYmax　　{max:d}  {unit}
/G4M/DICOM/winZmin　　{min:d}  {unit}
/G4M/DICOM/winZmax　　{max:d}  {unit}
```
- min, max:  最小値、最大値,    unit:  mm etc.

### 空気のHU値を設定する。
このHU値は有効な画像領域が指定されたとき、その領域の外側のHU値として利用される。
```
/G4M/DICOM/ctair {ct:double}
```
- ct:  空気のHU値

### 有効な画像領域をトリミングするためのフラグを設定する
```
/G4M/DICOM/trim　　{flag:b}
```
- flag:  true　トリミング実効

### HU値の範囲を設定する。
範囲外のHU値は指定された最小値または最大値に値が丸められる
```
 /G4M/DICOM/minvalue　　{minvalue:d}
 /G4M/DICOM/maxvalue　　{maxvalue:d}
 ```
- minvalue, maxvalue:  最小、最大のHU値

### 患者ジオメトリの輪郭抽出をするためのフラグ
ラベリングアルゴリズムによって、輪郭を判断しています。
```
/G4M/DICOM/label　　{flag:b}
```
- flag: true　輪郭抽出を実行

### 輪郭抽出を実行する際のHU値の閾値
```
/G4M/DICOM/cutoff　{ct:d}
```
- ct:  HU値の閾値

### HU-密度変換表ファイルを指定する
```
/G4M/DICOM/ct2density 　{ifilename:s}
```
- CT2Densityファイルの書式:　data/Sample/Dicom/CT2Density.dat　　
| {HU} |  {Density in g/cm3} |
|---:| :---|
|10   |  (データ点数)   |
|-5000 |   0.0 |
|-1000 |  1.21e-3 |
|-98   |  0.93 |
|-97   |  0.930486 |
|14    |  1.03 |
|23    |  1.031 |
|100   |  1.119900 |
|101   |  1.076200 |
|1600  |  1.964200 |
|3000  |  2.8	 |

### HU値の範囲と物質との対応表ファイルを指定する
```
/G4M/DICOM/paramtype   {paramtype_ifile:s}
```
- paramtypeファイルの書式: 
  - data/Sample/Dicom/CT2Water.dat	
```
 {HU_L}  {HU_H}  {Mat} {Corr.}
 1 (データ点数)
 -2048   4096     H_2O     1.00
```

  - data/Sample/Dicom/CT2Tissue.dat	 
```
 27 (データ点数）
 -2048	-951	Group01	1.051
-950	-121	Group02	0.977
-120	-83	Group03	0.948
-82	-53	Group04	0.958
-52	-23	Group05	0.968
-22	7	Group06	0.976
8	18	Group07	0.983
19	79	Group08	0.993
80	119	Group09	0.971
120	199	Group10	1.002
200	299	Group11	1.005
300	399	Group12	1.010
400	499	Group13	1.014
500	599	Group14	1.018
600	699	Group15	1.021
700	799	Group16	1.025
800	899	Group17	1.030
900	999	Group18	1.033
1000	1099	Group19	1.035
1100	1199	Group20	1.038
1200	1299	Group21	1.041
1300	1399	Group22	1.043
1400	1499	Group23	1.046
1500	1599	Group24	1.048
1600	1999	Group25	1.042
2000	3060	Group26	1.049
3061	4096	Group27	1.000	Number of lines
{HU_L} {HU_H} {Mat} {Corr.}
```

### 物質を低減するためのHU値のサンプリングを設定する
```
/G4M/DICOM/ctsamp  {bit:i}
```
- bit:  HU値の間引き数

### HU値と密度g/cm3の対応表での密度の分解能を決定する
```
/G4M/DICOM/densityResol  {reso:d}
```
- reso:  密度の分解能（例　0.01）

### ガントリー角を指定する
```
/G4M/DICOM/gantry  {angle:d}  {unit}
```
- angle:  ガントリー角度
- default unit:  deg

### カウチ角を指定する
```
/G4M/DICOM/couchAngle  {angle:d}  {unit}
```
- angle:  角度
- default unit: deg

### CTデータのデータオリエンテーションを設定する
```
/G4M/DICOM/dataOrientation　 {type:s}
```
- type:  NONE.  HFS,   FSS, HFP, FFP

### DICOMジオメトリのためのパラメタを更新する
```
/G4M/DICOM/update
```

### ボクセルの可視化フラグを設定する
```
/G4M/DICOM/vis   {flag:b}
```
- flag: true = ボクセルが可視化される.  false = エンベロープのみ可視化

### 物質名を変更する
```
/G4M/DICOM/renameMaterial　{str1:s}   {str2:s}
```
- str1:  元の物質名
- str2:  新しい物質名

### DICOMジオメトリに使用される物質の一覧を表示する
```
/G4M/DICOM/showMatList　　{ofilename:s}  {level:i}
```
- default ofilename:  stdout　出力ファイル名
- default level: 1　出力レベル

### RT-Structureの処理を有効化する。
```
/G4M/DICOM/rts  {flag:b}
```

### RT-Structureを読む（実験）
```
/G4M/DICOM/RTS/file  {filename:s}
```
- filename:  RT-Structureデータのファイル名を記述したASCIIファイル名

### デバック用フラグ
```
/G4M/DICOM/RTS/verbose   {level:i}
```
- lvl:  デバックレベル

### それぞれのROI-IDに優先順位を設定する。
 ROI-IDは、PTSIMでのROI領域をラベリングしたときのラベル番号に使われる。
 ```
/G4M/DICOM/RTS/setROIPriority  roi0  roi1 .... roiN
```
- roi0-roiN:  優先順位低→高（上書き）

### CT値の置き換え
CTでの患者台の削除やデバッグ目的のために指定ROI番号の領域のHU値を指定のHU値に置き換える。rtsコマンドがtrueの場合、このコマンドが適用される。
```
/G4M/DICOM/roiCtValReplacer　{ifile:s}
```
- ifile:  ROI番号と置き換えるHU値の対応表が記述されたファイル

---

## DICOM geometry and Couch geometry

### DICOMモジュール直下流にカウチモジュールを配置する手順
```
/G4M/Module/install Couch
/G4M/Module/select  Couch
/G4M/Module/attachZ DICOM
```
