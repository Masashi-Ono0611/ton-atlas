# Ethereum / Base エコシステム動向

> 調査日: 2026-03-28
> 情報源: Ethereum Foundation, Coinbase Developer Platform, a16z crypto

---

## 1. Ethereum 本体

### 戦略ポジション: "Agent Economy の決済・調整レイヤー"

| 指標 | 数値 |
|---|---|
| DeFi TVL | $56.3B |
| ステーブルコイン残高 | $158B（史上最高） |
| 次期アップグレード | Glamsterdam（May 2026） |
| ETF新申請 | BlackRock / Fidelity がステーキング付きETH ETFをSEC申請中 |

---

### ERC-8004: "Trustless Agents" 標準（2026-01-29 Mainnetデプロイ）

起草者: MetaMask, Ethereum Foundation dAI Team, Google, Coinbase

AIエージェントのためのオンチェーンID・レピュテーション・バリデーション標準。現時点で**最も前進したエージェントID標準**。

#### 3つのレジストリ構造

```
ERC-8004
├── Identity Registry
│   ├── ERC-721ベースのエージェントID（NFT）
│   ├── Agent Card JSON: MCP/A2Aエンドポイント記述
│   └── オーナー（人間 or 組織）との紐付け
│
├── Reputation Registry
│   ├── クライアント（人間・エージェント双方）からの構造化フィードバック
│   ├── スコアの集計・重み付けロジック
│   └── 時間加重型（古いフィードバックの影響低下）
│
└── Validation Registry
    ├── zkML: AIモデルの推論整合性をゼロ知識証明で検証
    ├── TEEオラクル: TEE内実行の整合性証明
    └── ステーク再実行: 複数バリデータが同一実行を再現
```

#### 展開状況

- EVM互換チェーンに展開済み: Base, Taiko, Avalanche C-Chain等
- **v2開発中**: MCPサポート強化 + x402統合
- 設計思想: 決済・ビジネスモデルは持たず「発見と信頼の共通レール」に特化

#### TONビルダーへの示唆

ERC-8004はEVM専用。TONエコシステムのエージェントID標準は未確立。
TAMP・AWikiが先行しているが、ERC-8004との相互運用性がない。
→ **TON ↔ EVM クロスチェーンエージェントIDブリッジ** がホワイトスペース。

---

### Ethereum Foundation dAI チーム（2025年9月発足）

- エージェント決済・ID標準の策定に特化
- ERC-8004, ERC-6551（Token Bound Account）, x402との統合を推進
- 2026年の活動: A2Aプロトコル実装、クロスチェーン標準化

---

### Glamsterdam アップグレード（May 2026）

| 変更 | 内容 |
|---|---|
| **ePBS (Enshrined Proposer-Builder Separation)** | MEV-Boostの外部依存をプロトコルに内包 |
| Inclusion Lists | 検閲耐性の強化（Proposerが強制トランザクション設定可） |
| EOF (EVM Object Format) | EVMのバイトコードを構造化し安全性向上 |

**ePBSの影響:**
- beaverbuild + Titan（現在90%超）の寡占状態が変化
- 小規模builderの参入障壁が低下
- MEVの再分配がより透明化

---

## 2. Base（Coinbase L2）

### 戦略ポジション: "企業・機関エージェント"のホーム

| 指標 | 数値 |
|---|---|
| L2 アクティブアドレスシェア | 約70% |
| L2 DeFi TVLシェア | 46.58% |
| TVL | $12.64B |
| AI agent トークン市場規模 | $3.0B |

---

### Coinbase Agentic Wallets（2026-02-11）

**「企業がAIエージェントにお金を持たせる」ための公式インフラ**

| 機能 | 詳細 |
|---|---|
| 自律資金保有 | x402プロトコル経由でエージェントが独立して資金保有・決済 |
| プログラマブルガードレール | セッション上限・トランザクション上限・ホワイトリスト |
| スケール | ローンチ50日で**5,000万トランザクション**処理 |
| 平均決済額 | $0.20 |
| 日次決済額 | $28,000（実験段階） |

**TONのAgentGuardとの比較:**
- AgentGuard（TON): オンチェーンセッション制御、時限・支出上限
- Coinbase Agentic Wallets: クラウドホスト型、x402統合済み、5,000万tx実績
- → AgentGuardの方向性は正しいが、実績・統合度で1年以上の差

---

### x402 プロトコル（Coinbase + Cloudflare）

| バージョン | 内容 |
|---|---|
| v1 | HTTP 402レスポンスに乗るM2M即時ステーブルコイン決済 |
| **v2 (Dec 2025)** | 全決済データをHTTPヘッダに格納、ボディを解放。より軽量化 |

**採用状況:**
- Stripe, PayPal統合済み
- Solana: x402決済の65%シェア
- TON: x402 SDK、TonPay 402等のコミュニティ実装が先行

---

### World (Sam Altman) × Coinbase AgentKit（2026-03-17）

```
World ID（生体認証 → ZKP） + Coinbase AgentKit（x402） =

「人間が背後にいること」 × 「自律的にお金を使えるエージェント」
```

- ZKPでプライバシー保護しつつ「1人1エージェント上限」を実現
- World認証ユーザー: **1,800万人**（160カ国）
- x402 + World ID = 決済の「how」+ 身元の「who」を統合

**TONへの応用可能性:**
- World IDはOrb（虹彩スキャン端末）が必要 → インフラコストが高い
- Telegram電話番号認証でより軽量な「KYA on TON」が実現可能（詳細: [ideation/05-kya-ton-identity.md](../ideation/05-kya-ton-identity.md)）

---

### Base主要エコシステムプロジェクト

| プロジェクト | 概要 | 規模 |
|---|---|---|
| **Virtuals Protocol** | エージェント経済GDP(aGDP)のインフラ。15,800+プロジェクト | aGDP $477M |
| **G.A.M.E.** | Virtualsのエージェント実行エンジン、AgentKit統合 | — |
| **aixbt** | X上で自律的に動くAIエージェント | 465K+フォロワー |
| **Moltbook** | エージェント専用Redditスタイルソーシャル | — |
| **openwork** | エージェントが互いに雇い合うオンチェーンギグエコノミー | — |
| **clawnet** | エージェント版LinkedInスタイルのプロフェッショナルネットワーク | — |

---

## 3. ETH / Base のTONへの示唆

| ETH/Base動向 | TONへの示唆 |
|---|---|
| ERC-8004 Mainnet（エージェントID標準） | TONのTAMP/AWikiはまだ初期。クロスチェーン互換性が必要 |
| Coinbase Agentic Wallets（5,000万tx） | AgentGuardの方向性は正しいが速度・実績で劣後 |
| x402 v2（軽量ヘッダ化） | TONのx402実装はv1水準。v2対応が必要 |
| ePBS（MEV民主化） | TONはMEVインフラ自体がない → 参考にすべき設計 |
| Virtuals/G.A.M.E.（エージェントトークン化） | TON固有のGifts・Stars経済圏でのエージェントトークン化は未着手 |
| World ID + x402（決済×身元） | Telegram IDで軽量な代替が設計可能 |

---

*関連: [cross-chain-protocols.md](cross-chain-protocols.md) — エコシステム横断プロトコルスタック*
*関連: [ton-vs-ecosystem.md](ton-vs-ecosystem.md) — TONのポジション分析*
*作成: 2026-03-28*
