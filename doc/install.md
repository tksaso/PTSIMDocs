# Installation and Build


## 必要な環境

PTSIMの実行は、Linux環境またはMac環境を前提としています。ウインドウズの場合はWSL環境を用意してください。
また、gccなどの開発環境やGeant4に加えて、次のライブラリ等のインストールを推奨しています。

- 可視化やGUI環境: QtやOpenGLのインストール
- 独自のビーム機器をGDMLで記述: XMLパーサ
- データ解析：CERN Analysis ROOT

これらのインストールについては、以下の手引きを参考にしてください。

但し、OSバージョン等により手順が変わる場合があります。  
[WSLでのインストール Geant4-v11.3](./Geant4/installWSL_Geant4_113.pdf)  
[Macでのインストール Geant4-v11.3](./Geant4/installMac_Geant4_113.pdf)  

## PTSIMソースコード

PTSIMのソースコードをKEKのWikiサイトからダウンロードしてください。

[KEK Wikiサイト eant4医学応用](https://wiki.kek.jp/spaces/g4med/pages/5343876/PTSIM)

## ソースコードの展開とビルド

### 展開
ソースコードが、ホームディレクトリの下にあるDownloadsディレクトリにあり、ホームディレクトリに展開するものとして説明します。下記、xxxの部分は、PTSIMのバージョンによって読み替えてください。
```
$ cd
$ tar  zxvf  ~/Downloads/PTSproject-11x-xxx-xxx.tar.gz
```

### ビルド
はじめにライブラリをビルドします。
```
$ cd
$ cd PTSproject-11x-xxx-xxx
$ ./buildToolkitIAEAGDML.sh
```

次にアプリケーションをビルドします。
```
$ ./buildDynamicIAEAGDML.sh
```

実行ディレクトリに移ります
```
$ cd  ~/PTSproject-install/PTSapps/DynamicPort
$ ls bin/
```
lsコマンドで次の表示があればインストールが完了しています。
```
PTSdemo
```

### 例題実行

例題のマクロファイルをコピーして実行してみましょう
```
$ cp  ./macros/DynamicPort/Sample0.mac  .
$ ./bin/PTSdemo  -i  Sample0.mac
```

Qtウインドウが起動して、OpenGLで例題の粒子線治療装置のモデルが表示されます。
<!-- ![実行画面](./img/ptsim1.png) -->

ウインドウの下にある「Session:」に次のように入力します。
```
Session:  /run/beamOn 10
```
陽子線治療のシミュレーションが実行されます。
<!-- ![実行画面](./img/ptsim2.png)  -->

終了します。
```
Session:  exit
```

