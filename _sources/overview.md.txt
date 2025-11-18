# Overview

## What's PTSIM
PTSIM, Particle Therapy System Simultor Frameworkは、[Geant4](https://www.geant4.org)の医学分野応用として、主に粒子線医療での線量計算を行うために開発されたGeant4アプリケーションです。次のような特徴があります。

- シミュレーション条件を設定するユーザインターフェイスコマンドが提供さている
  - Geant4コード開発が不要
- 粒子線治療で使用されるビーム機器をモジュールとして提供している
  - ビーム機器を組みあわてビームラインを構成可
  - パラメタ変更によって形状変更が可能
  - DICOM-CTデータを患者形状としてインポート可能
  - ICRP145メッシュデータもインポート可能
  - GDML, Geometry Description Markup Language利用による独自機器の導入が容易
- 柔軟なスコアリングが可能
  - 線量分布などの既定値だけでなく、解析用のトラックデータを取出し可能
  - 1D ~ 3Dヒストグラム、2D,3Dプロファイルの物理量分布を作成可能
- マルチスレッド対応

PTSIM開発の原点は、粒子線治療における精密線量計算ですが、一般的な放射線取扱の広範囲な分野に適用可能なソフトウェアです。

PTSIMの情報は、下記のリンクを参照してください。
[KEK Wiki PTSIM page](https://wiki.kek.jp/spaces/g4med/pages/5343876/PTSIM)

![logo](../images/PTSIMlogo2.png)   


