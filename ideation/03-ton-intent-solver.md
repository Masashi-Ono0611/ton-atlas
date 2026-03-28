# アイデア3: IntentLayer TON — TON上のIntent-basedDEX + Solverネットワーク

> 作成日: 2026-03-28
> 優先度: ★★
> カテゴリ: DeFi / Protocol

---

## 問題

### TON DEXの現状

```
現在のTON DEX:
- DeDust: AMM（Automated Market Maker）
- STONfi: AMM
- Bidask: CLMM（Concentrated Liquidity）

全て「ユーザーが直接プロトコルに操作する」モデル:
1. スリッページをユーザーが手動設定
2. どのプールを使うかをユーザーが選ぶ
3. MEV攻撃（サンドイッチ）に無防備
4. クロスプロトコル最適化なし
```

### ユーザーの問題

```
「TONを200 USDTに換えたい」

現状: DeDust or STONfi どちらを使うか選ぶ
    → どちらが安いか調べる → スリッページ設定 → 承認 → 実行
    → MEVボットに先回りされる可能性

理想: 「200 USDT欲しい」と宣言するだけ
    → ソルバーが最良経路を競争 → 最良価格で自動実行
    → MEV保護付き
```

---

## ソリューション: IntentLayer TON

### コアコンセプト

CoW Swap（ETH）の設計をTONに移植 + TON-native改良

```
IntentLayer TON の動作:

1. ユーザーが「Intent（意図）」に署名
   {from: TON, to: USDT, amount: 100, minReturn: 195, expiry: 5min}

2. ソルバーネットワーク（オフチェーン）がIntentを受け取る

3. ソルバーが競争入札（オフチェーン計算）
   - DeDust最適ルート
   - STONfi最適ルート
   - 複数ホップ経路（TON→USDT→他Jetton→...）
   - Coincidence of Wants（売り手と買い手のマッチング）

4. 最良ソルバーが選ばれてオンチェーン実行

5. ユーザーは最良レートを受け取る（+MEV保護）
```

### Coincidence of Wants (CoW) の仕組み

```
例:
- Alice: TON 100 → USDT 欲しい
- Bob: USDT 300 → TON 欲しい

CoWマッチング:
- Aliceの100 TONとBobの300 USDTをそのまま交換
- 中間の流動性プールを経由しない
- 両者とも手数料ゼロ、価格改善
```

TONのactor modelでは、複数のメッセージを非同期に処理するため、バッチ実行の設計が重要。

---

## アーキテクチャ

```
IntentLayer TON

┌──────────────────────────────────────────────────────┐
│ オフチェーン（ソルバーネットワーク）                   │
│                                                        │
│ Intent Aggregator                                      │
│ ├── ユーザーのIntent受け取り（署名済み）               │
│ ├── バッチ形成（15秒ごと）                            │
│ └── ソルバーへの配布                                  │
│                                                        │
│ Solver Network（競争参加者）                           │
│ ├── Solver A: DeDust最適化                           │
│ ├── Solver B: STONfi最適化                           │
│ ├── Solver C: クロスプロトコル最適化                  │
│ └── Solver D: CoWマッチング専門                       │
│                                                        │
│ Settlement Selection                                   │
│ └── 最良ソルバーを選択（Solver Auction）              │
└────────────────────────┬─────────────────────────────┘
                          ↓ 選ばれたソルバーが
┌────────────────────────▼─────────────────────────────┐
│ オンチェーン（TON）                                    │
│                                                        │
│ IntentLayer Settlement Contract                        │
│ ├── Intent署名の検証                                  │
│ ├── ソルバーの実行トランザクション検証                 │
│ ├── スリッページ・最低リターン保証                     │
│ └── 実行失敗時の自動refund                            │
└──────────────────────────────────────────────────────┘
```

---

## 差別化

| 観点 | IntentLayer TON | DeDust | STONfi |
|---|---|---|---|
| ユーザーUX | Intent宣言のみ | AMM直接操作 | AMM直接操作 |
| 価格最適化 | 複数プロトコル横断最適化 | 単一プール | 単一プール |
| MEV保護 | バッチ実行で原理的に保護 | なし | なし |
| CoW | あり（手数料なしマッチング） | なし | なし |
| スリッページ設定 | 不要（最低リターンを指定） | 手動 | 手動 |

---

## MEV研究資産との接続

**MEVサーチャーの知識 = Solverの知識**

```
MEVサーチャーが解く問題:
→ 「このtxを見て、どうすれば最大利益を得られるか」
→ 最適経路探索、グラフアルゴリズム、ガス最適化

Intentソルバーが解く問題:
→ 「このIntentを見て、どうすれば最良レートを出せるか」
→ 同じく最適経路探索、グラフアルゴリズム、ガス最適化
```

**Flashbots sponsored txの知識でSolverを実装できる。**

---

## 収益モデル

| 収益源 | 詳細 |
|---|---|
| プロトコル手数料 | 執行済みボリュームの0.05〜0.1% |
| Solver登録料 | ソルバーがネットワークに参加するためのステーク（悪意ある行動の防止） |
| Intent Aggregator料 | Solverへの注文フロー配布手数料 |

---

## 実装順序

### Phase 1: Single Solver（シンプル版）

まず1つのソルバー（自分自身）でMVPを実装:
1. Intent署名スキーマの設計（TONスマートコントラクト上）
2. オフチェーンルーティング（DeDust + STONfi）
3. Settlement Contractでの検証・実行

### Phase 2: Solver Network

MVPで取引ボリュームが確認できたらソルバーを開放:
1. Solver APIの公開
2. Solver Auctionメカニズムの実装
3. コミュニティへのSolver参加招待

### Phase 3: CoW + Full Intent

1. Coincidence of Wantsマッチング
2. クロスチェーンIntent（TON ↔ ETH/Solana）
3. AIエージェントとの統合（AgentPay + IntentLayer）

---

## リスク

| リスク | 対策 |
|---|---|
| TONのactor model非同期性でのバッチ実行困難 | 単一ブロック内バッチは難しいためoffchain aggregationで代替 |
| Solver参加者が少ない初期 | 自社Solverが初期流動性を担保 |
| DeDust/STONfi等の既存プロトコルに対する依存 | 将来的に直接流動性プールを持つオプション |

---

*関連: [defi-trends.md](../research/defi-trends.md) — Intent-basedトレンド（CoW Swap等）*
*関連: [mev-ton-opportunity.md](../research/mev-ton-opportunity.md) — Solver = MEVサーチャーの転用*
*関連: [01-gasless-agent-payment.md](01-gasless-agent-payment.md) — AgentPayとの統合*
