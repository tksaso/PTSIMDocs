# Example A1(ファントム/スコア）

シミュレーション空間への水ファントムの配置とスコアリングについて解説します。
水ファントムのビームモジュール`G4MWaterPhantom`クラスは、３次元メッシュ化されたボクセル構造を持ち、各ボクセルでのスコアリングが可能です。

**解説概要**  
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
ビームを打ってみます。
```
Session: /run/beamOn 10
```
治療室の原点\(アイソセンター\)に水ファントムが配置されたシミュレーション空間です。
![exampleA1](../images/exampleA1.png)

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
/control/verbose 1
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
/G4M/Module/select Pantom
/G4M/Module/rotate 0. 180. 0. degree
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
/My/runaction/ntuple/addColumn NT ix   I
/My/runaction/ntuple/addColumn NT iy   I
/My/runaction/ntuple/addColumn NT iz   I
/My/runaction/ntuple/addColumn NT de   F  keV
/My/runaction/ntuple/showScColumn NT
#
# BeamOn
#/run/beamOn 100
#
```

#### 水ファントムモジュールの利用

水ファントム(G4MWaterPhantomクラス）は、水ファントムを想定したビームモジュールです。
内部をx,y,z方向にスライスしてボクセル化して3次元線量分布等を取得することができます。

以下に、使用における手順を説明します。

1. PreInit Stateで、水ファントムをシステムに登録する。<br>
PreInit Stateとは、`/run/initialize`を実行する前までの状態です。  
`/Dynamic/Module/WaterPhantom/register`コマンドで登録を行います。引数は、このモジュールに付ける固有名となります。登録後であっても、この固有名を指定してモジュールの設定を変更することが可能です。後ほど説明する水ファントムの詳細設定コマンドでは、コマンドの一部に固有名が自動的に使われています。そのため、異なる固有名で複数の水ファントムを導入し、個別に設定することも可能です。  

2. Idle Stateで、水ファントムの詳細設定や実体化を行う。<br>
Idle Stateとは、`/run/initialize`を実行した後の状態です。  
2.1 `/G4M/Module/Phantom/size`コマンドで、各辺の半長とその単位を設定します。  
2.2 `/G4M/Module/Phantom/dim`コマンドで、x,y,z方向にボクセル分割します。この例では、2ミリ辺のボクセルとなります。  
2.3 `/G4M/Module/Phantom/material`コマンドで、水ファントムの物質をG4_WATERに設定しています。デフォルトでも、G4_WATERが適用されます。水以外の物質を設定することが可能ですが、予め`/G4M/Material/create`コマンドで物質作成をしておく必要があります。
2.4 `/G4M/Module/select Phantom`でモジュールを選択し、`/G4M/Module/rotate  0. 180. 0. degree`でy軸の周りに180度回転して配置しています。ボクセル番号は、各軸のマイナス方向からプラス方向にインデックス番号が割り振られます。そのため、ビーム入射面にz軸のインデックス番号0が来るように回転をさせています。

*水ファントムモジュールの固有コマンドについての詳細は[コマンドリファレンス](../cmd-reference.md#waterphantom-commands)の[WaterPhantom Commands](../BeamModules/WaterPhantom.md)参照してください。
 
3. `/G4M/Module/install`コマンドで治療室（ワールドボリューム）にモジュールを配置します。<br>
 デフォルトではモジュール中心が原点となるように配置されます。
*モジュールにおいて、配置座標を指定したい場合は、`/G4M/Module/select`、`/G4M/Module/translate`、`/G4M/Module/rotate`などのコマンドを利用します。詳細は[コマンドリファレンス](../cmd-reference.md#beam-module-installation)を参照してください。


#### 水ファントムモジュールでのスコアリング
水ファントムモジュールは、スコアリング機能がデフォルトで有効化されています。

- スコアリング情報へのアクセス<br>
シミュレーション実行中にスコアリングされた粒子情報は、一時保存されています。この一時保存されているコンテナ（入れ物）を`HitsCollection`と呼び、その中に格納された記録データの単位を`Hit`と呼びます。`Hit`には、ボクセルに入ってきたトラック毎の情報が格納されています。PTSIMでは、この`HitsCollection`から情報を選択的に取り出して、ファイルに保存することができます。複数のモジュールが一時保存する`HitsCollection`を区別するために、<br> `{モジュール名/HitsCollection}` <br> と名前が付されています。上記の例題マクロでは、`Phantom/HitsCollection`となります。

- スコア量のファイル保存<br>
`HitsCollection`から取り出したスコアリング結果は、root形式、csv形式、xml形式のいずれかの形式でファイル保存が可能です。
ヒストグラムやNtuple(別名TTree)のデータ形式で保存が可能です。ここではNtuple形式で保存する例を示します。Ntupleとは、N個の情報を１行のデータ単位とし、情報が入る毎に行データを追加していくリストモードを想像してください。

以下に、Ntupleを使ってデータを保存する手順を説明します。

1. `/My/runaction/dumpfile`コマンド<br>
出力ファイル名を設定します。このとき、ファイルの拡張子が保存形式となります。拡張子は、`.root`, `.csv`, `.xml`のいずれかです。

2. `/My/runaction/ntuple/merge`コマンド <br>
(注)root形式を選択時のみ有効です。 <br>
スレッド環境でシミュレーション実行が行われているとき、出力ファイルがスレッド毎に生成され、ファイル名に`_t0`のようにスレッド番号が自動的に付加されます。Ntupleデータは、各スレッドの出力ファイルに別々に保存されます。<br>
出力ファイル名の拡張子が`.root`である場合のみ、このコマンドが有効です。`true`を設定すると、シミュレーション終了時にNtupleデータをメインスレッドの出力ファイルにマージして１つに集約します。そして、各スレッド出力ファイルのNtupleデータはマージ後に自動削除されます。<br>
マージすることで、複数のファイルを解析する必要がなくなりますが、その反面でスレッド毎のNtupleデータが大きい場合、マージしたデータは当然ながら巨大になります。動作確認など、試験的に実行する場合に限ってtrueに設定するのが良いでしょう。

3. `/My/runaction/ntuple/create`コマンド<br> Ntupleの固有名(例題では`NT`)を宣言し、情報元の`HitsCollection`とのリンク付けを行います。例題では`Phantom/HitsCollection`となります。

4. `/My/runaction/ntuple/addColumn`コマンド<br> Ntupleの固有名を指定して、保存するスコア量の名称と、保存時のデータ型\(`I`, `F`, `D`\)、そして単位を設定する場合には単位名を設定します。このコマンドを必要なスコア量を指定して繰り返し、Ntuple１行分のデータ単位を定義します。選択可能な格納されている情報は、こちら[スコア量](../Scoring/NtupleQuantities.md)を参照してください。また、これらのコマンドで入力したNtupleデータのスコア項目を確認したいときは、`/My/runaction/ntuple/showScColumn`コマンドでNtuple名を指定すると表示されます。

#### シミュレーション結果データ作成のための実行
通常は、バッチモードで実行しますが、今回は、インタラクティブモードで実行してみましょう。
```
$ ./bin/PTSdemo  -u  tcsh  -i exampleA1.mac
```
PTSIMが起動して、マクロファイルに記載されたコマンドが実行されます。今回は`/run/beamOn 10000`で実行してみましょう。
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
今回は`NT`という名前のNtupleを作って情報を保存指定ます。
下記のrootコマンドで、次のことを確認しています。
1. ファイルに`NT`が保存されているかを確認 (`.ls` )
2. `NT`に含まれるデータ（ここではスコア名と呼びます）が何かを確認 (`NT->Print()`)
3. `NT`に記録されているデータの値をチラ見してみる。(`NT->Scan()`) <br> 終了は`q`.

```
root[] .ls
TFile**		A1.root	
 TFile*		A1.root
  KEY: TTree	NT;1	NT

root[] NT->Print()
******************************************************************************
*Tree    :NT        : NT                                                     *
*Entries :  1237495 : Total =        49657146 bytes  File  Size =   13231595 *
*        :          : Tree compression factor =   3.75                       *
******************************************************************************
*Br    0 :evno      : Int_t NT                                               *
*Entries :  1237495 : Total  Size=    9931672 bytes  File Size  =    1598502 *
*Baskets :      311 : Basket Size=      32000 bytes  Compression=   6.21     *
*............................................................................*
*Br    1 :pid       : Int_t NT                                               *
*Entries :  1237495 : Total  Size=    9931358 bytes  File Size  =    1673710 *
*Baskets :      311 : Basket Size=      32000 bytes  Compression=   5.93     *
*............................................................................*
*Br    2 :proc      : Int_t NT                                               *
*Entries :  1237495 : Total  Size=    9931672 bytes  File Size  =    1616101 *
*Baskets :      311 : Basket Size=      32000 bytes  Compression=   6.14     *
*............................................................................*
*Br    3 :iz        : Int_t NT                                               *
*Entries :  1237495 : Total  Size=    9931044 bytes  File Size  =    2311829 *
*Baskets :      311 : Basket Size=      32000 bytes  Compression=   4.29     *
*............................................................................*
*Br    4 :de        : Float_t NT                                             *
*Entries :  1237495 : Total  Size=    9931044 bytes  File Size  =    6020680 *
*Baskets :      311 : Basket Size=      32000 bytes  Compression=   1.65     *
*............................................................................*

root[] NT-Scan()
************************************************************************
*    Row   * evno.evno *   pid.pid * proc.proc *     iz.iz *     de.de *
************************************************************************
*        0 *         0 *      2212 *         0 *       249 * 906.37304 *
*        1 *         0 *      2212 *         0 *       248 *  1031.677 *
*        2 *         0 *      2212 *         0 *       247 * 788.84240 *
*        3 *         0 *      2212 *         0 *       246 * 837.16821 *
*        4 *         0 *      2212 *         0 *       245 * 820.92474 *
*        5 *         0 *      2212 *         0 *       244 * 807.99725 *
....snippet
Type <CR> to continue or q to quit ==>  q

root[]
```

次に`NT`き記録された情報で、グラフを作成します。
Ntupleでグラフを書くには、`Ntuple名->Draw()`を使います。
引数は次のようになります。
```
root[] Ntuple名->Draw("x軸スコア名","積算する値*(論理式)","表示モード")
root[] Ntuple名->Draw("x軸スコア名:y軸スコア名","積算する値*(論理式)","表示モード")
root[] Ntuple名->Draw("x軸スコア名:y軸スコア名:z軸スコア名","積算する値*(論理式)","表示モード")
```
例えば、深度エネルギー付与分布を作成する場合は、横軸が`iz`などの位置情報、縦軸がエネルギー付与`de`の積算値です。
つまり、次のようになります。
```
root[] NT->Draw("iz","de","hist")
```
![../images/exampleA1izde1.png](../images/exampleA1izde1.png)


今回、izの大きい番号側(250)から陽子線が入射しています。izを置き換えたグラフは次のようになります。また、Ntupleで上記のように表示した時のビンニングは、rootが自動的に決めてしまいますが、自分でヒストグラムを宣言してフィルしなおすことも可能です。
```
root[] auto h1 = new TH1D("h1"," title ", 250, 0, 250);
root[] NT->Draw("iz>>h1","de","hist")
```
![../images/exampleA1izde2.png](../images/exampleA1izde2.png)

`TH1D`の引数は順番に、ヒストグラムの固有名(フィルするときは、変数名ではなく、この固有名を指定します）、タイトル、ビン数、xmin, xmaxを表します。`TH2D`,`TH3D`も軸の設定が同様に追加されるだけで使い方は同じ感じです。

初期粒子の反応(proc)の種類毎に線量を重ね書きする場合は、次のようになります。<br>
最後の引数にある`histsame`は、`hist`表示で重ね書き`same`を指示しています。
```
root[] NT->Draw("iz","de*(proc==0)","histsame")
root[] NT->Draw("iz","de*(proc==1)","histsame")
root[] NT->Draw("iz","de*(proc==2)","histsame")
root[] NT->Draw("iz","de*(proc==3)","histsame")
root[] NT->Draw("iz","de*(proc==4)","histsame")
root[] NT->Draw("iz","de*(proc==5)","histsame")
```
*`proc`の番号の意味は、[こちら](../Scoring/ProcessType.md)

![../images/exampleA1izde3.png](../images/exampleA1izde3.png)

入射面（iz=0)での陽子線の*個数*をix-iy平面でみたい場合は、次のようになります。
```
root[] NT->Draw("iy:ix","iz==0&&pid==2212","colz")
```
![../images/exampleA1ixiy.png](../images/exampleA1ixiy.png)

2次元分布の描画方法には、`colz`の他に`lego`,`surf`,`box`などもあります。

rootを終了します。
```
root[] .q
```
