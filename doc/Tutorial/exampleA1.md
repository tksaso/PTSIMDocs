# Example A1(ファントム/スコア）

シミュレーション空間への水ファントムの配置とスコアリングについて解説します。
 - 水ファントムの登録と配置設定
 - 水ファントムでのスコアリング

以下、PTSIMの実行ディレクトリ(例: ~/PTSproject-install/PTSapps/DynamicPort)で作業します。

## 例題マクロファイル
PTSIMコードに付属するマクロファイル`exampleA1.mac`をコピーして用います。
```
$ cp  ./macros/tut/exampleA1.mac  .
```

実行
```
$ ./bin/PTSdemo  -i  exampleA1.mac
```
治療室の原点\(アイソセンター\)に水ファントムが配置されたシミュレーション空間です。

終了
```
Session: exit
```

### マクロファイルの解説
実行に必要なコマンド部分のみを抜粋して説明します。

```{code-block} bash
:linenos:
#
#  (PreInit State)
#
# [MA]Material
/G4M/Material/create G4_AIR
/G4M/Material/create G4_WATER
#
# [MA]PhysicsList
/control/execute ./macros/common/phys.mac
#
# [MA]Particle Therapy System Selection and Beam Module Registration
/G4M/System DynamicPort
/Dynamic/Module/Room/register  525.  525.  3550. mm
#
/Dynamic/Module/WaterPhantom/register Phantom
#
# [MA]Run Initialize
/run/initialize
#
#  (Idle State)
#
# Phantom settings and installation
/G4M/Module/Phantom/size 150. 150. 250.0 mm
/G4M/Module/Phantom/dim 150. 150. 250.
/G4M/Module/Phantom/material G4_WATER
/G4M/Module/install Phantom
#
# [MA] Beam Parameters
/My/PrimaryGenerator/select GPS
/control/execute ./macros/common/gps.mac
#
# Scoring
/My/runaction/dumpfile      A1.root
/My/runaction/ntuple/merge  true
#
# Track analysis
/My/runaction/ntuple/create    NT Phantom/HitsCollection 
/My/runaction/ntuple/addColumn NT evno I
/My/runaction/ntuple/addColumn NT pid  I
/My/runaction/ntuple/addColumn NT proc I
/My/runaction/ntuple/addColumn NT iz   I
/My/runaction/ntuple/addColumn NT de   F  keV
/My/runaction/ntuple/showScColumn NT
#
# BeamOn
/run/beamOn 100
#
```

#### 水ファントムモジュールの利用
水ファントム(G4MWaterPhantonクラス）は、水ファントムを想定して作成されたビームモジュールです。
内部をx,y,z方向にスライスしてボクセル化して内部での線量分布等を取得することができます。
以下に、使用における手順を説明します。

1. PreInit State\( /run/initializeコマンドの実行前\）に、水ファントムを`/Dynamic/Module/WaterPhantom/register`コマンドで登録します。引数は、モジュールに付ける固有名となります。この固有名は、後で説明する水ファントムの詳細設定コマンドの一部に使われます。つまり、複数の水ファントムを別名の固有名で導入することも可能です。

2. Idle State\( /run/initializeコマンドの実行後\）に、水ファントムの詳細設定や実体化を行います。
  2.1 `/G4M/Module/Phantom/size`コマンドで、水ファントムの各辺の半長とその単位を設定します。
  2.2 `/G4M/Module/Phantom/dim`コマンドで、x,y,z方向にボクセル分割します。<br> この例では2ミリ辺のボクセルとなります。
  2.3 `/G4M/Module/Phantom/material`コマンドで、水ファントムの物質を設定します。<br> デフォルトでは、物質名H_2Oが適用されます。NIST物質データベースのG4_WATERを使うように設定します。<br>また、水に限らず物質設定可能です。但し、予め`/G4M/Material/create`コマンで物質を作成してください。
  2.4 `/G4M/Module/install`コマンドで治療室（ワールドボリューム）にモジュールを配置します。<br> デフォルトではモジュール中心が原点となります。配置座標を指定する場合等は、`/G4M/Module`関連のコマンド[解説](../cmd-reference.md#beam-module-installation)を参照してください。

*水ファントムモジュールの固有コマンドについての詳細は[WaterPhantom Commands](../cmd-reference.md#waterphantom-commands)を参照してください。

#### 水ファントムモジュールでのスコアリング
水ファントムモジュールは、スコアリング機能がデフォルトで有効化されています。

- スコアリング情報へのアクセス名
シミュレーション実行中に一時保存されている粒子情報等を選択的に取り出して、ファイルに保存します。
このとき、一時保存されているコンテナを`HitsCollection｀と呼び、一つ一つの記録データの単位を`Hit`と呼びます。
水ファントムの`HitsCollection｀には、{モジュール名/HitsCollection}と名前付されています。
`Hit`には、ボクセルに入ってきたトラック毎の情報が格納されています。

*水ファントムモジュールの固有コマンドについての詳細は[WaterPhantom Commands](../cmd-reference.md#waterphantom-commands)を参照してください。


- スコア量のファイル保存
スコアリング結果は、root形式、csv形式、xml形式のいずれかの形式でファイル保存が可能です。
ヒストグラムやNtuple(別名TTree)のデータをして保存が可能です。ここではNtupleで保存する例を示します。
Ntupleとは、表データのようなもので、見出しとなるN個の情報を１行のデータ単位とし、情報が入る毎に行を追加していくデータ形式です。
以下に、Ntupleを使ってデータを保存する手順を説明します。

1. `/My/runaction/dumpfile`コマンドで、出力ファイル名を設定します。<br> このとき、ファイルの拡張子が保存形式となります。つまり、`.root`, `.csv`, `.xml`のいずれかです。

2. `/My/runaction/ntuple/merge`コマンドは、root形式を選択した場合のみ有効です。<br>シミュレーション実行はスレッド環境で行われます。それぞれ出力ファイルに`_t0`のようにスレッド番号が自動的に付加されます。Ntuple形式のデータ出力は、基本的に各スレッド毎にの各スレッドの出力ファイルに別々に保存されます。<br>
出力ファイル名の拡張子が`.root`である場合、このコマンドで`true`を設定すると、シミュレーション終了時にNtupleデータをメインスレッドの出力ファイルにマージして１つにまとめます。この場合は、各スレッド出力ファイルのNtupleデータはありません。実際、データ量が大きくなるとマージしたデータ量が数非常に大きくなるため、試験的に実行する場合のみtrueに設定することを推奨します。

3. `/My/runaction/ntuple/create`コマンドでNtupleを固有名\(例題では`NT`)を付けて宣言し、情報元のHitsCollectionとの対応付を行います。<br>HitsCollection名には、水ファントムモジュールの場合は、{モジュール名}/HitsCollectionとします。

4. `/My/runaction/ntuple/addColumn`コマンドで、Ntupleの固有名をしてして、保存するスコア量の名称と、保存時のデータ型\(`I`, `F`, `D`\)、そして単位がある場合には単位名を設定します。このコマンドをスコア量と変えて繰り返して、１行のデータ単位を定義します。選択可能な格納されている情報は、こちら[スコア量](../Scoring/NtupleQuantities.md)を参照してください。また、これらのコマンドで入力したNtupleデータのスコア項目を確認したいときは、`/My/runaction/ntuple/showScColumn`コマンドでNtuple名を指定すると表示されます。

#### シミュレーション結果データ作成のための実行
通常は、バッチモードで実行しますが、今回は、インタラクティブモードで実行してみましょう。
```
$ ./bin/PTSdemo  -u  tcsh  -i exampleA1.mac
```
PTSIMが起動して、マクロファイルに記載されたコマンドが実行されます。上記のマクロファイルでは`/run/beamOn 100`となっているので、100回の陽子線が水ファントムに向けて照射されます。照射回数が少ないので、10000回にしましょう。
```
Idle>  /run/beamOn 10000
```
実行が終わるり再度`Idle>`になったら終了します。
```
Idle> exit
```
  
#### CERN ROOT Analysis Toolでの解析
実行が終わると、`A1.root`という名前のファイルができています。
```
$ ls
..
A1.root
```
rootを起動してファイルを読み込みます。
```
$ root  A1.root
```
rootのコマンドモードに入ります。
今回は`NT`という名前のNtupleを作って情報を保存指定マウ。
下記のrootコマンドで、次のことを確認しています。
1. ファイルに`NT`が保存されているか
2. `NT`に含まれるデータ（ここではスコア名と呼びます）が何か
3. `NT`に記録されているデータの値をチラ見
```
root[] .ls
root[] NT->Print()
root[] NT-Scan()
```

次に`NT`き記録された情報で、グラフを作成します。
Ntupleでグラフを書くには、`Ntuple名->Draw()`を使います。
引数は次のようになります。
```
root[] Ntuple名->Draw("x軸スコア名","積算する値*(論理式)","表示モード")
root[] Ntuple名->Draw("x軸スコア名:y軸スコア名","積算する値*(論理式)","表示モード")
root[] Ntuple名->Draw("x軸スコア名:y軸スコア名:z軸スコア名","積算する値*(論理式)","表示モード")
```
例えば、深度線量分布を作成する場合は、横軸がizなどの位置情報、縦軸が線量の積算値です。
つまり、次のようになります。
```
root[] NT->Draw("iz","dose","hist")
```
今回、izの大きい番号側(250)から陽子線が入射しています。izを置き換えたグラフは次のようになります。
```
root[] NT->Draw("250-iz","dose","hist")
```

もし、初期粒子の反応(proc)の種類毎に線量を重ね書きする場合は、次のようになります。
```
root[] NT->Draw("250-iz","dose*(proc==0)","histsama")
root[] NT->Draw("250-iz","dose*(proc==1)","histsama")
root[] NT->Draw("250-iz","dose*(proc==2)","histsama")
```
Ntupleで上記のように表示した時のビンニングは、rootが自動的に決めてしまいますが、自分でヒストグラムを宣言してフィルしなおすことも可能です。
```
root[] h1 = new TH1D("h1"," title ", 250, 0, 250);
root[] NT->Draw("250-iz>>h1","dose","hist")
```
`TH1D`の引数は、ヒストグラムの固有名(フィルするときは、この固有名が使われる）、タイトル、ビン数、xmin, xmaxを表します。`TH2D`,`TH3D`も軸の設定が同様に追加されるだけで使い方は同じ感じです。


入射面（iz=250)での陽子線の個数をix-iy平面でみたい場合は、次のようになります。
```
root[] NT->Draw("iy:ix","pid=2212","colz")
```
2次元分布の描画方法には、`colz`の他に`lego`,`surf`,`box`などもあります。

rootを終了します。
```
root[] .q
```
