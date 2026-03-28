# TON Atlas — TON Ecosystem Research & Ideation

TON/Telegram エコシステムの深層調査とプロダクトアイデアのリポジトリ。
DeFi・MEV・AIエージェント・決済インフラ、そして検閲耐性・自由技術まで。

## ディレクトリ構成

```
ton-atlas/
├── research/                    # エコシステム調査・競合分析・市場動向
│   ├── _index.md                ← ナビゲーションインデックス（ここから始める）
│   │
│   ├── ton/                     # TON エコシステム
│   │   ├── hackathon-projects.md     ← 全200件プロジェクト生データ
│   │   ├── hackathon-trends.md       ← 7大トレンド・競合マップ
│   │   ├── roadmap-products.md       ← AgenticKit・AppKit・TON Pay
│   │   ├── strategy.md               ← TON Foundation戦略・Gateway 2026
│   │   └── vs-ecosystem.md           ← 他チェーンとのポジション比較
│   │
│   ├── chains/                  # クロスチェーン・エコシステム
│   │   ├── ethereum-base.md          ← ERC-8004, ePBS, Coinbase AgentKit
│   │   ├── solana.md                 ← elizaOS, Jito, Alpenglow
│   │   ├── near-sui-aptos.md         ← NEAR Intents, Sui, Aptos
│   │   └── cross-chain-protocols.md  ← x402, Open Wallet, Agent ID標準
│   │
│   ├── defi-mev/                # DeFi・MEV
│   │   ├── defi-trends.md            ← Intent DeFi, yield SB, RWA, AI×DeFi
│   │   ├── mev-landscape.md          ← Flashbots, ePBS, SUAVE, Jito
│   │   └── mev-ton-opportunity.md    ← TON MEV機会・既存研究資産活用
│   │
│   └── sovereignty/             # 通信の自由・検閲耐性（NEW）
│       ├── decentralized-communication.md  ← bitchat, TON Proxy, Nostr, Session
│       └── freedom-tech-philosophy.md      ← Cypherpunk思想・「Can't be evil」
│
├── ideation/                    # プロダクトコンセプト
│   ├── README.md                ← アイデア一覧と選定根拠
│   ├── 01-gasless-agent-payment.md   ★★★ AgentPay
│   ├── 02-telegram-gifts-defi.md     ★★★ GiftFi
│   ├── 03-ton-intent-solver.md       ★★  IntentLayer TON
│   ├── 04-ton-mev-infra.md           ★★  TON MEV Infra
│   ├── 05-kya-ton-identity.md        ★★  KYA on TON
│   └── 06-freedom-stack.md           ★★  TON Freedom Stack
│
├── references/                  # 参考資料・リンク
└── notes/                       # メモ・ブレスト（.gitignore済み）
```

## テーマ

- **TON / Telegram**: TON Blockchain, Telegram Mini Apps, TON Connect, Jetton, NFT, Gifts
- **DeFi**: DEX, Lending, Yield, Liquidity, Intent-based trading, MEV
- **AI Agent**: Autonomous agents, LLM × on-chain, agent wallet, A2A payment
- **Payment**: P2P payment, M2M payment, cross-chain settlement, gasless UX
- **Sovereignty**: Censorship-resistant communication, Bluetooth mesh, TON Proxy/Sites/Storage, Cypherpunk philosophy

## 進め方

1. `research/` の各カテゴリで調査を蓄積
2. `ideation/` にプロダクトアイデアをMD形式で記録
3. `research/_index.md` がナビゲーションの起点
