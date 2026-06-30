[torabo-tsuki LP](https://github.com/sekigon-gonnoc/torabo-tsuki-lp)用のZMKファームウェア

* _centralがついているuf2をトラックボールがついている方に、_peripheralを反対側に書き込んでください
  * 右側トラックボールで組み立てた場合は、右手がcentral、左手がperipheralです
* USBでキーボードとして認識されるのはcentral側です。peripheral側だけをUSB接続しても入力デバイスとしては動きません
* キーマップはkeymap-editorおよびzmk-studioで編集できます

## 書き込み前のリセット

既存のファームウェアやBLE接続情報が残っている場合、先に`settings_reset-bmp_boost-zmk.uf2`を両側へ一度ずつ書き込んでください。

1. 左右それぞれに`settings_reset-bmp_boost-zmk.uf2`を書き込む
2. 数秒待ってから、使いたい構成のuf2を書き込む
3. BLEで使う場合は、PC/スマートフォン側に残っている古い`torabo-tsuki`のペアリング情報を削除してから再ペアリングする

反応しない場合は、まずトラックパッドなしの通常構成でcentral側をUSB接続してキー入力できるか確認してください。

* 右がトラックボール: `torabo_tsuki_lp_right_central.uf2` と `torabo_tsuki_lp_left_peripheral.uf2`
* 左がトラックボール: `torabo_tsuki_lp_left_central.uf2` と `torabo_tsuki_lp_right_peripheral.uf2`

## ミニトラックパッドを使う場合

トラックボール側には`*_trackball_central`、ミニトラックパッド側には`*_trackpad_peripheral`のuf2を書き込んでください。`*_trackpad_peripheral`は高分解能スクロール用なので、カーソル移動ではなくスクロール入力として動作します。

* 右がトラックボール、左がミニトラックパッド: `torabo_tsuki_lp_right_trackball_central.uf2` と `torabo_tsuki_lp_left_trackpad_peripheral.uf2`
* 左がトラックボール、右がミニトラックパッド: `torabo_tsuki_lp_left_trackball_central.uf2` と `torabo_tsuki_lp_right_trackpad_peripheral.uf2`

カーソル移動として試したい場合は、ミニトラックパッド側だけ`*_trackpad_pointer_peripheral.uf2`を書き込んでください。central側は同じ`*_trackball_central.uf2`を使います。

* 右がトラックボール、左がミニトラックパッド: `torabo_tsuki_lp_right_trackball_central.uf2` と `torabo_tsuki_lp_left_trackpad_pointer_peripheral.uf2`
* 左がトラックボール、右がミニトラックパッド: `torabo_tsuki_lp_left_trackball_central.uf2` と `torabo_tsuki_lp_right_trackpad_pointer_peripheral.uf2`

トラックパッドだけ反応しない場合は、まずスクロールできる画面で`*_trackpad_peripheral.uf2`を試してください。トラックパッド側のキー入力も反応しない場合は、peripheralがcentralへ接続できていないため、両側へ`settings_reset-bmp_boost-zmk.uf2`を書き込んでから再度ペアリングしてください。
