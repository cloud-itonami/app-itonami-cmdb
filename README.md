# app-itonami-cmdb

**cmdb（資産台帳）の itonami dapp。** 端末・サーバ・クラウド・SaaS・データの
資産と依存関係を 1 つの台帳にする。host の収集出力をレコード・優先度・
gaps・共有 remote に変え、前後2回の収集の差分（software 追加/削除/変更、
listener 追加/削除）を出す。

slice 1 は判断コアのみ: 優先度スコアとバージョン差分の純粋計算を guest
（aiueos）と adapter（src）で parity 付きで持つ。公開・画面・永続化は後の
slice。追跡は Linear JUN-1049（親）/ JUN-1066（この slice）。dapp の形は
JUN-116（1 repo = 1 スライス）、前例は app-itonami-mihari（JUN-286）に従う。

## 何があって、何が動くか

| 層 | ファイル | 状態 |
|---|---|---|
| 判断コア guest | `aiueos/cmdb_core.cljk`（ns `cmdb.guest`、`:effects #{}`） | 実装済み。amu test は実機待ち（`amu test` が要る） |
| アプリ記述子 | `aiueos/cmdb.edn`（`:aiueos/kind :app`） | 記述のみ |
| adapter | `src/cmdb/core.cljk` — guest と同名・同結果の関数＋文字列→コードの橋渡し | 実装済み・実測（kbb） |
| parity 表 | `test/fixtures/parity.edn`（19 行） | guest の test-* と adapter の両方が通す |
| 取り込み・書き出し・画面・公開 | — | 未着手（後の slice） |

## 開発

```bash
npm test   # kbb で parity 表を adapter 照合
```

guest の test: `amu test aiueos/cmdb_core.cljk`（amu 実機で）。
