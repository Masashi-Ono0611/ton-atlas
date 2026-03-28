# ハッカソン トレンド分析・競合マップ・ホワイトスペース

> 分析対象: identityhub.app 2コンテスト（Fast Grants 40件 + AI Hackathon 160件）
> 分析日: 2026-03-28
> 生データ: [hackathon-projects.md](hackathon-projects.md)

---

## 1. 7大トレンド

### 🔥 トレンド1: MCP サーバー化（AIへのTONアクセス開放）

**最大のメガトレンド。** TONエコシステム全体をMCP経由でAIエージェントに開放する動き。

| プロジェクト | 対象ドメイン | Claps |
|---|---|---|
| TON MCP | TON全般操作 | 49 |
| Telegram MCP | Telegram Bot API + MTProto | 94 |
| TON API Skill | TON API 全エンドポイント | 4 |
| TON OpenClaw Skill | DeFi操作（swap/stake/NFT） | 11 |
| Gift Asset MCP | Telegram Gifts市場データ | 8 |
| Bidask MCP | CLMM DEX | 11 |

**競合状況:** すでにレッドオーシャン化。単純なAPI wrappingは差別化困難。

**TON公式ロードマップとのアライン:** AppKit Q2でMCP対応予定。公式が後追いする形。

**次の戦場:** MCPサーバー「の上で何ができるか」— ドメイン特化MCP（Gifts, DNS, DeFi）はまだ参入余地あり。

---

### 🔥 トレンド2: x402 / M2M 決済プロトコル

HTTP 402を使った**エージェント間・エージェント-API間の自動決済**が集中的に開発。

| プロジェクト | アプローチ | Claps |
|---|---|---|
| x402 Payments SDK (TON) | x402プロトコルのTON実装SDK | 3 |
| TonPay 402 | W5 × x402、支出ガードレール付き | 3 |
| ton402 | 決済→JWT即時発行ゲートウェイ | 5 |
| Catallaxy | HTTP 402ベースのエージェントマーケットプレイス | 7 |
| FireFlow | x402ノード統合 + Inference Tokenization | 5 |
| Imago | MCPエージェント × オンチェーンTON決済 | 15 |

**競合状況:** 競合増加中。実装水準にばらつき大。

**主要課題:** non-custodial wallet + autopay のシームレスな統合が未解決。
「エージェントがお金を使う」ことはできるが「誰が管理するか」が不明確。

**次の戦場:** ガードレール（AgentGuardパターン）と組み合わせたセーフな自律支払い。

---

### 🔥 トレンド3: Telegram Gifts エコノミー

Telegram Giftsが独立した金融・投機資産として機能し始め、専門エージェントが急増。

| プロジェクト | 機能 | Claps |
|---|---|---|
| Giftstat AI Bots | フロア価格/時価総額/ボリューム分析 | 11 |
| Gift Asset MCP | 市場データMCP | 8 |
| Morgan Gift Plugins | クロスマーケットアービトラージ（6取引所） | 4 |
| Giftindex / ODROB | 派生商品取引（インデックストークン） | 10 |
| Gifti for TeleGifts | ユーザー向けチャット型アシスタント | 22 |
| TON Agent Platform | ギフトアービトラージエージェント | 15/20 |

**競合状況:** 活発だが、上位レイヤー（担保・レンディング・保険）は未参入。

**独自性:** TON固有市場。他チェーンに相当物なし。TON公式は言及薄い→ボトムアップ市場。

**進化の方向:** アービトラージ → 分析 → デリバティブ → **レンディング・担保化** → 保険。
Giftsをオンチェーン担保として使うプロダクトが次の大きなチャンス。

---

### 🔥 トレンド4: セキュリティ・信頼レイヤー

TON/Telegramの詐欺問題が深刻で、AI×オンチェーンデータによる防衛ツールが急増。

| プロジェクト | アプローチ | Claps |
|---|---|---|
| Ton AI Audit | スマートコントラクトLLM監査 | 49 |
| AgentGuard | エージェント向けオンチェーンセッション制御 | 3 |
| TON-SHA | エージェント実行整合性プリミティブ | 10 |
| VERITAS | ウェブサイト視覚解析+オンチェーン詐欺検出 | 6 |
| Scamified | コントラクトアドレス詐欺スコア | 36 |
| TonTrustGuard | ウォレットリスク分析 | 17 |
| TON Security Agent | 公開REST API型セキュリティスコア | 16 |
| TON-HITL | ヒューマンアプルーバルミドルウェア | 8 |

**競合状況:** 同質化が進む。「スコアを出す」だけでは差別化困難。

**次の戦場:** スコアを提供する → **スコアに基づいてアクションする**（取引ブロック・保険支払い・評判担保）。

---

### 🔥 トレンド5: ノーコードAIエージェントプラットフォーム

技術者以外でもAIエージェントを作れるプラットフォームへの需要。

| プロジェクト | 特徴 | Claps |
|---|---|---|
| SpawnDock | TMA+AIツール2分スキャフォールディング、5 npmパッケージ | **266** |
| PromptCraft | NLでTelegramボット生成 | 19 |
| TON Agent Platform | MTProto本物ユーザー/84ツール/ビジュアルワークフロー | 15/20 |
| FireFlow | エージェントオーケストレーションフレームワーク | 5 |

**SpawnDockが266clapsで圧倒的首位。**
これは「2分で動くもの」「開発者体験の摩擦ゼロ」への強いニーズを示している。

**競合状況:** 少数精鋭化。SpawnDockが事実上の勝者になりつつある。

---

### 🔥 トレンド6: A2A（エージェント間）通信・発見・マーケットプレイス

「エージェントがエージェントを雇う」経済圏の基盤整備。

| プロジェクト | アプローチ | Claps |
|---|---|---|
| TAMP | TONスマートコントラクト上の分散型エージェント発見+MCP統合 | 3 |
| AWiki | W3C DID + TON DNSのエージェントID・通信基盤 | 18 |
| Mesh Network | Teletonエージェント向けP2Pジョブマーケット | 3 |
| Catallaxy | 分散型AIエージェントマーケットプレイス（HTTP 402） | 7 |
| TON AGENT NETWORK | 複数エージェント競合型マーケットプレイス | 6 |
| AgentLayer | Telegramチャンネルを知識ソースとして販売 | 12 |

**競合状況:** 初期段階。標準化されたプロトコルが存在せず、競争は始まったばかり。

**次の戦場:** ERC-8004（Ethereum）と相互運用できるTON-native エージェントIDスタンダード。

---

### 📊 トレンド7: Wallet-native AI

財布の中にAIを埋め込む = 最もユーザーに近い場所からDeFiを操作。

| プロジェクト | 特徴 | Claps |
|---|---|---|
| MyTonWallet AI Agent | MyTonWalletネイティブAI。非カストディアル設計 | 16 |
| TON Agent Platform | 自動財布+エージェント生成 | 20 |
| Teleton Agent | 本物TGアカウント操作+TONウォレット統合 | 35 |

---

## 2. 競合マップ（レイヤー別）

```
Layer 0: ブロックチェーン基盤 (TON)
└── 既成インフラ / 競合ほぼなし

Layer 1: MCPサーバー / SDK / スキル       ← 🔴 最も過密・レッドオーシャン
├── TON MCP (49), Telegram MCP (94)
├── ton-tools (24), tonapi-langchain-tools (42)
├── TON OpenClaw Skill (11), TON API Skill (4)
└── Gift Asset MCP (8), Bidask MCP (11)

Layer 2: 決済インフラ (x402 / M2M)        ← 🟡 競合増加中、差別化余地あり
├── x402 SDK (3), TonPay 402 (3), ton402 (5)
├── Catallaxy (7), Imago (15)
└── FireFlow x402統合 (5)

Layer 3: セキュリティ / 信頼              ← 🔴 過密・同質化
├── Ton AI Audit (49), Scamified (36)
├── TonTrustGuard (17), TON Security Agent (16)
├── AgentGuard (3), TON-SHA (10), VERITAS (6)
└── TON-HITL (8)

Layer 4: エージェントプラットフォーム     ← 🟡 少数精鋭
├── SpawnDock (266) ← 圧倒的
├── TON Agent Platform (20)
├── Teleton Agent (35)
└── PromptCraft (19)

Layer 5: ドメイン特化エージェント         ← 🟢 参入余地あり
├── Gifts: Giftstat (11), Morgan (4), Gifti (22), Giftindex (10)
├── DeFi: Esprito (5), PanoramaBlock (93), Bidask (11)
├── DNS: webdom.market (117)
└── Commerce: AgentLayer (12), Forlarge (3), Jarvis (17)

Layer 6: A2A / エージェントID             ← 🟢 初期段階・最大の機会
├── TAMP (3), AWiki (18)
├── Mesh Network (3), Crypto Claw (4)
└── TON AGENT NETWORK (6), Catallaxy (7)
```

---

## 3. コミュニティ支持シグナルの読み方

### Clapsの分布が示すもの

```
SpawnDock:  266  ████████████████████████████████████████
CleanAds:    96  ███████████████
webdom:     117  ██████████████████
Telegram MCP: 94 ██████████████
PanoramaBlock: 93 ██████████████
---（大きな断絶）---
Cocoon:      61  ██████████
TON MCP:     49  ████████
Ton AI Audit: 49 ████████
---（2桁の世界）---
Most projects: 3-30
```

**示唆:**
1. **「すぐ動くもの」への高い評価** — SpawnDockの「2分で完備環境」は突出
2. **「Telegramそのもの」へのアクセス需要** — Telegram MCP (94) は Bot APIだけでなくMTProtoも含む
3. **マルチチェーン視点** — PanoramaBlock (93) はTON単体でなくマルチチェーンDeFi
4. **Winner != High Claps** — Fast Grants WinnerのTonPay 402はClaps 3のみ。審査基準(PMF×技術×usability)とコミュニティ人気は別

---

## 4. 両ハッカソンで不在のテーマ（ホワイトスペース）

### ◎ 有望度高

| テーマ | 理由 | 現状 |
|---|---|---|
| **Telegram Gifts 担保レンディング** | Gifts = 価値ある資産だが流動性なし → 担保化ニーズが潜在 | なし（Giftindexが派生商品のみ） |
| **TON Intent Layer / Solver Network** | AMM中心のDEXに対する価格改善＋MEV保護が未実装 | なし |
| **エージェントパフォーマンス/SLA 監視** | エージェントが増えるほど「どれを信頼するか」が問題化 | なし |
| **Gasless Agent Payment SDK（汎用）** | AgentGuardはセッション制御。ガス代代払いSDKは存在しない | なし（AppKit Gas Bankは開発中） |

### ○ 有望度中

| テーマ | 理由 | 現状 |
|---|---|---|
| **プライバシー保護型エージェント実行** | TEE/zkを活用した機密エージェント | Cocoon (TEE) が参入 |
| **クロスプラットフォーム Agent ID** | AWikiがW3C DID/TON DNS標準を試みているが初期 | AWiki |
| **Telegram Stars経済圏** | Stars↔TON/USDTスワップ、Stars担保ローン等 | Stars Skillが入口のみ |
| **AI-driven価格予測** | 価格データは揃った。予測モデルは未開拓 | なし |
| **RWA on TON** | Telegramユーザー（新興国）への低コスト信用アクセス | なし |
| **Yield-bearing Stablecoin on TON** | USDe/USDY類の20% APYが始まったがDeFi統合は初期 | なし |

---

## 5. ビルダー速度・コミュニティ成熟度の分析

### Fast Grants (Feb) → AI Hackathon (Mar) の変化

| 観点 | Fast Grants | AI Hackathon | 変化 |
|---|---|---|---|
| 参加数 | 40件 | 160件 | 4倍（但し品質差あり） |
| Average claps | 〜8 | 〜15 | 上昇傾向 |
| Winnerの技術水準 | プロトタイプ〜MVP | MVP〜アーリープロダクト | 成熟 |
| 継続参加 | — | IntelliSense, TON Agent Platformが連続参加 | 高速学習 |
| MCP覇権 | Fast Grants: 不在（OpenClawが先行） | AI Hackathon: Telegram MCP (94) が台頭 | MCPが標準化 |

**1ヶ月でビルダーコミュニティが顕著に成長。次のハッカソンでは「応用層」「垂直統合」が差別化要因になる。**

---

## 6. 総括と戦略的示唆

### 現在の主流（2026 Q1）

| 優先度 | テーマ | 状況 |
|---|---|---|
| ★★★ | MCP/SDK レイヤー整備 | すでにレッドオーシャン |
| ★★★ | x402 M2M決済 | 増加中、差別化が重要 |
| ★★★ | Telegram Gifts エコノミー | 独自発展、上位レイヤーに余白 |
| ★★ | セキュリティ・信頼スコア | 同質化、アクション統合が次の差別化 |
| ★★ | ノーコードプラットフォーム | SpawnDockが優位 |
| ★★ | A2A / エージェントID | 初期、標準化競争が始まる |

### ポジショニング示唆

- **SDK/MCPレイヤーは参入遅延** — 垂直統合（特定ドメイン深掘り）が必要
- **Gifts × DeFi × AI** が2026年前半のTONで最もホットな応用領域
- **A2A経済圏の標準化** — まだ勝者なし。TON × Telegram ID の組み合わせに優位性
- **「すぐ動くもの」** — SpawnDockの圧勝が示すようにDX・時間ゼロ摩擦が最重要

---

*関連: [hackathon-projects.md](hackathon-projects.md) — 全プロジェクト生データ*
*関連: [ton-roadmap-products.md](ton-roadmap-products.md) — 公式ロードマップとの対照*
*作成: 2026-03-28*
