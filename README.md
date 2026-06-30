[torabo-tsuki LP](https://github.com/sekigon-gonnoc/torabo-tsuki-lp)用のZMKファームウェア

* _centralがついているuf2をトラックボールがついている方に、_peripheralを反対側に書き込んでください
* USBでキーボードとして認識されるのはcentral側です。peripheral側だけをUSB接続しても入力デバイスとしては動きません
* キーマップはkeymap-editorおよびzmk-studioで編集できます

## 書き込み前のリセット

既存のファームウェアやBLE接続情報が残っている場合、先に`settings_reset-bmp_boost-zmk.uf2`を両側へ一度ずつ書き込んでください。

1. 左右それぞれに`settings_reset-bmp_boost-zmk.uf2`を書き込む
2. 数秒待ってから、使いたい構成のuf2を書き込む
3. BLEで使う場合は、PC/スマートフォン側に残っている古い`torabo-tsuki`のペアリング情報を削除してから再ペアリングする

反応しない場合は、まずトラックパッドなしの通常構成でcentral側をUSB接続してキー入力できるか確認してください。

* 左がトラックボール: `torabo_tsuki_lp_left_central.uf2` と `torabo_tsuki_lp_right_peripheral.uf2`
* 右がトラックボール: `torabo_tsuki_lp_right_central.uf2` と `torabo_tsuki_lp_left_peripheral.uf2`

## ミニトラックパッドを使う場合

トラックボール側には`*_trackball_central`、ミニトラックパッド側には`*_trackpad_peripheral`のuf2を書き込んでください。

* 左がトラックボール、右がミニトラックパッド: `torabo_tsuki_lp_left_trackball_central.uf2` と `torabo_tsuki_lp_right_trackpad_peripheral.uf2`
* 右がトラックボール、左がミニトラックパッド: `torabo_tsuki_lp_right_trackball_central.uf2` と `torabo_tsuki_lp_left_trackpad_peripheral.uf2`
