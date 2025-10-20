### Digitizerモジュールの時間ウインドウ設定
selectで選択したdigitizerモジュールに時間ウインドウを設定する
ただし、DigiCoincidenceのときのみ有効となる。
#### 時間/エネルギーウインドウを定
```
/My/eventaction/coincidence/twin  {tw:d}  {toff:d} {unit:s}
/My/eventaction/coincidence/ewin  {emean:d}  {ew:d} {unit:s}
```
| 値表記 | 説明 |
|:---|:---|
| tw:d  | 時間ウインドウ |
| toff:d | 信号から時間ウインド開始までの時間オフセット |
| unit:s | 単位 |
| emean:d | エネルギー中心値 |
| ew:d | エネルギー幅（emean+-ew） |
| unit:s |  単位 |
