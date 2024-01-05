# JSONの書式

https://www.caniusevia.com/docs/specification

## matrix
オブジェクト形式。`rows`キーに行数、`cols`キーに列数を指定。

The matrix property defines how many rows and columns the PCB's switch matrix uses. This must match the MATRIX_ROWS and MATRIX_COLS symbols in the QMK firmware.

## labels
キーマップのバリエーションを表現するための選択肢を作成するキー。**文字列または文字列配列**の配列形式。文字列の場合はトグルスイッチ、文字列配列の場合はプルダウンリストになる。

後述する`keymap`で配列のインデックスが暗黙的に使用されるので並び順は重要。

## keymap
キー行ごとの配列形式。

### string: "y, x"
キーマップ座標。行番号は`0`から始まることに注意！

`matrix`に合わせて設定していく。最初に`y`座標、次に`x`座標を指定する。

後述するオブジェクト表現のオプション指定は最後に出現した`"y, x"`のキーマップ座標に作用する。

### string: "[y, x]\n\n\n[配列インデックス],[状態]"
`labels`で設定したバリエーションの有効、無効状態によって`[y, x]`で指定したキーマップ座標の有効、無効を切り替える書式。

`[配列インデックス]`には`labels`で指定した`0`から始まる配列のインデックスを取る。並び順が重要と書いたのはこのため。

`[状態]`にはトグルスイッチの場合は`0, 1`を取る。プルダウンリストの場合はリスト項目数によって取りうる値は変わる。`0`が最初の項目なので注意。

`[状態]`が`true`の場合はキーマップが有効になる。ペアになる`false`時のキーマップ座標を別で設定していないとクラッシュする。

### オブジェクトプロパティ(boolean): d
キーマップを無効化して空白にする設定。ケースにより物理的に配置不能なキーに対して設定しておく使い方ができる。

### オブジェクトプロパティ(float): x
物理的なキー座標の設定。キーのX軸(横)ポジションを設定する。ブロッカーなどの隙間を空けたりするために使用する。

### オブジェクトプロパティ(float): w
キーの幅の設定。とてもややこしいのが、1行の中で`w`キーの合計値が2以上になるとキーマップ座標をその分スキップしないといけない。

4行目シフトキー(w: 2.25)によって座標をスキップする例:
```json
{ "w": 2.25 }
"3,0",
"3,2", ←キー幅 1 (未指定)の場合は"3, 1"になるはずだが、シフトキーが"3, 1"を跨ぐので"3, 2"になる。
...
```

### オブジェクトプロパティ(HEXカラーコードstring): t
キーカラー。設定したキーマップ座標から同じ行で別の`t`のカラー指定が出現するまで連続して右に反映される。

### オブジェクトプロパティ(HEXカラーコードstring): c
レジェンドカラー。コレ反映されてるのか分からん。仕様は`t`と同じなはず。
