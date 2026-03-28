# アイデア1: AgentPay — TON AIエージェント向けGasless決済SDK

> 作成日: 2026-03-28
> 優先度: ★★★（最優先）
> カテゴリ: Infrastructure / Payment

---

## 問題

### ユーザー体験の根本的な欠陥

TON/TelegramでAIエージェントを使う際の現実:

```
ユーザー: 「このJettonをUSDTに交換して」
エージェント: 「まずTONをウォレットに入金してください（ガス代として）」
ユーザー: ???（CEXでTONを買って → ウォレットに送金 → ...）
```

Web2ユーザー（Telegram 10億MAUの大半）にとって「ガス代のためにTONを持つ」というステップが致命的な離脱ポイント。

### 開発者の問題

AIエージェントを作る開発者も同じ問題に直面する:

```
エージェントが自律的にtxを実行するには:
1. エージェントが秘密鍵を管理するウォレットを持つ
2. そのウォレットにTONを補充し続ける
3. ガス代の変動に対応する仕組みが必要
4. 高頻度操作（スワップ・送金等）でTONが枯渇する問題
```

### 現状の部分的解決策とギャップ

| 解決策 | 限界 |
|---|---|
| AppKit Gas Bank（TON Foundation Q2予定） | 公式だが開発者が独立した制御を持てない |
| AgentGuard（ハッカソン） | セッション制御（上限・ホワイトリスト）だが、ガス代代払いは別問題 |
| TonPay 402 | x402決済に特化。汎用的なGasless SDKではない |

**汎用的な「エージェントがユーザーに代わってガス代を払う」SDKが存在しない。**

---

## ソリューション: AgentPay

### コアコンセプト

```
Sponsored Transaction モデル（Flashbotsから知見を転用）:

ETH版（実装済み）:
Bundle = [sponsor(開発者) → executor(エージェント) ETH]
       + [executor → target ERC20]
→ 同一ブロックに原子的インクルード

TON版 AgentPay:
Message Chain = [sponsor → executor TON（ガス代）]
              + [executor → DeFiプロトコル（Jetton操作等）]
→ actor model対応の非同期原子的実行
```

### アーキテクチャ

```
┌─────────────────────────────────────────────────┐
│                   AgentPay SDK                   │
├─────────────────────────────────────────────────┤
│  Developer Layer                                  │
│  ┌─────────────────────────────────────────────┐│
│  │ GasPolicy: 支出上限・ホワイトリスト・有効期限 ││
│  │ SponsorVault: TON poolの管理・補充           ││
│  │ SessionKey: 時限付きエージェント実行権限     ││
│  └─────────────────────────────────────────────┘│
├─────────────────────────────────────────────────┤
│  Protocol Layer                                   │
│  ┌─────────────────────────────────────────────┐│
│  │ SponsoredMessage: TON特有の非同期bundle設計 ││
│  │ AtomicSequence: メッセージチェーンの原子性   ││
│  │ FeeEstimator: 動的ガス見積もり              ││
│  └─────────────────────────────────────────────┘│
├─────────────────────────────────────────────────┤
│  Integration Layer                                │
│  ┌─────────────────────────────────────────────┐│
│  │ TON AgenticKit connector                    ││
│  │ TON Connect 拡張                            ││
│  │ DeDust / STONfi / Evaa SDK                  ││
│  └─────────────────────────────────────────────┘│
└─────────────────────────────────────────────────┘
```

### 開発者体験（DX）イメージ

```typescript
import { AgentPay } from '@agentpay/ton-sdk';

// 開発者がSponsor Vaultを設定
const agentPay = new AgentPay({
  sponsorPrivateKey: process.env.SPONSOR_KEY,
  gasPolicy: {
    maxTONPerSession: 0.1,          // セッションあたり最大0.1 TON
    maxTONPerTx: 0.01,              // tx1件あたり最大0.01 TON
    whitelist: ['STONfi', 'DeDust'], // 許可プロトコル
    expiry: 3600,                    // 1時間
  }
});

// エージェントがユーザーの代わりにスワップ（ガスゼロ）
const result = await agentPay.sponsoredExecute({
  userAddress: user.tonAddress,
  action: {
    type: 'swap',
    protocol: 'STONfi',
    fromToken: 'USDT',
    toToken: 'TON',
    amount: '100',
  }
});
// → ユーザーはTONを持っていなくてOK
// → 開発者のSponsor VaultからTONが自動補充
```

---

## 差別化・競合優位

| 観点 | AgentPay | AppKit Gas Bank | AgentGuard |
|---|---|---|---|
| 汎用性 | 任意のDeFi/tx操作に対応 | AppKitエコシステムのみ | セッション制御のみ |
| 独立性 | 完全に開発者管理 | TON Foundation依存 | オンチェーンのみ |
| セキュリティ | GasPolicy + ホワイトリスト | 非公開 | 時限・上限・ホワイトリスト |
| AIエージェント統合 | TON AgenticKit対応 | 予定 | 非対応 |
| x402連携 | ネイティブ対応 | 未定 | 非対応 |

### 独自の強み

**既存研究資産からの直接展開:**
- `searcher-sponsored-tx` のsponsored bundleパターンをTONに移植
- ETHでの検証済み原子性設計ロジックが知的基盤
- MEVサーチャーの視点 = 「最小コストで最大確実性のtx実行」 = Gasless SDK設計と同じ問題

---

## 市場・収益モデル

### ターゲットユーザー

| セグメント | 使い方 | 支払意欲 |
|---|---|---|
| TMA開発者 | ユーザーのガス代を肩代わり | 月額SaaS / 従量課金 |
| AIエージェント開発者 | エージェントの自律操作 | 従量課金 |
| TON DeFiプロトコル | ユーザーオンボーディング改善 | 月額 |
| 企業（Telegram上のビジネス） | 自動決済・送金ワークフロー | エンタープライズ |

### 収益モデル

```
1. SaaS月額（開発者向け）
   - Starter: $99/月 (1,000 sponsored tx)
   - Pro: $499/月 (10,000 sponsored tx + アナリティクス)
   - Enterprise: カスタム

2. 従量課金（スケールアウト）
   - $0.05 / sponsored transaction
   - 実際のガス代を上乗せして請求

3. プロトコル統合料（DeFiプロトコル向け）
   - DeDust/STONfi等のDefi protocol が自分のユーザーのガス代をAgentPay経由で支払う
```

---

## 技術実装ロードマップ

| フェーズ | 期間 | 内容 |
|---|---|---|
| Phase 1 | 0〜6週 | TON Sponsored Tx プロトコル設計 + PoC |
| Phase 2 | 6〜12週 | SDK実装（TypeScript）+ DeDust/STONfi統合 |
| Phase 3 | 12〜18週 | GasPolicy UI + 開発者ポータル |
| Phase 4 | 18〜24週 | TON AgenticKit連携 + x402統合 |

---

## リスク

| リスク | 対策 |
|---|---|
| AppKit Gas BankがAgentPayを代替 | 公式より先にリリース + より汎用的なAPIを提供 |
| TONのactor model非同期性による原子性保証困難 | 失敗時のロールバック設計 + 小口での信頼性優先 |
| Sponsor Vaultの管理負担 | 自動補充のAlerta + 残高モニタリングUI |

---

*関連: [mev-ton-opportunity.md](../research/mev-ton-opportunity.md)*
*関連: [ton-roadmap-products.md](../research/ton-roadmap-products.md) — AppKit Gas Bankとの比較*
