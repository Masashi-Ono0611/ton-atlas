# Solana エコシステム動向

> 調査日: 2026-03-28
> 情報源: Solana Foundation, elizaOS, Jito Labs

---

## 1. 戦略ポジション

**"高頻度エージェント取引のデフォルトチェーン"**

Solana Foundationは明言している:
> "99.99%のオンチェーントランザクションは2年以内にエージェント・ボット・LLMベースのウォレットによって実行される" — Vibhu Norby, Solana Foundation

この予測の根拠として、すでにx402決済の65%がSolana上で処理されている。

---

## 2. ネットワーク指標

| 指標 | 数値 |
|---|---|
| DeFi TVL | $11.5B |
| Firedancer後のスループット（テスト） | 65,000 TPS（内部テスト: 1,000,000 TPS目標） |
| ブロックファイナリティ | 400ms以下 |
| トランザクション手数料 | $0.001以下 |
| x402決済のSolanaシェア | 65% |
| elizaOS GitHub Stars | 17,600+ |

---

## 3. 技術ロードマップ

### Alpenglow（SIMD-0326）— 最重要アップグレード

**Proof of Historyを廃止する抜本的なコンセンサス刷新**

| 変更 | 内容 |
|---|---|
| PoH廃止 | ブロック検証をリーダーベースから全バリデータ参加型へ |
| ファイナリティ | 現在400ms → **150ms** に短縮 |
| スループット | 同等コストでより高い処理能力 |
| MEV影響 | リーダー順序の予測困難化 → front-runningがより難しく |

**Alpenglow後のSolana:**
- ETHのGlamstertadm (May 2026) と同時期のアップグレード競争
- 150ms × $0.001 = AI高頻度取引に最適化した設計

### P-token format

- SPLトークンプログラム全面刷新
- 計算コスト95〜98%削減
- マイクロペイメントの経済的成立範囲が拡大

---

## 4. 規制面の重大進展

| 出来事 | 日付 | 意義 |
|---|---|---|
| SECがSOLを「デジタルコモディティ」に分類 | 2026-03-22 | 機関参入の法的障壁が解消 |
| Walmart OnePay にSOL決済統合 | 2026-03-22 | 月間アクティブユーザー300万人、実店舗利用 |
| Mastercard・Western Union がSolana Developer Platform参入 | 2026 Q1 | 伝統的金融機関のSolana採用 |

SEC分類の影響は大きい。Ethereum(SEC訴訟)とは対照的にSolanaは規制クリアで機関資金が流入しやすい状態。

---

## 5. elizaOS — デファクトAIエージェントOS

### 概要

| 指標 | 数値 |
|---|---|
| GitHub Stars | 17,600+ |
| フォーク数 | 5,300 |
| コントリビューター | 1,350+ |
| プラグイン数 | 200+ |

### アーキテクチャ

```
elizaOS
├── Core Runtime
│   ├── Character（エージェントの人格・知識定義）
│   ├── Memory（会話履歴・長期記憶）
│   └── Actions（プラグイン実行エンジン）
│
├── Platform Adapters（同一エージェントで複数プラットフォーム）
│   ├── X (Twitter)
│   ├── Discord
│   ├── Telegram
│   └── Browser
│
└── Plugin Ecosystem（200+）
    ├── DeFi: Uniswap, Aave, Curve, Jupiter
    ├── Chain: Solana, EVM, TON（コミュニティ製）
    ├── Payment: x402
    └── Data: Dune, The Graph
```

### TONとの関係

- TON版はコミュニティ製プラグインとして存在（OpenClaw, Teleton等がelizaOSを参考）
- Teletonは「本物Telegramユーザーとして動作」という独自進化（elizaOS非互換）
- **elizaOSのプラグインとして使えるTON公式プラグインが存在しない** → ビジネス機会

---

## 6. Jito — Solana MEVインフラ

### 概要

Solana版のFlashbotsに相当するMEVインフラ。バリデータがJitoクライアントを採用することで機能する。

| 要素 | 詳細 |
|---|---|
| アーキテクチャ | バリデータがJitoのBlock Engine経由でバンドルを受け取る |
| バンドル | アトミックなトランザクション群。ETHのFlashbotsバンドルと同等概念 |
| Tip | サーチャーがバリデータにSOLでチップ（ETHのpriority fee相当） |
| Jito-Solana | Jito改修済みバリデータクライアント（全バリデータの〜80%が採用） |
| ブロック占有率 | Solanaブロックの60〜70%がJito経由 |

### FlashbotsとJitoの比較

| 観点 | Flashbots (ETH) | Jito (Solana) |
|---|---|---|
| バンドル単位 | 複数tx、同一ブロック内原子 | 同左 |
| オークション | Builder経由のアクション入札 | バリデータへの直接tip入札 |
| Mempool | 公開mempoolあり（front-runリスク） | 部分的（リーダーが事前知識あり） |
| インフラ成熟度 | 高（MEV-Boost, SUAVE) | 中（Jito単一依存、ePBS未） |
| 採用率 | Ethereumブロックの90%+ | Solanaバリデータの80%+ |

### TONとの比較

TONにJito・Flashbotsに相当するインフラは存在しない（詳細: [mev-ton-opportunity.md](mev-ton-opportunity.md)）。

---

## 7. Virtuals Protocol（Solana拡張）

Base発祥 → Solanaにも展開。

| 指標 | 数値 |
|---|---|
| aGDP（エージェント経済GDP） | $477M |
| 完了ジョブ数 | 178万件 |
| ユニークウォレット | 23,514 |

**G.A.M.E.（エージェント実行エンジン）:**
- Virtualsのオンチェーンエージェントランタイム
- Coinbase AgentKit, elizaOSと統合
- エージェントのタスク実行を自動的にトークン化・報酬化

---

## 8. TONへの示唆

| Solana動向 | TONへの示唆 |
|---|---|
| elizaOS（17,600 stars）がデファクトOS | TON公式がelizaOSプラグインを出せばエコシステムが拡大 |
| Alpenglow（150ms finality） | New TON Consensusがこれに対抗できるか要注目 |
| x402の65%シェア | TONのx402実装はまだコミュニティレベル → 公式実装が必要 |
| Jito（80%採用MEVインフラ） | TON MEVインフラは空白 → ファーストムーバーチャンス |
| SEC「コモディティ」分類 | TONの規制状況は不透明 → 新興国フォーカスが現実的 |
| Virtuals/G.A.M.E.（aGDP $477M） | TON GiftsエコノミーのaGDP化が未着手 |

---

*関連: [cross-chain-protocols.md](cross-chain-protocols.md)*
*関連: [mev-ton-opportunity.md](mev-ton-opportunity.md) — Jito比較でのTON MEV機会*
*作成: 2026-03-28*
