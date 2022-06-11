---
title: "Tang PriMER Install Problem"
date: 2022-05-07T17:57:05+09:00
draft: false
tags : [
    "Tang PriMER"
]

---

2011年ごろのPCにWindow10 Homeをクリーンインストールして[SiPEEDTang PriMER公式サイトのGetting Started](https://tang.sipeed.com/en/getting-started/)で環境を作ったら２つエラーが発生しました。ここでは以下２つの解決方法を書きます。

- msvc110.ddlのインストール方法
- ライセンスファイルの更新

## 2022/05/07時点の最新のファイル名

公式の手順では明記されてませんが、2022/05/07現在以下が最新のようです。ライセンスは定期的に更新されているようです。

- Anlogic_20220703.lic
- TD_RELEASE_March2020_r4.6.4_64bit.msi

---

## msvc110.dllがないエラー

最初に公式サイトに従いTang Dinasty IDE(=TD)をインストールします。インストールは問題なくできますが、起動するとmsvc110.dllがないというエラーが発生します。

対応としては、Microsoft公式にある[Microsoft Visual C++ 2010 Service Pack 1 再頒布可能パッケージ MFC のセキュリティ更新プログラム](https://www.microsoft.com/ja-jp/download/details.aspx?id=26999) をインストールします。赤いダウンロードボタンを押すと、次のダイアログ画面で、`vcredist_x64.exe`が選択できます。これをインストールするとTDが起動できます。

## 未署名のデバイスドライバーのインストール

これは手順通りにやれば問題ないです。

```bat
bcdedit.exe -set loadoptions DISABLE_INTEGRITY_CHECKS
bcdedit.exe -set TESTSIGNING ON
```

**UEFIの場合はセキュアブートの無効も必要だそうです。**

---

## `RUN-003 ERROR: License expired!`ライセンス期限切れエラー

GitHubのサンプルプロジェクトをビルドしてTangPriMERで動かす時に、`RUN-003 ERROR: License expired!`ライセンス期限切れエラーが発生します。

対応としては、Anlogic_20220703.licをAnlogic.licにしてC:\Anlogic\TD4.6.4\license\に
配置します
