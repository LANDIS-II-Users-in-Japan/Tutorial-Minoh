# Tutorial-Minoh

国内のオープンなデータだけでLANDIS-IIの準備をして計算を回す体験をするためのチュートリアルです。

## 0. Succession Extensionを決める
- Succession Extension: 各グリッド内の植物の成長・枯死・更新のプロセスを計算
- どのような生態学的プロセスを計算するかによって、必要な入力データや計算の負荷が変わる
- 研究目的に応じて選択しましょう

検討ポイント (例)
- [ ] モデルで再現したい生態学的プロセスは何か？
- [ ] どの程度気候変動の影響を考慮したいか？
- [ ] 地上部バイオマスのみに興味があるか？NEE (Net Ecosystem Exchange) に興味があるか？





## 1. 対象地域を決める

### 計算対象地域を決める
- 都府県の大きさであれば計算可能
- 今回は箕面国定公園の国有林を考える
- 民有林のデータは各都道府県の方針で公開されていたりいなかったりする。





### 計算の空間解像度を決める
検討ポイント
- [ ] モデルで再現したい生態学的プロセス・自然撹乱・管理の空間解像度
- [ ] 入手可能なデータの空間解像度
- [ ] 計算の負荷








## 2. 入力データを作成する

### Initial Community Mapを作成する
#### 要求仕様
- 各グリッドに以下の情報を格納すること
    - 樹種名
    - 樹齢
    - バイオマス
- 以下の制約を満たすこと
    - 樹齢は、各樹種のLongevityを超えないこと

#### 情報源の例

| 情報源 | Pros | Cons |
| --- | --- | --- |
| 森林簿 / 森林GIS | 樹種名、樹齢、バイオスが得られる | 広葉樹の情報が限定的 / 林床の情報がない / 情報が必ずしも正確ではない |
| 植生図 | 広葉樹の情報も得られる | 樹齢、バイオスの情報がない / 図郭の境界で非連続になることがある |
| 毎木調査 | 樹種名、蓄積量が得られる | 広域をカバーできない / 樹齢は樹齢-樹高の関係から推定しないといけない |

#### 本チュートリアルでの情報源
- 国土数値情報: 国有林野データ, https://nlftp.mlit.go.jp/ksj/gml/datalist/KsjTmplt-A45.html
    - 保存場所: ./data/forest




### Ecoregion Mapを作成する
#### 要求仕様
- 計算対象のグリッド (Active sites) が指定されている
- 各グリッドの環境情報に基づきActive sitesを類型化し、各グリッドの類型別のID番号が格納されている

#### 情報源の例

| 環境情報 | 情報源 | Pros | Cons |
| --- | --- | --- | --- |
| 気候 (過去〜現在) | [国土数値情報 平年値メッシュ](https://nlftp.mlit.go.jp/ksj/gml/datalist/KsjTmplt-G02-2022.html) | 1km解像度で容易に平年値を取得できる | 平均のみで分散の情報がない == 年変動がない |
| | [農研機構 メッシュ農業気象データ](https://amu.rd.naro.go.jp/wiki_open/doku.php?id=about) | 1km解像度でのこれまでの気象データを取得できる | ない・・・？ |
| 気候 (将来) | [NIES2020](https://www.nies.go.jp/doi/10.17595/20210501.001.html) | 1km解像度のCMIP6のhistorical(1900-2014) & SSP-RCPデータ (2015-2100) |  |
| 土壌 | [国土数値情報 土壌図](https://nlftp.mlit.go.jp/ksj/gml/datalist/KsjTmplt-G02-2022.html) | 1km解像度で容易に土壌情報を取得できる | 1km解像度では詳細な土壌情報は得られない |

Ecoregionを作成する際に検討すべき項目はSuccession Extensionによって異なるため、まずはSuccession Extensionを決めましょう。

#### 本チュートリアルでの情報源
- 国土数値情報: 平年値メッシュ, https://nlftp.mlit.go.jp/ksj/gml/datalist/KsjTmplt-G02-2022.html
    - 保存場所: ./data/climate





### iniファイルを設定する (Biomass successionとNECNを例に)

- [ ] 2. で作成したラスタデータ・パラメータ等を./scenario内のチュートリアルに配置する
    - 設定ファイルの雛形は、各extensionのGitHubリポジトリ内にあるのでそれを参照して使う
        - Biomass-succession: https://github.com/LANDIS-II-Foundation/Extension-Biomass-Succession
        - NECN-succession: https://github.com/LANDIS-II-Foundation/Extension-NECN-Succession/tree/master
        - PnET-succession: https://github.com/LANDIS-II-Foundation/Extension-PnET-Succession

## 3. Biomass-succession / NECN-succession / PnET-successionを実行する

- [ ] Biomass-successionから試してみる。NECNやPnETは時間があれば。

## 4. Calibration & Validation関係
- [ ] キャリブレーション・バリデーションで当てるべき現象をクリアにすること！！
- See, https://cdnsciencepub.com/doi/full/10.1139/cjfr-2024-0085


## 5. 自然撹乱・森林管理を設定する
- 森林管理のチュートリアルができれば・・・

