# DeFi Q1 2026 トレンド調査

> 調査日: 2026-03-28
> 対象: 全チェーンDeFiトレンド + TONへの示唆

---

## 1. マクロ指標

| 指標 | 数値 | 前年比 |
|---|---|---|
| DeFi TVL（全チェーン合計） | $130〜140B | ＋ |
| 全暗号資産市場規模 | $238.5B | — |
| 機関投資家トレジャリー保有（利回り付きSB） | $20B+ | $9.5B → $20B |
| RWA on-chain | $30B+ | — |
| RWA 2030予測（ARK Invest） | $9.43T | — |
| 新規DeFiプロトコルのAIエージェント組み込み率 | 68% | ＋ |

---

## 2. 5大DeFiトレンド

### 🎯 トレンド1: Intent-based / Solver-based 取引

**概要**

ユーザーが「何をしたいか（intent）」を宣言し、ソルバーが最適な実行経路を競争入札するモデルが標準化しつつある。

従来のDeFiはユーザーが「どのプロトコル・どのルート」を指定する必要があった（AMM→プールを選ぶ等）。
Intentモデルでは「USDTをBTCに変えたい」と宣言するだけで良い。

**主要プレイヤー**

| プレイヤー | 仕組み | 規模 |
|---|---|---|
| **CoW Swap** | Batch auction + Coincidence of Wants | 月次 $10B+ ボリューム |
| **UniswapX** | Uniswap v4と統合、クロスチェーンintent | Uniswap TVL連動 |
| **NEAR Intents** | ソルバーがNEARで即時決済 | 200,000倍成長（2025年中） |
| **Anoma** | Intent-centric L1、multi-agent counterparty discovery | プロダクション前 |

**CoW Swap の仕組み（参考）**

```
バッチオークション（15秒ごと）:
1. ユーザーが「intent署名（スリッページなし、有効期限付き）」を提出
2. ソルバーが最良実行経路を競争入札（オフチェーン計算）
3. 最良ソルバーが選ばれてバッチをオンチェーン決済
4. CoW（売り手と買い手が一致）があれば中間手数料ゼロ
→ MEV保護: バッチ内のintentは同時に実行されるためサンドイッチ攻撃不可
```

**TONへの示唆**

- TON DEXは現在AMM中心（DeDust / STONfi）
- ソルバー競争市場が存在しない → Intent Layer実装の大きな機会
- MEV保護という副次的価値も提供できる

---

### 💰 トレンド2: Yield-bearing Stablecoin の制度化

**概要**

利回り付きステーブルコインが機関投資家の短期資金運用として採用急拡大。
「ステーブルコインを持っているだけで稼げる」が標準になりつつある。

**市場データ**

| 指標 | 数値 |
|---|---|
| 機関トレジャリー保有（利回り付きSB） | $9.5B → $20B（2025〜2026） |
| DeFiプロトコル平均利回り | 5〜12% |
| 米国債連動型（安全側） | 約5% |

**主要プロダクト比較**

| プロダクト | 発行体 | 利回り | 仕組み | KYC |
|---|---|---|---|---|
| **USDe** | Ethena | 7〜20% | デルタニュートラル（ETH long + 先物short） | 不要 |
| **sUSDe** | Ethena | 約10% | staked版USDe、自動再投資 | 不要 |
| **USDY** | Ondo Finance | 5.2% | 米国債バック | ✅ 必須 |
| **OUSD** | Origin Protocol | 変動 | AMM利回り + DeFi利回り | 不要 |
| **mEVUSD** | Pendle × Maple | 7〜12% | EU機関向けプライベートクレジット | ✅ 必須 |

**規制環境**

- GENIUS Act（米国）: ステーブルコイン規制明確化 → 機関の参入障壁低下
- MiCA（EU）: 2025年完全施行 → EU機関のステーブルコイン利用推進

**TONへの示唆**

- TONでのUSDe 20% APY開始（X公式発信、2026年）
- Tetherが主力だが利回りゼロ → yield-bearing SBの差別化が大きい
- TON DeFiでの利回りステーブルコイン統合はまだ黎明期

---

### 🏭 トレンド3: RWA（現実資産トークン化）の本格化

**概要**

米国債・プライベートクレジット・株式・不動産がオンチェーン化。
DeFiの基盤担保資産になりつつある。

**市場規模**

| 分野 | 規模 | 主要プレイヤー |
|---|---|---|
| 米国債トークン | $10B+ | Ondo, Franklin Templeton, BlackRock BUIDL |
| プライベートクレジット | $181M → $1.5B残高 | Maple Finance |
| 株式トークン | 250+ 銘柄 | Felix Protocol（HyperEVM） |
| 不動産/ローン | — | Centrifuge, Goldfinch |

**Maple Finance の事例**

```
私的信用のオンチェーン化:
- 借り手: マーケットメーカー・トレーディングファームに貸出
- 貸し手: DeFiユーザーが暗号資産担保でローンプールに入金
- mEVUSD: EU機関向けに7-12% APYを提供するPendle統合版
- 2025年実績: $181M → $1.5B 残高（8倍超）
```

**Pendle Finance の役割**

Pendleは「将来の利回り」と「元本」を分離するDeFiプロトコル。

```
例: USDe（年10%利回り）
→ Pendle分割: PT-USDe（確定利回り部分）+ YT-USDe（変動利回り部分）
→ PT-USDe: 固定利率で買える → 機関の短期運用に最適
→ YT-USDe: 利回りの変動をトレードできる → リスクテイカー向け
```

**TONへの示唆**

- TON上のRWAはほぼ空白地帯
- Telegramユーザー = 新興国比率高い → 低コスト信用アクセス（マイクロローン）への需要
- Giftsを「RWA的担保」として扱う設計（GiftFiの発想）が革新的

---

### 📊 トレンド4: フルオンチェーンCLOBの成立（Hyperliquid）

**概要**

AMM中心だったDeFi取引所市場で、中央集権型取引所（CEX）と同等のパフォーマンスを持つオンチェーンオーダーブックが実証された。

**Hyperliquid の実績**

| 指標 | 数値 |
|---|---|
| スポットオーダーブック成長 | $12B → $125B（2025年） |
| 独自L1 | HyperEVM（EVM互換、高スループット） |
| マッチング方式 | FIFO（タイムプライオリティ）— 完全オンチェーン |
| MEV保護 | FIFO一致でfront-runningを構造的に排除 |

**なぜAMMではなくCLOBか**

```
AMM（DeDust/STONfi/Uniswap等）の弱点:
- スリッページが大きな取引で問題
- MEVのサンドイッチ攻撃に弱い
- 価格発見がオフチェーンマーケットに依存

CLOB（オーダーブック）の強点:
- CEXと同等の価格発見
- 指値注文でスリッページゼロ
- FIFO一致でMEV保護
- 大口取引に適している
```

**TONへの示唆**

- TON DEXはDeDust/STONfiのAMM中心 → CLOBは未参入
- TONのactor model（非同期メッセージパッシング）でのCLOBアーキテクチャ設計は技術的チャレンジ
- 解決できれば差別化の大きいプロダクト（詳細: [ideation/03-ton-intent-solver.md](../ideation/03-ton-intent-solver.md)）

---

### 🤖 トレンド5: AI × DeFi の融合

**概要**

AIエージェントがDeFiの「実行レイヤー」となる動きが加速。

**主要な動き**

| プレイヤー | 動き | 規模 |
|---|---|---|
| Virtuals Protocol / G.A.M.E. | AIエージェントの経済基盤。エージェントがDeFi操作を自律実行 | aGDP $477M |
| elizaOS DeFi plugins | Uniswap・Aave・Curve等への自律操作プラグイン | 200+プラグイン |
| Coinbase Agentic Wallets | プログラマブル支出制限付きエージェントウォレット | 5,000万tx |
| PanoramaBlock (TON) | NLでスワップ・イールド・ステーキングを横断（maルチチェーン） | 93 claps |

**Intent → Agent パイプライン**

```
ユーザー: 「毎週月曜日に給与の10%をETHに変えて、5%をUSDe利回りに入れておいて」

AIエージェント（elizaOS / TON Agent Platform等）:
1. インテントを解析
2. 最適なタイミング・ルートを判断
3. DeFiプロトコルを自律実行（USDe入金、ETH購入）
4. 利回りを毎日複利で再投資
5. 異常があれば通知

→ ユーザーは設定だけ、あとは全自動
```

**TONへの示唆**

- TON AgenticKit × TON Pay × DeFiプロトコルの統合が最大機会
- Telegramのメッセージングから自然言語でDeFi操作できるUXは他チェーンにない
- PanoramaBlockがプロトタイプだが、TON-native深度が必要

---

## 3. TON DeFi 現状と特性

### TON DeFi エコシステム

| プロトコル | 種類 | 特徴 |
|---|---|---|
| DeDust | AMM DEX | 主要DEX、複数プール |
| STONfi (STON.fi) | AMM DEX | Jetton対応、UX重視 |
| Evaa Protocol | Lending | TON初のAave相当 |
| Bemo | Liquid Staking | TON staking + LST |
| DAOLama | Yield Aggregator | 複数プロトコル最適化 |

### TON DeFiの構造的制約

1. **TVL規模**: $0.5B程度（ETH $56B、Solana $11.5Bと桁違い）
2. **流動性の深度**: 大口取引でスリッページが問題になる水準
3. **Jetton標準の分散**: ERC-20と異なり複数実装が混在
4. **ブリッジ限界**: 他チェーンからの流入に摩擦

### TON DeFiの独自優位

1. **Telegram Mini App内での決済** — Web2ユーザーが「知らないうちにDeFiを使う」UX
2. **Telegram Gifts / Stars** — 他チェーンに存在しない独自資産
3. **新興国ユーザー基盤** — USドルへのアクセスを求める層
4. **Gas Bank** — 開発者がガス代を肩代わり → 完全なガスレスUX

---

## 4. 規制環境（2026 Q1）

| 規制 | 内容 | DeFiへの影響 |
|---|---|---|
| GENIUS Act（米国） | ステーブルコイン発行者への要件明確化 | 機関参入を後押し、USDTリスク低下 |
| MiCA（EU） | 2025年完全施行。暗号資産サービス規制 | EU機関がRWA・yield SBに参入しやすく |
| SEC SOL分類 | Solanaを「デジタルコモディティ」に | Solana機関参入加速。ETH対比優位 |
| FIT21法案 | CFTC vs SEC 管轄明確化 | DeFiプロトコルのコンプライアンスが計画可能に |

**TONへの影響:**
- TONはSECから直接照準されていない（新興国フォーカスが功を奏している）
- Telegram/DurovのEU圧力は継続中
- 新興国でのステーブルコイン決済は規制グレーゾーンが多い → 慎重な設計が必要

---

*関連: [mev-landscape.md](mev-landscape.md) — MEV動向（DeFiと深く連動）*
*関連: [mev-ton-opportunity.md](mev-ton-opportunity.md) — TONでの機会*
*関連: [ton-vs-ecosystem.md](ton-vs-ecosystem.md)*
*作成: 2026-03-28*
