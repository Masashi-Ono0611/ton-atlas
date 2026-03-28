# MEV 2026 グローバルランドスケープ

> 調査日: 2026-03-28
> 対象: Ethereum / Solana (Jito) / クロスチェーン MEV動向

---

## 1. MEV市場規模

| 指標 | 数値 |
|---|---|
| 年間MEV抽出額（全チェーン） | $3B+ |
| 成長率 | 2年で2倍 |
| Ethereum arbitrage の私的チャネル比率 | 90%+（MEV-Boost経由） |
| Baseのスパムオークション非効率 | 650倍のガス無駄（Flashbots研究） |
| Jito（Solana）のバリデータ採用率 | 80%+ |

---

## 2. MEVの基本構造（Ethereum）

### 現在のMEV-Boost アーキテクチャ

```
Validator (Proposer)
    ↓ getHeader (最高入札のBuilder blockに署名)
MEV-Boost Relay (中継者)
    ↑ submitBlock (Builder が構築したブロックを提出)
Block Builder (ブロック構築者)
    ↑ bundle (優先度付きトランザクション群)
Searcher (MEVサーチャー)
    ↑ frontrun / arbitrage / liquidation
Mempool / Private OFA
```

### Block Builder の現実

| Builder | Ethereum block share | 特徴 |
|---|---|---|
| **beaverbuild** | 50%+ | 最大手、強力なサーチャー連携 |
| **Titan** | 40%+ | 2番手、beaverbuildと合計90%超 |
| その他 | 10%未満 | 小規模builderは生存困難 |

**2社で90%超の集中は、Ethereumの検閲耐性・中立性への脅威。**

---

## 3. 5大MEVトレンド（2026年）

### 🔥 トレンド1: AI-on-AI 軍拡競争

**概要**

MEV botの競争が機械学習・強化学習ベースの予測型へ進化。「AIに勝つにはAIが必要」な状態に。

**具体的な変化**

- **RL (強化学習) ベースbot**: 過去の入札履歴から最適入札額・タイミングを学習
- **予測front-running**: メンプールのtxパターンを分析し、大口スワップの前後に先回り
- **大型ファンドがMLエンジニアを大量採用**: Jump, Wintermute等がAIチームを強化
- **アルゴの秘匿化**: 競合のリバースエンジニアリングを防ぐため難読化・暗号化が進む

**参入障壁の変化**

```
2020年: 単純なarb bot → 誰でも参入可能
2022年: flashloans + multi-hop arb → プログラマー必要
2024年: AI予測front-running → MLエンジニア必要
2026年: AI-on-AI + 大規模データ → 数百万ドルの研究開発費が必要
```

**結論: 小規模サーチャーがEthereum arbitrageで稼ぐのはほぼ不可能に。**

---

### 🔀 トレンド2: クロスチェーンMEV

**概要**

単一チェーンでの価格差アービトラージから、複数ネットワーク横断スキャンへ。

**主要戦略**

| 戦略 | 仕組み |
|---|---|
| Cross-L2 arbitrage | Ethereum L2間（Base, Arbitrum, Optimism）の価格差を同時監視・実行 |
| Bridge timing MEV | クロスチェーンブリッジの遅延（T+7min等）を利用したポジション取り |
| ETH-Solana spread | 両チェーンのUniswap↔Jupiterの価格差（確実性は低い） |
| CEX-DEX latency | CEXの注文約定 → DEXへの反映ラグを利用 |

**技術的課題**

- **原子性の欠如**: クロスチェーンでtxを同時原子実行できない
- **失敗リスク**: 片側が失敗したとき相手側の損失が確定する
- **遅延の不確実性**: ブリッジ遅延が可変

**結論: 大手サーチャーのみが実行可能。インフラ投資が数百万ドル規模。**

---

### ⚖️ トレンド3: OFA (Order Flow Auction) の寡占化

**概要**

注文フロー（ユーザーのtx）を売買するマーケットが形成されたが、寡占化が深刻。

**OFAの仕組み**

```
ユーザーがtxを送信
    ↓
Wallet (MetaMask, Coinbase Wallet等)がOFAプロバイダーにルーティング
    ↓
OFAプロバイダー（e.g. UniswapX, CoW Swap, MEV Blocker）
    ↓
Builderへのオークション → 最高入札者がブロックに含める
    ↓
ユーザーへのリベート（MEV利益の一部が返ってくる）
```

**問題点**

1. **Builder寡占がOFAにも波及**: beaverbuild + Titanへの注文集中 = 中立性消失
2. **Searcher-builder垂直統合**: 大手builderが自社searcherを持ち内部優先
3. **ユーザーへの還元が不透明**: リベートがどこまで通るか不明確

---

### 🔧 トレンド4: ePBS（Enshrined Proposer-Builder Separation）— Glamsterdam May 2026

**概要**

EthereumのGlamstertadmアップグレードで、プロポーザー-ビルダー分離がプロトコルに組み込まれる。

**Before (外部PBS):**
```
Validator → MEV-Boost (外部middleware) → Relay → Builder
問題: Relayへの信頼が必要、検閲可能、寡占温床
```

**After (ePBS):**
```
Validator (Proposer) → [プロトコルネイティブ競売] → Builder
改善:
- Relay不要 (trustless)
- Inclusion Listで検閲耐性強化
- 小規模builderの参入が容易
- MEV再分配の透明化
```

**ePBS後のシナリオ**

- beaverbuild / Titan の90%支配が崩れる可能性
- 新興builderが参入しやすくなる
- MEV全体額は変わらないが分配が民主化

---

### 🛡️ トレンド5: SUAVE — クロスドメインプログラマブルMEV

**概要**

FlashbotsのSUAVE（Single Unified Auction for Value Expression）がSDKを公開。「MEVを悪から善に変える」アーキテクチャ。

**SUAVE の設計**

```
SUAVE = TEE（Trusted Execution Environment）上で動く
         クロスドメインMEVオークションフレームワーク

特徴:
├── Programmable privacy: ユーザーのintentを暗号化したままマッチング
├── Cross-domain: ETH + L2 + Solana の統一MEVオークション
├── KEK (Key Escrow + Encryption): ユーザーが自分のMEVを競売にかける
└── MEVシェア: サーチャーのMEV利益をユーザーに還元する仕組み
```

**SUAVE SDK（公開中）**

- Go/Rust ベースのライブラリ
- TEEオラクル連携ツール
- クロスドメインバンドル設計ツール
- MEVシェアの計算・分配ロジック

---

## 4. Jito（Solana MEVインフラ）詳細

### アーキテクチャ

```
Jito Block Engine（Solana固有のMEVインフラ）

├── Jito-Solana バリデータクライアント
│   └── バリデータの80%が採用
│
├── Bundle処理
│   ├── アトミックなtx群（ETHのFlashbotsバンドルと同等）
│   └── Tip（SOL）で優先度決定
│
└── Tip支払い
    └── サーチャー → バリデータ（Relay不要の直接モデル）
```

### ETH Flashbots vs Jito比較

| 観点 | Flashbots (ETH) | Jito (Solana) |
|---|---|---|
| アーキテクチャ | Builder中介モデル（Relay経由） | バリデータ直接tip |
| 採用率 | ETHブロックの90%+ | Solanaバリデータの80%+ |
| 入札単位 | Gwei（ETH手数料） | SOL tip |
| 原子性 | ブロック内完全原子 | ほぼ同等 |
| Mempool | 公開mempool（front-runリスク） | Jitoが一時的なプライベートmempool提供 |
| インフラ成熟度 | 高（SUAVE等の次世代も） | 中（ePBS相当がない） |

---

## 5. 2026年以降のMEV展望

```
短期（2026年）:
├── Glamsterdam ePBSでETH builder寡占が一部緩和
├── AI-on-AI競争でsearcher参入コストが急上昇
└── SUAVE SDKの商業実装が登場

中期（2027〜2028年）:
├── クロスチェーンMEVが主戦場に移行
├── Intent/Solverモデルで従来MEVの一部が合法化・共有化
└── MEVシェア（ユーザー還元）がDeFiプロトコルの標準機能に

長期（2029〜）:
└── AI-native MEVと人間運営MEVの分離
    → AI: 高頻度・マイクロarb
    → 人間: 戦略的・クロスチェーン・情報優位のポジション
```

---

*関連: [mev-ton-opportunity.md](mev-ton-opportunity.md) — TON MEV機会 + 個人研究資産活用*
*関連: [defi-trends.md](defi-trends.md) — DeFiトレンド（MEVと連動）*
*作成: 2026-03-28*
