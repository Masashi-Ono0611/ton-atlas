# Research ナビゲーションインデックス

> 最終更新: 2026-03-28
> 全体調査サマリーと各ファイルへのガイド

---

## ディレクトリ構成

```
research/
├── _index.md                   ← このファイル（ナビゲーション）
│
├── # ハッカソン調査（identityhub.app）
├── hackathon-projects.md       ← 全200件プロジェクト生データ
├── hackathon-trends.md         ← 7大トレンド・競合マップ・ホワイトスペース分析
│
├── # TON公式エコシステム
├── ton-roadmap-products.md     ← AgenticKit・AppKit・TON Pay ロードマップ詳細
├── ton-strategy.md             ← TON Foundation戦略・公式メッセージ分析
│
├── # チェーン別分析
├── chain-ethereum-base.md      ← ETH (ERC-8004, Glamsterdam) + Base (AgentKit, x402)
├── chain-solana.md             ← Solana (elizaOS, Alpenglow, Jito, x402)
├── chain-near-sui-aptos.md     ← NEAR (Intents) + Sui + Aptos
├── cross-chain-protocols.md    ← 決済プロトコル・エージェントID・フレームワーク横断比較
├── ton-vs-ecosystem.md         ← TONのポジション分析・ホワイトスペース
│
├── # DeFi・MEV
├── defi-trends.md              ← DeFi Q1 2026: Intent・yield SB・RWA・CLOB・AI×DeFi
├── mev-landscape.md            ← MEVグローバル動向: AI軍拡・クロスチェーン・ePBS・SUAVE・Jito
└── mev-ton-opportunity.md      ← TON MEV機会 + 既存Flashbots研究資産の活用マップ
```

---

## クイックリファレンス: 知りたいことから探す

### 「どんなプロジェクトが作られているか知りたい」

→ [hackathon-projects.md](hackathon-projects.md)（生データ）
→ [hackathon-trends.md](hackathon-trends.md)（トレンド分析）

### 「TON公式のロードマップを知りたい」

→ [ton-roadmap-products.md](ton-roadmap-products.md)（AgenticKit・AppKit・TON Pay）
→ [ton-strategy.md](ton-strategy.md)（戦略・メッセージ・Gateway 2026）

### 「他のチェーンがどうやっているか知りたい」

→ [chain-ethereum-base.md](chain-ethereum-base.md)（ERC-8004・Coinbase AgentKit）
→ [chain-solana.md](chain-solana.md)（elizaOS・Jito・Alpenglow）
→ [chain-near-sui-aptos.md](chain-near-sui-aptos.md)（NEAR Intents・Sui・Aptos）
→ [cross-chain-protocols.md](cross-chain-protocols.md)（決済・ID・フレームワーク標準化）

### 「TONの競争優位と弱点を知りたい」

→ [ton-vs-ecosystem.md](ton-vs-ecosystem.md)

### 「DeFiの最新トレンドを知りたい」

→ [defi-trends.md](defi-trends.md)

### 「MEVとその機会を知りたい」

→ [mev-landscape.md](mev-landscape.md)（グローバル動向）
→ [mev-ton-opportunity.md](mev-ton-opportunity.md)（TON固有機会・既存研究活用）

---

## エグゼクティブサマリー

### 調査の結論

#### TONの唯一の構造的優位

技術・TVL・ツール・プロトコル標準化は全て他チェーンが先行している。
**唯一の決定的差異: Telegram 10億MAUとの統合**。

> "10億人が毎日生活するネットワークは、その争いに勝たなくていい。すでに答えだから。" — Max Crown (TON CEO)

#### 3つのホワイトスペース（最重要）

| ホワイトスペース | 根拠 |
|---|---|
| **Telegram Gifts × DeFi金融化** | TON固有資産、ハッカソン200件で担保/レンディングは0件 |
| **AIエージェント向けGasless決済** | AppKit Gas Bankより先に汎用SDK、MEV知識を転用 |
| **TON MEVインフラ** | Flashbots/Jitoが証明したファーストムーバー優位、TONは空白 |

#### 競合状況のサマリー

| レイヤー | 競合度 |
|---|---|
| MCPサーバー / SDK | 🔴 レッドオーシャン（10+プロジェクト） |
| x402 M2M決済 | 🟡 競合増加中（差別化が重要） |
| セキュリティ・信頼スコア | 🔴 過密・同質化 |
| Gifts経済圏（分析・アービトラージ） | 🟡 活発だが上位レイヤーは空白 |
| A2A / エージェントID | 🟢 初期段階（標準化競争が始まる） |
| MEVインフラ | 🟢 完全な空白 |
| NFT担保レンディング | 🟢 完全な空白（Gifts特化） |

---

## 調査の時系列と信頼度

| ファイル | 情報ソース | 信頼度 |
|---|---|---|
| hackathon-projects.md | identityhub.appから直接クローリング | ★★★ |
| hackathon-trends.md | 上記データの分析 | ★★★ |
| ton-roadmap-products.md | ton.org公式ページ | ★★★ |
| ton-strategy.md | ton.org公式ブログ・X | ★★★ |
| chain-ethereum-base.md | ETH Foundation・Coinbase公式 | ★★★ |
| chain-solana.md | Solana Foundation・elizaOS公式 | ★★★ |
| chain-near-sui-aptos.md | 各公式・公開情報 | ★★ |
| cross-chain-protocols.md | 複数一次ソース | ★★★ |
| ton-vs-ecosystem.md | 上記全調査からの分析 | ★★ |
| defi-trends.md | 公開データ・各プロジェクト公式 | ★★★ |
| mev-landscape.md | Flashbots研究・公開データ | ★★★ |
| mev-ton-opportunity.md | 既存コード分析 + 推論 | ★★（推論含む） |

---

*ideationへ: [../ideation/README.md](../ideation/README.md)*
*最終調査日: 2026-03-28*
