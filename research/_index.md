# Research ナビゲーションインデックス

> 最終更新: 2026-03-28
> 全体調査サマリーと各ファイルへのガイド

---

## ディレクトリ構成

```
research/
├── _index.md                        ← このファイル（ナビゲーション）
│
├── ton/                             # TON エコシステム
│   ├── hackathon-projects.md        ← 全200件プロジェクト生データ
│   ├── hackathon-trends.md          ← 7大トレンド・競合マップ・ホワイトスペース分析
│   ├── roadmap-products.md          ← AgenticKit・AppKit・TON Pay ロードマップ詳細
│   ├── strategy.md                  ← TON Foundation戦略・公式メッセージ分析
│   └── vs-ecosystem.md              ← TONのポジション分析・ホワイトスペース
│
├── chains/                          # クロスチェーン・エコシステム
│   ├── ethereum-base.md             ← ETH (ERC-8004, Glamsterdam) + Base (AgentKit, x402)
│   ├── solana.md                    ← Solana (elizaOS, Alpenglow, Jito, x402)
│   ├── near-sui-aptos.md            ← NEAR (Intents) + Sui + Aptos
│   └── cross-chain-protocols.md     ← 決済プロトコル・エージェントID・フレームワーク横断比較
│
├── defi-mev/                        # DeFi・MEV
│   ├── defi-trends.md               ← DeFi Q1 2026: Intent・yield SB・RWA・CLOB・AI×DeFi
│   ├── mev-landscape.md             ← MEVグローバル動向: AI軍拡・クロスチェーン・ePBS・SUAVE・Jito
│   └── mev-ton-opportunity.md       ← TON MEV機会 + 既存Flashbots研究資産の活用マップ
│
└── sovereignty/                     # 通信の自由・検閲耐性（NEW）
    ├── decentralized-communication.md  ← bitchat・TON Proxy・Nostr・Session等の技術ランドスケープ
    └── freedom-tech-philosophy.md      ← Cypherpunk運動の系譜・「Can't be evil」思想
```

---

## クイックリファレンス: 知りたいことから探す

### 「どんなプロジェクトが作られているか知りたい」

→ [ton/hackathon-projects.md](ton/hackathon-projects.md)（生データ）
→ [ton/hackathon-trends.md](ton/hackathon-trends.md)（トレンド分析）

### 「TON公式のロードマップを知りたい」

→ [ton/roadmap-products.md](ton/roadmap-products.md)（AgenticKit・AppKit・TON Pay）
→ [ton/strategy.md](ton/strategy.md)（戦略・メッセージ・Gateway 2026）

### 「他のチェーンがどうやっているか知りたい」

→ [chains/ethereum-base.md](chains/ethereum-base.md)（ERC-8004・Coinbase AgentKit）
→ [chains/solana.md](chains/solana.md)（elizaOS・Jito・Alpenglow）
→ [chains/near-sui-aptos.md](chains/near-sui-aptos.md)（NEAR Intents・Sui・Aptos）
→ [chains/cross-chain-protocols.md](chains/cross-chain-protocols.md)（決済・ID・フレームワーク標準化）

### 「TONの競争優位と弱点を知りたい」

→ [ton/vs-ecosystem.md](ton/vs-ecosystem.md)

### 「DeFiの最新トレンドを知りたい」

→ [defi-mev/defi-trends.md](defi-mev/defi-trends.md)

### 「MEVとその機会を知りたい」

→ [defi-mev/mev-landscape.md](defi-mev/mev-landscape.md)（グローバル動向）
→ [defi-mev/mev-ton-opportunity.md](defi-mev/mev-ton-opportunity.md)（TON固有機会・既存研究活用）

### 「検閲耐性・通信の自由の技術を知りたい」

→ [sovereignty/decentralized-communication.md](sovereignty/decentralized-communication.md)（bitchat・TON Proxy・Nostr等）
→ [sovereignty/freedom-tech-philosophy.md](sovereignty/freedom-tech-philosophy.md)（Cypherpunk思想・「Can't be evil」）

---

## エグゼクティブサマリー

### 調査の結論

#### TONの唯一の構造的優位

技術・TVL・ツール・プロトコル標準化は全て他チェーンが先行している。
**唯一の決定的差異: Telegram 10億MAUとの統合**。

> "10億人が毎日生活するネットワークは、その争いに勝たなくていい。すでに答えだから。" — Max Crown (TON CEO)

#### 追加の視点: TONの「自由技術」DNA

TONはDurov兄弟が設計した思想を持つ唯一の主要L1:
- **TON Proxy / Sites / Storage**: 稼働中の検閲耐性インフラ（未活用）
- **Telegram 900M**: プライバシーを選んで来たユーザー群
- **「Can't be evil」へのパス**: 技術的に検閲不可能なWebとコミュニケーション

#### 4つのホワイトスペース（最重要）

| ホワイトスペース | 根拠 |
|---|---|
| **Telegram Gifts × DeFi金融化** | TON固有資産、ハッカソン200件で担保/レンディングは0件 |
| **AIエージェント向けGasless決済** | AppKit Gas Bankより先に汎用SDK、MEV知識を転用 |
| **TON MEVインフラ** | Flashbots/Jitoが証明したファーストムーバー優位、TONは空白 |
| **TON Freedom Stack** | TON Proxy/Sites/Storage は稼働中だが「プロダクト」がない。MeshPay×ブラウザで差別化 |

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
| 検閲耐性ブラウザ/決済 | 🟢 完全な空白（TON既存インフラ未活用） |

---

## 調査の時系列と信頼度

| ファイル | 情報ソース | 信頼度 |
|---|---|---|
| ton/hackathon-projects.md | identityhub.appから直接クローリング | ★★★ |
| ton/hackathon-trends.md | 上記データの分析 | ★★★ |
| ton/roadmap-products.md | ton.org公式ページ | ★★★ |
| ton/strategy.md | ton.org公式ブログ・X | ★★★ |
| chains/ethereum-base.md | ETH Foundation・Coinbase公式 | ★★★ |
| chains/solana.md | Solana Foundation・elizaOS公式 | ★★★ |
| chains/near-sui-aptos.md | 各公式・公開情報 | ★★ |
| chains/cross-chain-protocols.md | 複数一次ソース | ★★★ |
| ton/vs-ecosystem.md | 上記全調査からの分析 | ★★ |
| defi-mev/defi-trends.md | 公開データ・各プロジェクト公式 | ★★★ |
| defi-mev/mev-landscape.md | Flashbots研究・公開データ | ★★★ |
| defi-mev/mev-ton-opportunity.md | 既存コード分析 + 推論 | ★★（推論含む） |
| sovereignty/decentralized-communication.md | 公開情報・技術仕様 | ★★★ |
| sovereignty/freedom-tech-philosophy.md | 文献・事実関係 | ★★★ |

---

*ideationへ: [../ideation/README.md](../ideation/README.md)*
*最終調査日: 2026-03-28*
