# カメラでモニタの切り替えを監視して適切なディスプレイ設定にしてくれる機能

## 概要

カメラを使ってモニタ（ディスプレイ）への入力切替を検知し、状況（在宅勤務 / プライベート）に応じたディスプレイ設定を自動で適用する機能。

## 背景・モチベーション

在宅勤務とプライベートで同一モニタを共用している場合、PC切替器（KVMスイッチ）や入力切替ボタンで接続先を変えるたびに、輝度・色温度・カラープロファイル・Night Shiftの有効化などの設定を手動で変更する必要があり、煩雑。

## 解決方法のアイデア

1. **カメラによる検知**
   - PCに接続されたカメラ（Webカメラ等）で定期的に画面を撮影する
   - 撮影した画像から「現在どのPCの画面が表示されているか」を判定する
     - 例：画面の配色・レイアウト・ウィンドウの特徴などを機械学習で分類
     - あるいはモニタのOSD（On-Screen Display）切替音・輝度変化などを手がかりにする

2. **モニタ切替の検知方法（代替案）**
   - DDC/CI（Display Data Channel / Command Interface）でモニタの入力ソース情報を取得する
   - モニタのUSBハブ経由の接続状態変化を監視する

3. **設定の自動適用**
   - 検知した入力ソース（仕事用 PC / プライベート PC）に応じて、あらかじめ登録したプロファイルを自動適用する
   - 設定例：
     - 仕事用：輝度低め・色温度高め（ブルーライトカット）・Night Shift オン
     - プライベート：輝度高め・色温度低め（鮮やか）・Night Shift オフ

## 想定される技術スタック

- Python（OpenCV / scikit-learn / TensorFlow Lite など）
- DDC/CI 制御ライブラリ（`ddcutil` on Linux、`monitorcontrol` on Python など）
- macOS：`displayplacer`、`lunar` CLI など
- Windows：WMI / `ControlMyMonitor` など

## 参考

- [DDC/CI - Wikipedia](https://en.wikipedia.org/wiki/Display_Data_Channel#DDC/CI)
- [monitorcontrol (Python)](https://github.com/newAM/monitorcontrol)
- [Lunar (macOS)](https://lunar.fyi/)
