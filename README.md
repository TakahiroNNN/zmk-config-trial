[torabo-tsuki LP](https://github.com/sekigon-gonnoc/torabo-tsuki-lp)用のZMKファームウェア

* _centralがついているuf2をトラックボールがついている方に、_peripheralを反対側に書き込んでください
  * 右側トラックボールで組み立てた場合は、右手がcentral、左手がperipheralです
  * トラックボールを使わず右手トラックパッドだけで使う場合は、右手に`*_right_trackpad_central.uf2`を書き込んでください
* USBでキーボードとして認識されるのはcentral側です。peripheral側だけをUSB接続しても入力デバイスとしては動きません
* キーマップはkeymap-editorおよびzmk-studioで編集できます

## 左に4方向スイッチ、右にトラックボールを使う場合

この構成では、次の **2つをセットで** 書き込んでください。右側も4方向スイッチの5入力を含むレイアウトでビルドする必要があります。

| 書き込み先 | ファイル |
| --- | --- |
| 左手（4方向スイッチ、peripheral） | `torabo_tsuki_lp_left_4_direction_peripheral.uf2` |
| 右手（トラックボール、central） | `torabo_tsuki_lp_right_trackball_4_direction_central.uf2` |

上下左右はそれぞれ同じ方向へのスクロール、中央押し込みはマウス右クリックです。全4レイヤーで同じ動作をします。倒している間はスクロールが継続します。

ZMK Studioでは、動作の選択欄に **Scroll Up / Scroll Down / Scroll Left / Scroll Right / Right Click** が個別に表示されます。この5項目は追加の数値入力なしで選択できます。ZMK v0.3の標準 `Mouse Scroll` にはStudio用の方向パラメーター情報がないため、この構成では方向を固定した動作として登録しています。標準のマウスボタン動作を直接使う場合、右クリックのボタン値は **MB2** です。

既存のM Layout・52キーの割り当て、レイヤー1/2へのモーメンタリ切り替え、Bluetoothクリアのコンボ、右トラックボール操作時のレイヤー3への一時切り替えは維持しています。4方向スイッチの5入力だけをキーマップ末尾（位置52〜56）へ追加しています。通常構成のuf2も引き続き生成します。

### ビルドと書き込み

1. GitHub Actionsの `Build ZMK firmware` が成功した実行から、`firmware` アーティファクトをダウンロードして展開します。
2. 左右をそれぞれブートローダーモードにして、上の表にあるuf2を書き込みます。
3. 右手側をUSB接続し、既存キーと4方向スクロール・右クリックを確認します。
4. ZMK Studioで以前キーマップを保存したことがある場合は、右手側へ接続して **Restore Stock Settings** を実行します。画面上部の接続中キーボード名（`torabo-tsuki` の横の下向き矢印）をクリックし、メニューの **Restore Stock Settings** を選び、確認画面でも同名のボタンを押します。この操作はStudioで保存した割り当てを消し、ファームウェアに埋め込んだ現在のGitHubのキーマップと追加5入力へ戻します。物理レイアウトは **M Layout** を使ってください。

Studioで動作を選んでも別のキーをクリックしたときに元へ戻る場合は、動作に必要なパラメーターが未選択の可能性があります。4方向スイッチには上記5項目を使ってください。キー配置図の追加5入力が上・下・左・右・押し込みの順に割り当てられていることを確認し、変更後は **Save** を押します。入力が一方向しか反応しない場合は、Studio上の編集可否とは別に実機の入力確認が必要です。

通常の更新では `settings_reset` は不要です。左右が接続できない場合のみ、後述のリセット手順を使ってください。ZMK Studioで保存したキーマップは通常の書き込みでは消えないため、キーマップの反映には上記のStudio側の操作を使います（[ZMK公式ドキュメント](https://v0-3-branch.zmk.dev/docs/features/studio)）。

4方向スイッチは左側のFPC端子に接続します。同じ左側の端子へトラックボールやトラックパッドを同時接続する構成には対応しません。GPIOは提供元の定義に合わせ、上=P0.19、下=P0.18、左=P0.19とP0.18の同時入力、右=P0.16、押し込み=P0.20です。方向は[モジュールの説明](https://github.com/sekigon-gonnoc/4-direction-switch-module#ピン配置)にある取り付け向きが基準です。

### 取り込んだ提供元の更新

2026-11-08時点の[提供元の最新ソース](https://github.com/sekigon-gonnoc/zmk-keyboard-torabo-tsuki-lp/tree/c3dfa3c963456b550845b29cf01f549531a0c05e)から、共通レイアウト定義と4方向スイッチ用スニペットを取り込んでいます。提供元の標準L Layoutではなく、このリポジトリのM Layoutを既定のまま維持しています。

ZMKは提供元に合わせて `v0.3` を使います。[最新リリース](https://github.com/sekigon-gonnoc/zmk-keyboard-torabo-tsuki-lp/releases/tag/latest)に記載された2026-08-15の省電力更新は、既存のBMP Boost `v0.2` に含まれています。そのコミット `7dc3f9e4d344db0d1916170ac1a87a937dd717d9` を明示して固定しました。追加ドライバも `2329edb80a589fdf4245b4edace0e9c1d3424b83` に固定しています。

## 書き込み前のリセット

左右が接続できないなど、BLE接続情報の初期化が必要な場合のみ、`settings_reset-bmp_boost-zmk.uf2`を両側へ一度ずつ書き込んでください。保存済みのペアリング情報は消去されます。

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

## 右手トラックパッド単体で使う場合

左手側を使わず、右手側のミニトラックパッドだけをUSB/BLEデバイスとして使う場合は、`peripheral`ではなく`central`のuf2を書き込んでください。

* スクロール用: `torabo_tsuki_lp_right_trackpad_central.uf2`
* カーソル移動用: `torabo_tsuki_lp_right_trackpad_pointer_central.uf2`

左手側をキー入力用に追加する場合だけ、左手に`torabo_tsuki_lp_left_peripheral.uf2`を書き込んでください。
