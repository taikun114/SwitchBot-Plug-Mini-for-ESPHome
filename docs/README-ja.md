# SwitchBot プラグミニ for ESPHome
[English](/README.md) | **日本語**

![banner](images/banner.png)

このリポジトリには、SwitchBotが発売している電力モニタリング機能付きスマートプラグ「プラグミニ」を[**ESPHome**](https://esphome.io/)（と[**Home Assistant**](https://www.home-assistant.io/)）で使用するための手順や設定ファイルが含まれています。

> [!CAUTION]
> 作業は全て**自己責任で**行うようにしてください。
>
> SwitchBot プラグミニをESPHomeで使う事は**SwitchBot公式の使い方ではありません**ので、**SwitchBotからのいかなるサポートも受けられなくなります。**
>
> また、ここで紹介している手順ではファームウェアを書き換える作業があり、**手順を誤ったり失敗したりしてしまうと製品を破損させてしまう可能性**があります。
>
> どのような状況になったとしてもご自身で対処できる場合に限り作業を行われることをおすすめします。


## 目次
- [対応機種](#対応機種)
  - [動作することを確認している機種](#動作することを確認している機種)
  - [動作する可能性が高い機種](#動作する可能性が高い機種)
- [SwitchBot プラグミニをESPHomeで使えるようにする](#SwitchBot-プラグミニをESPHomeで使えるようにする)
  - [事前準備: ESPHome用のファームウェアをダウンロードする](#事前準備-ESPHome用のファームウェアをダウンロードする)
    - [ファームウェアの種類](#ファームウェアの種類)
  - [手順1. SwitchbOTAを使ってTasmotaファームウェアをインストールする](#手順1-SwitchbOTAを使ってTasmotaファームウェアをインストールする)
  - [手順2. ESPHomeファームウェアをインストールする](#手順2-ESPHomeファームウェアをインストールする)
  - [手順3. Wi-Fiネットワークの設定を行う](#手順3-Wi-Fiネットワークの設定を行う)
    - [フォールバックホットスポットからWi-Fiネットワークを設定する](#フォールバックホットスポットからWi-Fiネットワークを設定する)
    - [Improv via BLEを使ってWi-Fiネットワークを設定する](#Improv-via-BLEを使ってWi-Fiネットワークを設定する)
  - [手順4. ESPHomeの設定を行う](#手順4-ESPHomeの設定を行う)
    - [ESPHome Device Builderをインストールする](#ESPHome-Device-Builderをインストールする)
    - [デバイスの完全な制御を得る](#デバイスの完全な制御を得る)
  - [手順5. 最初に変更しておくべき設定](#手順5-最初に変更しておくべき設定)
    - [Wi-Fiネットワークに関する設定](#Wi-Fiネットワークに関する設定)
    - [Improv via BLEに関する設定](#Improv-via-BLEに関する設定)
    - [設定変更が終わったらファームウェアをインストールする](#設定変更が終わったらファームウェアをインストールする)
  - [手順6. 電力モニタリングセンサーのキャリブレーションを行う（オプション）](#手順6-電力モニタリングセンサーのキャリブレーションを行うオプション)
  - [手順7. HomeKitを設定する（HomeKitファームウェアのみ）](#手順7-HomeKitを設定するHomeKitファームウェアのみ)
- [デフォルトの機能について](#デフォルトの機能について)
  - [LEDライトの表示について](#LEDライトの表示について)
    - [白色LEDライト](#白色LEDライト)
    - [青色LEDライト](#青色LEDライト)
  - [本体のボタンについて](#本体のボタンについて)
    - [ボタンを押してすぐ離したとき](#ボタンを押してすぐ離したとき)
    - [ボタンを長押ししたとき](#ボタンを長押ししたとき)
    - [特定のタイミングで複数回押したとき（HomeKitファームウェアのみ）](#特定のタイミングで複数回押したときHomeKitファームウェアのみ)
- [Home Assistantに表示されるエンティティ名について](#Home-Assistantに表示されるエンティティ名について)
- [クレジット](#クレジット)


## 対応機種

### 動作することを確認している機種
- **W2001401**: [**プラグミニ（JP） HomeKit対応**](https://www.switchbot.jp/products/switchbot-plugmini-homekit)


### 動作する可能性が高い機種
- **W1901400**: [**プラグミニ（US）**](https://www.switch-bot.com/products/switchbot-plug-mini)
- **W1901401**: [**プラグミニ（US） HomeKit対応**](https://www.switch-bot.com/products/switchbot-plug-mini-homekit-enabled)
- **W2001400**: [**プラグミニ（JP）**](https://www.switchbot.jp/products/switchbot-plug-mini)

すべてのモデルが全く同じハードウェア構成かどうかがわからないため、これらのモデルで使用するためには設定ファイルを変更しなければならない可能性があります。


> [!IMPORTANT]
> ここで紹介している手順を行うと、HomeKit対応モデルでは**HomeKitの機能は失われます。**
>
> これを補完するために、「[**HAP-ESPHome**](https://github.com/rednblkx/HAP-ESPHome)」を使ってHomeKitの機能を追加したファームウェアも用意していますが、**工場出荷時に設定されていたHomeKitの情報が失われているため、認定されていないハードウェアとして認識されるようになります**のでご注意ください。


## SwitchBot プラグミニをESPHomeで使えるようにする

### 事前準備: ESPHome用のファームウェアをダウンロードする
[**リリースページ**](https://github.com/taikun114/SwitchBot-Plug-Mini-for-ESPHome/releases)からファームウェアをダウンロードすることができます。


#### ファームウェアの種類
機能や言語に応じて、複数のファームウェアを用意しています。

- W2001401用
  - 英語
    - **switchbot-plug-mini-w2001401-vX.X.X.bin**
      - 基本的なファームウェアファイルです。
    - **switchbot-plug-mini-w2001401-homekit-enabled-vX.X.X.bin**
      - 基本的な機能に加え、「[**HAP-ESPHome**](https://github.com/rednblkx/HAP-ESPHome)」を使ってHomeKitの機能を追加したファームウェアファイルです。
  - 日本語
    - **switchbot-plug-mini-w2001401-vX.X.X-ja.bin**
      - 基本的なファームウェアファイルです。
    - **switchbot-plug-mini-w2001401-homekit-enabled-vX.X.X-ja.bin**
      - 基本的な機能に加え、「[**HAP-ESPHome**](https://github.com/rednblkx/HAP-ESPHome)」を使ってHomeKitの機能を追加したファームウェアファイルです。

変更ログは[**こちら**](CHANGELOG-ja.md)をご覧ください。


> [!NOTE]
> [**USモデルのW1901400とW1901401ではソフトウェア面以外での違いはほとんどないとの報告**](https://github.com/kendallgoto/switchbota/issues/19)がありますので、JPモデルのW2001400とW2001401も同様の可能性が高く、W2001400とW2001401は同じファームウェア / 設定ファイルが使用できる可能性があります。


### 手順1. SwitchbOTAを使ってTasmotaファームウェアをインストールする
プラグミニを分解することなく、OTA（Over The Air）でファームウェアを書き換えることができるオープンソースのツール「[**SwitchbOTA**](https://github.com/kendallgoto/switchbota)」を使用して、最初にTasmotaのファームウェアに書き換える必要があります。

ファームウェア書き換えの手順については、[**SwitchbOTAのREADME**](https://github.com/kendallgoto/switchbota#readme-ov-file)をご覧ください。

Tasmotaファームウェアのインストールが完了してWi-Fiの設定が終わったら、次のステップに進みます。

> [!WARNING]
> Tasmotaをv12またはそれ以降のバージョンに**アップグレードしようとしないで**ください。Tasmota v12で導入された新しいパーティションレイアウトではESPHomeと互換性がないため、ESPHomeファームウェアをインストールすることができなくなってしまいます。
>
> また、Tasmota v11またはそれ以前に戻すには、本体を分解してシリアルフラッシュを行う必要がありますので、SwitchbOTAによってインストールされたTasmotaのバージョンは変更しないことを強くおすすめします。


### 手順2. ESPHomeファームウェアをインストールする
![Tasmota Home](images/Tasmota%20Home.png)

TasmotaファームウェアをインストールしたプラグミニのIPアドレスにアクセスすると、Tasmotaのページが表示されるので、「**Firmware Upgrade**」をクリックします。

![Tasmota FW Upgrade](images/Tasmota%20FW%20Upgrade.png)

ファームウェアアップグレードに関する画面が表示されるので、「**Upgrade by file upload**」のところにある「ファイルを選択」をクリックして、ダウンロードしておいたESPHome用のファームウェアを選択し、**ファイル選択の下にある「Start upgrade」をクリック**するとファームウェアアップグレードが始まります。

![Tasmota FW Upgrade Successful](images/Tasmota%20FW%20Upgrade%20Successful.png)

このように、「Upload Successful」と表示されていれば問題なくアップグレードに成功しています。これで、ESPHome用のファームウェアをインストールすることができました。

> [!TIP]
> うまくファームウェアをインストールすることができない場合は、Tasmotaのコンソールで`SetOption78 1`コマンド（OTA互換性チェックを無効化するコマンド）を実行してからファームウェアのアップデートを行う必要があるかもしれません。


### 手順3. Wi-Fiネットワークの設定を行う
ファームウェアのインストールが終わってプラグミニが再起動した後、自動的にWi-Fiネットワークに接続されない場合は、プラグミニの**フォールバックホットスポットに接続してWi-Fiネットワークを設定**するか、**Improv via BLEを使ってWi-Fiネットワークを設定**します。


#### フォールバックホットスポットからWi-Fiネットワークを設定する
プラグミニのフォールバックホットスポットのSSIDは、次のようになっているはずです：`switchbot-plug-mini-xxxxxx`

フォールバックホットスポットに接続すると、自動的に設定画面が開くはずですので、そこからプラグミニを接続したいWi-Fiネットワークを設定します。

> [!TIP]
> 設定画面が自動的に表示されない場合は、ブラウザから手動で [http://192.168.4.1](http://192.168.4.1) にアクセスします。


#### Improv via BLEを使ってWi-Fiネットワークを設定する

[**Improv Wi-Fi**](https://www.improv-wifi.com/)のサイトにアクセスしてBLE経由でWi-Fiネットワークを設定します。

Home Assistantをお使いでBluetooth統合を設定している場合、**自動的にImprov via BLEが検出される**ため、そこからWi-Fiネットワークを設定することもできます。


### 手順4. ESPHomeの設定を行う
ESPHome Device Builderを使って、プラグミニの設定を変更できるように設定を行います。まだESPHome Device Builderがインストールされていない場合は、インストールしておく必要があります。


#### ESPHome Device Builderをインストールする
[**ESPHomeの公式ドキュメント**](https://esphome.io/guides/installing_esphome)にインストール方法が記載されているため、そちらをご覧ください。

ESPHome Device BuilderはHome Assistantのアドオンとしても用意されているため、Home Assistant OSまたはHome Assistant Supervisedをお使いの方は、以下のボタンをクリックしてアドオンをインストールすることもできます。

[![Open your Home Assistant instance and show the dashboard of an add-on.](https://my.home-assistant.io/badges/supervisor_addon.svg)](https://my.home-assistant.io/redirect/supervisor_addon/?addon=5c53de3b_esphome)


#### デバイスの完全な制御を得る
![ESPHome Setup](images/ESPHome%20Setup.png)

ESPHomeのダッシュボードを開くと、先ほどWi-Fiネットワークに接続したプラグミニが検出されるので、「**TAKE CONTROL**」をクリックすると、ESPHomeを使ってプラグミニを**完全に制御することができるように**なります。

![ESPHome Configuration Created Dialog](images/ESPHome%20Configuration%20Created%20Dialog.png)

途中で「Configuration created」と表示されたら、「**SKIP**」をクリックします。

これで、ローカルにコピーされたプラグミニの設定ファイルを編集して、完全に制御できるようになりました。


### 手順5. 最初に変更しておくべき設定
完全に制御できるようになった後、狙い通りに使えるように、設定ファイルを少し変更しておくことをおすすめします。


#### Wi-Fiネットワークに関する設定
初期状態の設定ファイルでは、Wi-Fiネットワークに関する設定は以下のようになっています。
```yaml
# ...

# Wi-Fiネットワークに関する設定
wifi:
  # プラグミニが接続するWi-Fiネットワークの設定
#  ssid: !secret wifi_ssid
#  password: !secret wifi_password

  # プラグミニ起動後、一定の期間（デフォルトでは1分）Wi-Fiネットワークに
  # 接続できなかった場合に有効になるフォールバックホットスポット（アクセスポイントモード）の設定
  ap:
#    password: !secret ap_password # コメントを外して、パスワードを設定することをおすすめします。

# ...
```

Wi-Fiネットワークに関する一部の設定はコメントアウトされているため、**このままインストールするとプラグミニがWi-Fiネットワークに正しく接続できない可能性**があります。そのため、SSIDとパスワード部分のコメントを外しておくことをおすすめします。

また、デフォルトではフォールバックホットスポットのパスワードが設定されていないため、セキュリティ向上のためにパスワードを設定しておくことをおすすめします。

変更後の設定ファイルは以下のようになるはずです。
```yaml
# ...

# Wi-Fiネットワークに関する設定
wifi:
  # プラグミニが接続するWi-Fiネットワークの設定
  ssid: !secret wifi_ssid
  password: !secret wifi_password

  # プラグミニ起動後、一定の期間（デフォルトでは1分）Wi-Fiネットワークに
  # 接続できなかった場合に有効になるフォールバックホットスポット（アクセスポイントモード）の設定
  ap:
    password: !secret ap_password # コメントを外して、パスワードを設定することをおすすめします。

# ...
```

この設定では、Wi-Fiネットワークの設定が、`secrets.yaml`内で指定されたものを使用するようになっています。そのため、このファームウェアをインストールする前に、`secrets.yaml`が正しく設定されていることをご確認ください。

まだ設定されていなければ、`secrets.yaml`を以下のように設定します。
```yaml
# secrets.yaml内

# ESPデバイスが接続するWi-FiのSSIDとパスワード
wifi_ssid: "SSIDをここに入力します"
wifi_password: "ネットワークパスワードをここに入力します"

# ESPデバイスがフォールバックホットスポットモードで動作しているときに接続するためのパスワード
ap_password: "フォールバックホットスポットに接続するためのパスワードをここに入力します"
```


#### Improv via BLEに関する設定
初期状態の設定ファイルでは、Improv via BLEに関する設定は以下のようになっています。
```yaml
# ...

# Bluetooth LEを使ってWi-Fiをセットアップできるようにする設定
# 注意: このコンポーネントを使用していると内部温度が上昇するため、
# このコンポーネントを無効化（コメントアウト）し、フォールバックホットスポットからWi-Fiを設定することをおすすめします。
esp32_improv:
  authorizer: none

# ...
```

設定ファイル内のコメントにも書かれている通り、Improv via BLEコンポーネントを有効化していると**内部温度が上昇する現象を確認**しています。

**デバイス温度の上昇は製品寿命に影響してきます**ので、Improv via BLEを使う予定がない場合は、以下のようにコメントアウトして無効化しておくことをおすすめします（もちろん削除しても問題ありません）。
```yaml
# ...

# Bluetooth LEを使ってWi-Fiをセットアップできるようにする設定
# 注意: このコンポーネントを使用していると内部温度が上昇するため、
# このコンポーネントを無効化（コメントアウト）し、フォールバックホットスポットからWi-Fiを設定することをおすすめします。
#esp32_improv:
#  authorizer: none

# ...
```


#### 設定変更が終わったらファームウェアをインストールする
これらの設定変更が終わったら、ESPHomeのダッシュボードから目的のデバイスの右下にある「...」をクリックしてメニューを開いたら、「**Install**」をクリックします。

> [!TIP]
> 初めてESPHomeを使用する場合、編集した設定ファイルが正しいか不安になるかもしれません。
>
> その場合は、メニュー内の「**Validate**」をクリックすると、**設定ファイルが正しい状態になっていることをチェック**してくれます。
>
> **「INFO Configuration is valid!」と表示されれば設定ファイルは問題ありません**ので、インストールに進んで問題ありません。

その後、インストール方法を聞かれるので「**Wirelessly**」をクリックすると、設定ファイルをもとにファームウェアのコンパイルが行われ、プラグミニにファームウェアがインストールされます。

問題なくインストールすることができたら、**ESPHomeでプラグミニを完全に制御することができるようになりました！**

このままHome Assistantに追加して使ったりしても良いですし、設定ファイルを更新して機能を追加したりしても問題ありません。SwitchBotのクラウドからは完全に切り離されており、**プラグミニは真の意味で「あなたのもの」になったのです！**


### 手順6. 電力モニタリングセンサーのキャリブレーションを行う（オプション）

ESPHomeで使えるようになったプラグミニはそのまま使うこともできますが、工場出荷時に行われたキャリブレーションデータは消えてしまっているので、正しい値を取得できるようにするためには**センサーのキャリブレーションを行う必要**があります。

電力モニタリングセンサーのキャリブレーションを行う方法については、[**ESPHomeの公式ドキュメント**](https://esphome.io/components/sensor/hlw8012#calibration)をご覧ください。

キャリブレーションデータは、設定ファイル内の上部に、以下のように記載されています（初期状態では、私のプラグミニのキャリブレーションデータが設定されています）。
```yaml
# ...

  # キャリブレーションデータ
  voltage_divider: "660.7973855893309"
  current_resistor: "0.0024013086148925913"
  current_multiply: "1.783420806116323"
  # キャリブレーションを行う際、電力メーターに表示される値からプラグミニ自体の消費電力（リレーをオンにした状態で約0.8W / 0.019A）を
  # 引いた値を使ってキャリブレーションを行ってください。
  #
  # キャリブレーションの方法については、以下の記事をご覧ください。
  # https://esphome.io/components/sensor/hlw8012#calibration

# ...
```

キャリブレーションされた電力メーターの測定値に合わせるためのキャリブレーションデータ計算機が[**ESPHomeの公式ドキュメント**](https://esphome.io/components/sensor/hlw8012#calibration)にありますので、そちらをご利用ください。


### 手順7. HomeKitを設定する（HomeKitファームウェアのみ）

HomeKit機能付きのファームウェアをインストールした場合、プラグミニを**Appleの「ホーム」に追加して制御する**ことができます。

追加で必要な設定は特になく、「ホーム」アプリのアクセサリ追加画面を開くとプラグミニ（`プラグミニのブリッジ`または`Plug Mini Bridge`）が表示されるはずです。セットアップコードは[**HAP-ESPHome**](https://github.com/rednblkx/HAP-ESPHome)のデフォルトコードである`159-35-728`を使用します（設定ファイルから変更できます）。

スイッチの名前はデフォルトで`プラグ`または`Plug`になっていますが、「ホーム」アプリからお好みの名前に変更してください。

> [!TIP]
> 名前を変更した後にファームウェアを更新しても、「ホーム」アプリに表示される名前は変わりませんのでご安心ください。

これらの設定が終わったら、必要なセットアップは全て完了です。お疲れ様でした！


## デフォルトの機能について
### LEDライトの表示について
プラグミニには、白色と青色の2つのLEDライトが搭載されています。

デフォルトのLEDライトの動作は、以下の通りです。


#### 白色LEDライト
リレースイッチの状態を表しています。

LED消灯: リレースイッチが**オフ**

LED点灯: リレースイッチが**オン**

> [!NOTE]
> `LED Brightness`が`オフ`または`Off`に設定されている場合、リレースイッチの状態にかかわらずLEDは消灯状態となります。


#### 青色LEDライト
Wi-Fiネットワークの接続状況を表しています。

LED消灯: Wi-Fiネットワークに**接続されている**

LED点滅: Wi-Fiネットワークから**切断されている**

> [!NOTE]
> 青色LEDライトは、Wi-Fiネットワークの状態を常に確認できるように、`LED Brightness`設定の影響を受けません。
>
> つまり、`LED Brightness`が`明るい`または`Bright`以外に設定されていたとしても、Wi-Fiネットワークから切断されている間は100%の明るさで点滅します。


### 本体のボタンについて
プラグミニには、ボタンが1つ搭載されています。

デフォルトのボタンの動作は以下の通りです。


#### ボタンを押してすぐ離したとき
**50ミリ秒（0.05秒）以上500ミリ秒（0.5秒）以下の長さ**の間ボタンを押すと、**リレースイッチを切り替え**ます。


#### ボタンを長押ししたとき
**3秒以上10秒以下の長さ**の間ボタンを長押しすると、**プラグミニを再起動**します。


#### 特定のタイミングで複数回押したとき（HomeKitファームウェアのみ）
以下の通りにボタンを操作すると、HomeKitのペアリングをリセットします。

1. 1秒以上ボタンを長押しする
2. 2秒以上ボタンを離したままにする
3. 上記手順をあと2回繰り返す


## Home Assistantに表示されるエンティティ名について
デフォルトでは、**インストールしたファームウェアの言語にかかわらず、Home Assistantに表示されるエンティティ名は英語**になっています。

これは、正しいエンティティIDが割り振られるように意図しているためです。

Home AssistantのデフォルトエンティティIDは、ESPHomeの`friendly_name`および`name`プロパティからエンティティIDを決定しますが、**アルファベットと数字以外の文字が含まれていると、その部分は空として認識される**（例: `Wi-Fiシグナル` → `wi_fi`のように、日本語部分だけがなくなる）ため、目的のエンティティを探しづらくなってしまいます。

後からHome Assistant側でエンティティIDを変更することもできますが、すべてのエンティティIDを変更するのは非常に手間ですので、最初から英語のエンティティ名になるように設定しています。

そのため、日本語のエンティティ名が良いのであれば、**Home Assistantから手動でエンティティの名前を変更してください。**

名前の日本語訳は以下の通りです（Home Assistantのデバイス設定画面に表示される順番に並んでいます）。

| 英語名                | 日本語名            |
|----------------------|--------------------|
| Plug                 | プラグ              |
| Apparent Power       | 皮相電力            |
| Current              | 電流               |
| Power                | 電力               |
| Power Factor         | 力率               |
| Total Daily Energy   | 一日の累計消費電力量  |
| Total Energy         | 累計消費電力量       |
| Total Monthly Energy | 一ヵ月の累計消費電力量 |
| Voltage              | 電圧               |
| Blue LED             | 青色LED            |
| LED Brightness       | LEDの明るさ         |
| Reboot               | 再起動              |
| White LED            | 白色LED            |
| Internal Temperature | 内部温度            |
| Uptime               | アップタイム         |
| Wi-Fi Signal         | Wi-Fiシグナル       |

もちろん、これらの翻訳はあくまで一例ですので、お好きなようにカスタマイズしていただいて構いません。


## クレジット
- [**SwitchbOTA**](https://github.com/kendallgoto/switchbota)
  - OTAでプラグミニのファームウェア書き換え
- [**ESPHome**](https://github.com/esphome/esphome)
  - [ESPHomeプラットフォーム・ドキュメント](https://esphome.io/)
- [**HAP-ESPHome**](https://github.com/rednblkx/HAP-ESPHome)
  - ESPHome上でのHomeKit
- [**@halomakes**](https://gist.github.com/halomakes)
  - プラグミニ用設定ファイル作成の元となった、プラグミニ用のESPHome設定ファイル: [switchbot-plug-mini-esphome.yml](https://gist.github.com/halomakes/8be3976a034ad32e37e9c3b315d25b64)
- [**Home Assistant**](https://github.com/home-assistant/)
  - [Home Assistantプラットフォーム・ドキュメント](https://www.home-assistant.io/)
