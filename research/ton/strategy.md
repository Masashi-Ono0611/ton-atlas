# TON Foundation 戦略ポジション・公式メッセージ分析

> データソース: ton.org ニュースルーム, X(@ton_blockchain), Telegram公式
> 調査日: 2026-03-28

---

## 1. TON Foundation CEOによる戦略宣言

### "The Agent Economy Has a Billion-User Head Start. It's Called Telegram."
**2026-03-25 | Max Crown, President & CEO of TON Foundation**
> URL: https://ton.org/en/the_agent_economy_inside_telegram

これはTON Foundationの戦略的ポジショニングを最も明確に示した文章。重要発言を分析する。

#### 「Mastercardの$18億買収は敗北宣言だ」

Mastercardがステーブルコインスタートアップを$18億で買収したことを引き合いに、既存金融インフラが防衛戦に入ったと断言。TONは「勝つ必要がない、すでに答えだから」という逆説的な強さのポジションを主張。

#### 「ウォレットの所在を誰も言っていなかった」

Agent Economy各プレイヤーの問題認識:
- x402（ステーブルコイン）: 決済プロトコルを定義したが、ウォレットは別途
- Stripe Machine Payments Protocol: マイクロペイメントを定義したが、ウォレットは別途
- Google Agent Payments Protocol: 60+パートナーを集めたが、ウォレットは別途
- **全員が「ウォレットは既に持っている前提」で設計している**

TONの主張: TelegramはウォレットをすでにMAU 10億人に対して提供済み。

#### McKinseyの$3〜5兆予測の文脈化

「2030年のエージェントコマース市場$3〜5兆」という予測を前提に、
> 「そのインターネットの層（messaging + payments + users）を誰が持っているか？」

という問いを立て、TON/Telegramが唯一の自然な答えと位置付ける。

---

## 2. 国際送金・新興国決済戦略

### "Modernising the Rails of Global Money Movement" (Part 1 & 2)
**2026-03-26/27 | TON Foundation**
> URL: https://ton.org/en/modernising_the_rails_of_global_money_movement

#### 問題設定

- 国際送金コスト: $200送金で6.49%（2025年3月、世界平均）
- SWIFTとコルレス銀行の多重中継
- 為替スプレッドと処理時間（T+2〜T+5）

#### TONの戦略（stablecoin sandwich モデル）

```
[送金元・フィアット] → [TON/stablecoin変換] → [即時ステーブルコイン移動] → [受取側フィアット変換]
```

既存Visa/SWIFTを「置き換える」のではなく「補完する」立場を明示。

#### 新興国市場での自然な採用例

- Telegram はアフリカ・東南アジア・中東での「ビジネス会話」に使われている
- USDTでの請求書払い・P2P送金がすでに自然発生
- Tether: 2025 Q4に**3,500万人の新規ユーザー純増**（主に新興国）

#### 中期ターゲット

**新興国 × クロスボーダー決済 × Telegram-native** がTONの主戦場。
Solanaの「高頻度AI取引」、Ethereumの「機関投資家」とは異なる市場セグメント。

---

## 3. X (@ton_blockchain) の主要発信（2026年）

| 内容 | 時期 | 重要度 |
|---|---|---|
| Max Crown の "Agent Economy" 宣言 | Mar 2026 | ★★★ 戦略宣言 |
| Gateway 2026 (5/1-2, Dubai, 1,500人) 発表 | Mar 2026 | ★★★ |
| Sequoia / Ribbit / Benchmark 等 米大手VCが$4億+投資 | Mar 2026 | ★★★ 資金調達 |
| Coinbase Ventures が Toncoin 保有開始 | 2026 | ★★ 機関参入 |
| ZodiaCustody, Kiln, CopperHQ, CoinShares ETP が参入 | Jan 2026 | ★★ |
| USDe TON DeFiで最大20% APY | 2026 | ★★ DeFi利率 |
| TON Wallet に BTC/ETH 直接サポート（MoonPay統合） | Feb 2026 | ★★ |

---

## 4. Gateway 2026（次の主要マイルストーン）

| 項目 | 内容 |
|---|---|
| 日時 | 2026年5月1-2日 |
| 場所 | Dubai |
| 規模 | 1,500人（2024年比2倍） |
| 新設ステージ | Innovation Lab（エンジニアリング深掘り）、Community Stage |

**Gateway 2026で予想される主要発表:**
1. **AgenticKit 正式発表・デモ** — Q1ローンチ予定だが詳細未公開のため
2. **TON Pay 2.0 プレビュー** — マーチャント向け新機能
3. **AppKit 正式版（MCP + Embedded Wallet + Gas Bank）** — 完成系のデモ
4. **パートナーシップ発表** — 機関・VC・プラットフォームとの新提携
5. **コンセンサスアップグレード詳細** — New TON Consensus の技術仕様開示

---

## 5. 公式の3つの戦略ピラー

```
ピラー1: Agent Economy の「インフラ」になる
├── AgenticKit (AIエージェント向けKit)
├── AppKit MCP対応 (エージェント開発DX)
├── Open Wallet Standard (MoonPay, 8チェーン共通)
└── "ウォレットの所在問題" の解決

ピラー2: 決済レールの近代化（新興国・クロスボーダー）
├── TON Pay 1.0→2.0
├── Gasless Transactions (ユーザー摩擦ゼロ)
├── クリプトリンクカード (2026年中)
└── Telegram × 新興国 × ステーブルコイン

ピラー3: 機関投資家 × Telegram 10億ユーザーの交差点
├── Sequoia/Coinbase Ventures の参入
├── CoinShares ETP (機関アクセス)
└── McKinsey $3-5兆市場予測の文脈化
```

---

## 6. 公式発信 × ハッカソン アライメント分析

| 公式が強調するテーマ | ハッカソンで既に開発中 | アライメント |
|---|---|---|
| AgenticKit | TON OpenClaw, ton-tools, Teleton, TON MCP | ◎ 高い |
| Agent Economy / A2A | TAMP, AWiki, Mesh Network, Catallaxy | ◎ 高い |
| x402 / M2M Payment | x402 SDK, TonPay 402, ton402 | ◎ 高い |
| TON Pay / ガスレス決済 | Forlarge, SpawnDock Gas Bank, Jarvis | ○ 中程度 |
| Open Wallet Standard | AgentGuard, MyTonWallet AI | ○ 中程度 |
| 新興国 × ステーブルコイン | PanoramaBlock（マルチチェーン） | △ 間接的 |
| **Telegram Gifts 経済圏** | Giftstat, Morgan Gift, Giftindex, Gifti | ★ **公式未言及だがビルダー先行** |

### 重要観察: Gifts経済はコミュニティ先行

TON公式はTelegram Giftsに関してほぼ言及していない。
しかし両ハッカソン合計で6〜7プロジェクトがGiftsに特化。
→ **ボトムアップの独自市場形成** であり、公式ロードマップが追いかける可能性がある。

---

## 7. 公式チャンネル

| チャンネル | URL | 用途 |
|---|---|---|
| X公式 | https://x.com/ton_blockchain | メインアナウンス |
| Telegram公式 | https://t.me/toncoin | コミュニティ |
| ブログ | https://ton.org/en/newsroom | 詳細記事 |
| ロードマップ | https://ton.org/en/roadmap | 開発状況 |
| ドキュメント | https://docs.ton.org | 技術仕様 |

---

*関連: [ton-roadmap-products.md](ton-roadmap-products.md) — プロダクト詳細（AgenticKit・AppKit・TON Pay）*
*関連: [ton-vs-ecosystem.md](ton-vs-ecosystem.md) — 他チェーンとの比較ポジション*
*作成: 2026-03-28*
