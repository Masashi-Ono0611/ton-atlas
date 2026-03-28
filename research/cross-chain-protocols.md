# クロスチェーン共通プロトコルスタック比較

> 調査日: 2026-03-28
> 対象: M2M決済プロトコル / エージェントID標準 / AIエージェントフレームワーク

---

## 1. M2M決済プロトコル競争

2026年に乱立するエージェント間・API間決済プロトコルの比較。

| プロトコル | 発行元 | 対象 | 特徴 | チェーン |
|---|---|---|---|---|
| **x402** | Coinbase + Cloudflare | M2M / API課金 | HTTPヘッダベース, USDC, プロトコルミニマリズム。V2でボディを解放 | Base / Solana（65%） / ETH |
| **Stripe MPP** (Machine Payments Protocol) | Stripe | M2M + フィアット | フィアット・クリプト両対応。Stripe決済インフラ活用 | チェーン非依存 |
| **Google UCP** (Universal Commerce Protocol) | Google（60+パートナー） | 高インテント検索 | 検索クエリに紐付いた決済、広告クリックとの統合 | チェーン非依存 |
| **Visa Verifiable Intent** | Visa + Google | コンプライアンス | 「認可・行動・決済」を暗号学的にリンク。KYC/AML対応 | カードネットワーク連動 |
| **ACP** (Agentic Commerce Protocol) | OpenAI + Stripe | 会話型商取引 | 会話文脈から商品発見・決済を統合。ChatGPT向け | チェーン非依存 |
| **Open Wallet Standard** | MoonPay + PayPal + ETH/SOL Foundation | ウォレット統合 | 1シードフレーズ8チェーン対応。TON含む | TON/ETH/SOL/他 |
| **TON Pay** | TON Foundation | TMA/Webアプリ | TON公式マーチャント決済。1秒, 1セント以下 | TON |

### 決済プロトコルの棲み分け

```
Layer 0: ウォレット統合
└── Open Wallet Standard（MoonPay）: 8チェーン共通シードフレーズ

Layer 1: HTTP/API決済（M2M）
├── x402（Coinbase）: デファクトM2M標準、65% on Solana
├── Stripe MPP: フィアット・クリプト橋渡し
└── Google UCP: 検索連動

Layer 2: ビジネスロジック決済
├── Visa Verifiable Intent: コンプライアンス特化
└── ACP (OpenAI): 会話型商取引

Layer 3: チェーン固有
├── TON Pay: TMA/Webマーチャント向け
├── Jito (Solana): MEVバンドル決済
└── Flashbots MEV-Boost (ETH): Builder入札
```

### x402 が事実上のデファクトになりつつある理由

1. **プロトコルミニマリズム**: HTTP 402ステータスコードの再利用 → 既存インフラと親和性
2. **Coinbase + Cloudflare の共同発行** → 信頼性と普及力
3. **V2でヘッダのみ** → ペイロードのエンコードが不要、軽量
4. **Stripe・PayPalが統合済み** → リアル商業取引への橋渡し
5. **Solana 65%シェア** → 最も使われているM2M決済

---

## 2. エージェントID・信頼スタック

| 標準 | 発行元 | ステータス | 概要 |
|---|---|---|---|
| **ERC-8004** | ETH Foundation dAI | ✅ Mainnet (Jan 2026) | 3レジストリ: Identity(ERC-721) / Reputation / Validation(zkML+TEE) |
| **KYA (Know Your Agent)** | a16z crypto | 🔵 提唱中 | エージェントを人間プリンシパルに暗号学的紐付け。2026年の最重要プリミティブと予測 |
| **World AgentKit** | World + Coinbase | ✅ 実装済み (Mar 2026) | World ID（ZKP生体認証）をエージェントに委任、1人1エージェント上限 |
| **TAMP** (TON Agent Manifest Protocol) | TON Builder | 🟡 開発中 | TONスマートコントラクト上のエージェント発見・レピュテーション層、MCP統合 |
| **AWiki** | TON Builder | 🟡 開発中 | W3C DID + TON DNSによるエージェントID・通信・決済統合 |
| **ERC-6551** (Token Bound Account) | Ethereum | ✅ 実装済み | NFTがウォレットを持てる標準（エージェントIDへの応用） |

### TON Agent ID のギャップ

```
ERC-8004 (ETH): ████████████████████ 成熟、Mainnetデプロイ済み
KYA (a16z):     ████░░░░░░░░░░░░░░░░ 提唱段階、実装なし
World AgentKit: ████████░░░░░░░░░░░░ 実装済みだがOrb端末必要
TAMP (TON):     ████░░░░░░░░░░░░░░░░ 初期開発中
AWiki (TON):    ███░░░░░░░░░░░░░░░░░ 初期開発中
```

**TONは最も遅れている。**
ただし、Telegram電話番号 + TON DNSを組み合わせた「軽量KYA」を設計すれば、World IDより低コストで実現できる可能性がある。

---

## 3. AIエージェントフレームワーク

| フレームワーク | 主要チェーン | Stars/規模 | 特徴 |
|---|---|---|---|
| **elizaOS** | Solana / Base / EVM | 17,600 GitHub Stars | デファクトOS。200+プラグイン、1,350+コントリビューター。TypeScript-first |
| **Virtuals G.A.M.E.** | Base / Solana | aGDP $477M | エージェントトークン化・経済化、完了ジョブ178万件 |
| **NEAR AI** | NEAR | NEAR Intents統合 | アジェンティックコマース特化、ソルバーネットワーク連動 |
| **Coinbase AgentKit** | Base | 5,000万tx | x402・プログラマブルガードレール、Enterprise向け |
| **Teleton** | TON | 35 claps | MTProto本物TGユーザーとして動作、Plugin SDK |
| **TON Agent Platform** | TON | 266/20 claps | ノーコード、84ツール、7 AIプロバイダー、ビジュアルワークフロー |

### elizaOS vs TON固有フレームワーク

| 観点 | elizaOS | TON固有（Teleton等） |
|---|---|---|
| エコシステム | 巨大（17,600 stars）、200+プラグイン | 小規模、Telegram-native |
| Telegramとの統合 | プラグインで対応（Bot APIレベル） | MTProtoで本物ユーザーとして動作 |
| TONウォレット | コミュニティプラグイン（非公式） | ネイティブ統合 |
| DeFi操作 | Uniswap/Aave等のプラグイン豊富 | DeDust/STONfi特化 |
| 展開コスト | 自己ホスト / クラウド | TelegramアカウントSIMが必要 |

---

## 4. クロスチェーンの共通「レッドオーシャン」

どのエコシステムでも乱立しており、新規参入が困難な領域:

| 領域 | 状況 |
|---|---|
| 汎用AIエージェントSDK/フレームワーク | elizaOS, Virtuals, AgentKit等が覇権争い |
| ウォレット接続ライブラリ | TON Connect, WalletConnect等の既成標準 |
| チェーン汎用MCPサーバー | 各チェーンで10+プロジェクトが乱立 |
| スマートコントラクト監査ツール | AI監査は成熟市場 |
| 汎用DeFi取引エージェント | PanoramaBlock等が競合 |

---

## 5. 標準化競争の現時点での勝者

```
決済プロトコル:    x402 ✅（Coinbase公式 + Solana65% + Stripe/PayPal統合）
エージェントID:    ERC-8004 ✅（ETH Mainnet、業界標準化進行中）
エージェントOS:    elizaOS ✅（17,600 stars、デファクト）
ウォレット統合:    Open Wallet Standard 🔵（8チェーン、MoonPay主導）
人間-エージェント: KYA × World ID 🔵（実装済みだが普及未）
TON-native:       未確定（TAMP/AWikiが競争中、公式AgenticKitがQ1に来る）
```

---

*関連: [chain-ethereum-base.md](chain-ethereum-base.md)*
*関連: [chain-solana.md](chain-solana.md)*
*関連: [chain-near-sui-aptos.md](chain-near-sui-aptos.md)*
*関連: [ton-vs-ecosystem.md](ton-vs-ecosystem.md)*
*作成: 2026-03-28*
