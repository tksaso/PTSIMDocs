2)G4ScanningMagnet
電磁石のモジュールへの磁場値を設定する。
＊磁場値は、デフォルトでは電磁石の位置と磁場の長さ、粒子エネルギーと指定されたアイソセンター面での座標から、近似式で計算されて用いられる。近似式の補正を行うために、(1)手動で磁場値を設定するコマンド、(2)磁場計算の近似式に補正係数を設定するコマンドが用意されている。これらのコマンドは、各電磁石において用いる。

(2-1)手動で磁場値を設定するコマンド
以下{module_name}は、電磁石のビームモジュール名を表す。
/G4M/Module/{module_name}/fiedvalue  {field_value:d}  {unit:s}
field_value:  磁場値
 unit:  T etc.

アイソセンターから電磁石モジュールまでの距離を設定する。この距離がスキャニングのための磁場値を計算する際に用いられる。
/G4M/Module/{module_name}/distance  {dist:d}  {unit:s}
 dist: 距離
  unit:  m etc.

次の2つのコマンドで、スポットの位置とビームの運動エネルギーの入力データ点を手動で指定する
アイソセンター面でのスポット位置を与える
/G4M/Module/{module_name}/putXFieldMap {idx:i}  {xval:d} {unit:s}
idx:  インデックス番号
xval:  スポットの位置（座標）
unit:  mm etc.

ビームの運動エネルギーを与える
/G4M/Module/{module_name}/putYFieldMap {idy:i}  {yval:d} {unit:s}
idy:  インデックス番号
yval:  ビームの運動エネルギー
unit:  MeV etc.

idx, idyで指定したスポット位置とビーム運動エネルギーでの磁場値を指定する
/G4M/Module/{module_name}/putVaueFieldMap {idx:i} {idy:i} {value:d} {unit:s}
idx:  スポット位置のインデックス番号
idy:  ビームの運動エネルギーのインデックス番号
value:  磁場値
unit:  T etc.

磁場の極性を設定する。
/G4M/Module/{module_name}/sign  {sign:i}
 sign:  +1 又は -1

アイソセンター面でのスポットの位置とビームの運動エネルギーで関連付けされる磁場値の表を作成する
/G4M/Module/{module_name}/createFieldMap  {interpolation:b}
interpolation:  データ間を補間（true）

磁場値の対応表を削除する
/G4M/Module/{module_name}/deleteFieldMap

磁場値をASCIIファイルに保存する
/G4M/Module/{module_name}/storeFieldMap  {file_name:s}
file_name:  出力ファイル名

ASCIIファイルから磁場値の表を読み込む
/G4M/Module/{module_name}/retrieveFieldMap  {file_name:s}

(2-2)磁場計算の近似式に補正係数を設定するコマンド
磁場の補正係数表のスポット位置を与える
/G4M/Module/{module_name}/putXFieldCoeff {idx:i}  {xval:d} {unit:s}
idx:  インデックス番号,　　xval:  スポット位置
unit:  mm etc.

磁場補正係数表のビームの運動エネルギー値を与える
/G4M/Module/{module_name}/putYFieldCoeff {idy:i}  {yval:d} {unit:s}
idy:  インデックス番号
yval:  ビームの運動エネルギー
unit:  MeV etc.

磁場補正係数表のidx, idyに対応する補正係数を与える
/G4M/Module/{module_name}/putVaueFieldCoeff {idx:i} {idy:i} {value:d}
idx:  スポット位置のインデックス番号
idy:  ビームの運動エネルギーのインデックス番号
value:  補正係数値

スポット位置とビームの運動エネルギーに対応する磁場値への補正係数表を作成する
/G4M/Module/{module_name}/createFieldCoeff  {interpolation:b}
interpolation:  true 補間

磁場値の補正係数表を削除する
/G4M/Module/{module_name}/deleteFieldCoeff


ASCIIファイルに磁場補正係数表を保存する
/G4M/Module/{module_name}/storeFieldCoeff  {file_name:s}
file_name:  出力ファイル名

ASCIIファイルから磁場補正係数表を読み込む。
/G4M/Module/{module_name}/retrieveFieldCoeff  {file_name:s}
file_name:  ファイル名
