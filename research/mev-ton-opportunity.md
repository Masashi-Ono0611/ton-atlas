# TON MEV 機会分析 + 既存研究資産の活用

> 調査日: 2026-03-28
> 参照コード: `/Users/masashi_mac_ssd/Developer/mev/searcher-sponsored-tx/`

---

## 1. 既存MEV研究資産の棚卸し

### `searcher-sponsored-tx` (Flashbots)

**実装されているパターン:**

```typescript
// Bundle = 原子的に同一ブロックへインクルードされる tx 群
const bundle = [
  {
    // tx1: Sponsor → Executor (ETH送金)
    transaction: {
      to: executorWallet.address,
      value: GWEI.mul(31).mul(21000), // ガス代相当
      gasPrice: GWEI.mul(0),          // priority fee のみ
      maxPriorityFeePerGas: GWEI.mul(31)
    },
    signer: sponsorWallet
  },
  {
    // tx2: Executor → Target (ERC20 転送)
    transaction: {
      to: TOKEN_ADDRESS,
      data: encodeTransfer(targetAddress, amount),
      gasPrice: GWEI.mul(0),
      maxPriorityFeePerGas: GWEI.mul(31)
    },
    signer: executorWallet
  }
]
// → Flashbots relay に送信 → Builder がブロックに組み込む
```

**保有知識レベル:**

| 知識領域 | 深さ |
|---|---|
| Flashbots relay architecture (MEV-Boost) | ★★★ 実装済み |
| Bundle submission & bidding mechanics | ★★★ 実装済み |
| Post-EIP-1559 priority fee戦略 | ★★★ `GWEI.mul(31)` 実装済み |
| Builder selection logic | ★★★ 実装済み |
| Atomic bundle（2tx同一ブロック）原理 | ★★★ 実装済み |
| Sponsored tx pattern（gasless rescue） | ★★★ 実装済み |
| Sepolia testnet 検証経験 | ★★ 実績あり |
| SUAVE / ePBS / 最新MEVアーキテクチャ | ★★ 研究レベル |
| Jito (Solana) MEV | ★ 調査レベル |
| TON-specific MEV | ★ 未着手 |

---

## 2. TON のアーキテクチャ特性とMEVへの影響

### TON vs Ethereum: 根本的な違い

| 特性 | Ethereum (EVM) | TON (Actor Model) |
|---|---|---|
| 実行モデル | 同期的（グローバル状態マシン） | 非同期メッセージパッシング（Actor） |
| 状態モデル | グローバルステート（全アカウント共有） | ローカルステート（各コントラクトが独立） |
| Mempool | 公開mempool（誰でも閲覧可能） | **メッセージキュー（公開mempoolなし）** |
| ブロック構造 | 単一チェーン | Masterchain + Workchains（シャーディング） |
| MEVインフラ | Flashbots/MEV-Boost（成熟） | **ほぼ存在しない** |

### TONのメッセージパッシング: 詳細

```
TONのactor model:
1. Wallet → Jetton Master（Transfer message）
2. Jetton Master → Jetton Wallet A（Internal Transfer message）
3. Jetton Wallet A → Jetton Wallet B（Transfer Notification）
4. Jetton Wallet B → 受信確認

→ 各メッセージは非同期に処理
→ 1つのユーザー操作が複数のブロックにまたがることがある
→ 「完了したかどうか」はチェーンをトレースしないと分からない
```

### これがMEVにとって何を意味するか

**ETHのように「mempoolを見て先回り」という手法は使えない**

しかし新しいMEV機会が存在する:

1. **Cross-shard price arbitrage**: Workchain間でのJetton価格差
2. **Message ordering opportunity**: バリデータがメッセージ処理順序を影響できる可能性
3. **Liquidation racing**: Evaa等のレンディングプロトコルの担保清算
4. **LP pool rebalancing**: DeDust/STONfiのプールリバランス時の抽出

---

## 3. TON MEV機会の詳細分析

### 機会1: クロスシャードアービトラージ

```
シナリオ:
- Workchain 0 の STONfi: TON/USDT = $2.80
- Workchain -1 の DeDust: TON/USDT = $2.83

アービトラージ:
1. Workchain 0で安い側からTONを買う
2. Workchain -1で高い側にTONを売る
3. 差額: $0.03 × 取引量 が利益

課題:
- シャード間メッセージは非同期 → 原子性が保証されない
- 価格変動リスク: メッセージが届く前に価格が収束する可能性
```

### 機会2: 清算MEV（Liquidation Racing）

```
シナリオ (Evaa Protocol):
- ユーザーが TON を担保に USDT を借入
- TON価格が下落 → 担保比率が清算閾値を下回る
- 清算ボットが「清算メッセージ」を先に送れる →清算ボーナスを獲得

TONの特性:
- 清算可能なポジションはオンチェーンで公開されている
- 複数のボットが同時に清算メッセージを送る競争が発生
- 現状: 小規模な競争のみ（TVL $0.5B程度のため）
- 将来: TON DeFiが成長するほど清算MEVの絶対額も拡大
```

### 機会3: フロントランニング（TON式）

```
ETH式: mempoolを監視して先回り
TON式: バリデータが「どのメッセージを先に処理するか」を選択

→ バリデータとの関係構築がETHのBuilder連携と同等の意味を持つ
→ TONバリデータへのtip仕組み（ETHのpriority fee相当）の設計が鍵
```

### 機会4: Telegram Bot MEV

```
シナリオ:
- Telegram Mini App内でのDEXスワップが増加
- ユーザーのスワップ意図が特定のTMA UIから送信される
- UIを提供する側（TMAオペレーター）がスワップを見てインテントを先読み

これはEthereumのWallet OFA（Order Flow Auction）と同等の発想:
→ TMA内注文フローの優先ルーティング
→ ユーザーへのリベート（スワップ改善）が可能
```

---

## 4. 既存研究資産 → TON応用マップ

### 🟢 即活用可能（High Leverage）

#### ① AIエージェント向け Sponsored Transaction SDK for TON

**ETHでの実装経験 → TONへの直接応用**

```
ETH Sponsored Tx（実装済み）:
Bundle = [sponsor → executor ETH] + [executor → target ERC20]

TON版 Sponsored Tx（設計可能）:
Bundle = [sponsor → executor TON] + [executor → target Jetton]

TONのactor model:
- 2つのメッセージを同一バリデータへのキューに入れる
- ETHほど厳密な「同一ブロック原子性」はないが近似可能
```

**活用場面:**
- AIエージェントがユーザーの代わりにDeFi操作（ユーザーはTONゼロ）
- Telegram Mini AppでのGasless UX
- 企業・開発者がガス代を肩代わりするSDK

**実装難易度:** ★★★（actor modelの理解が必要だが、bundsle概念は転用可能）

#### ② TON Liquidation Bot

**ETHのflashloan + liquidation知識 → TONへ**

```
Evaa Protocol (TON lending):
- 公開清算閾値をモニタリング
- TON価格 oracle を監視
- 閾値以下になったら即座に清算tx送信
- 清算ボーナス（5〜8%）が利益
```

**実装難易度:** ★★（清算ロジックはETHと同様、TON API習熟が必要）

#### ③ DeDust/STONfi アービトラージ Bot

**ETHのarb知識 → TON AMM価格差arb**

```
対象: DeDust と STONfi 間の同一ペア価格差
仕組: 安い方で買い、高い方で売る（複数ホップ可）
課題: 非同期メッセージで原子性が弱い → 部分失敗リスクがある
対策: 小口 + 高頻度でリスクを分散
```

**実装難易度:** ★★（TON APIとDEX SDKの習熟が必要）

---

### 🟡 中期的に活用可能（Medium Leverage）

#### ④ TON MEVインフラのOSS化

**Flashbots がEthereumで果たした役割をTONで担う**

```
Flashbots のTON版:
- バリデータへのtip仕組みの設計
- Bundle提出インターフェースの仕様策定
- MEVシェア（ユーザーへのリベート）の実装
- ドキュメント・OSSとしての公開

学術/エコシステム価値:
- TON MEVの構造的分析論文
- Jito (Solana) vs TON MEV比較研究
- TON Foundation, hacketonコミュニティへのスポンサー価値
```

**実装難易度:** ★★★★（TONバリデータへのアクセス・仕様策定が必要）

#### ⑤ SUAVE SDK活用

**既存bundle mechanics知識 → SUAVEでのクロスドメインMEV**

- SUAVE Go/Rust SDKの習熟
- ETH + Base のクロスロールアップMEV実装
- Programmable privacy MEVのプロトタイプ

---

### 🔴 差別化困難（Low Leverage）

#### ⑥ Ethereum arb bot

- AI-on-AI競争で参入障壁が極めて高い（数百万ドルの研究開発費）
- beaverbuild + Titanの90%で小規模サーチャーの収益機会が縮小
- **推奨: 参入しない**

---

## 5. 優先度マトリクス

```
                    競合密度
                  低              高
スキル    ┌────────────────┬────────────────┐
活用度    │                │                │
          │  ★★★          │  ★★            │
高        │ TON Sponsored  │ TON-ETH        │
          │ Tx SDK         │ クロスチェーン  │
          │ TON Liq Bot    │ MEV研究         │
          ├────────────────┼────────────────┤
          │  ★★            │  ★             │
低        │ TON MEV OSS   │ ETH Arb Bot    │
          │ SUAVE実装      │                │
          └────────────────┴────────────────┘
```

---

## 6. 推奨アクションプラン

### Phase 1（0〜3ヶ月）: TON環境構築 + Quick Win

1. **TON SDK習熟**: `@ton/ton` TypeScript SDK でのメッセージ構築
2. **DeDust / STONfi アービトラージbot**: 小口でのリスク検証
3. **Evaa清算監視bot**: 清算機会のモニタリング（実行前に実績確認）

### Phase 2（3〜6ヶ月）: Sponsored Tx SDK

1. **TON Sponsored Transaction プロトコル設計**: actor model対応のbundle設計
2. **AIエージェント統合**: TON AgenticKit × Sponsored Tx SDK
3. **Gasless Telegram Mini App デモ**: ユーザーデモでPMF検証

### Phase 3（6〜12ヶ月）: エコシステム貢献

1. **TON MEV研究レポート**: TON Foundationへの公式提出
2. **OSS公開**: Flashbots相当のTON MEVインフラをOSSで
3. **コミュニティポジション確立**: ハッカソン・Gateway 2026等での発信

---

*関連: [mev-landscape.md](mev-landscape.md) — グローバルMEV動向*
*関連: [defi-trends.md](defi-trends.md) — DeFiトレンド（清算・ソルバー機会）*
*関連: [ideation/01-gasless-agent-payment.md](../ideation/01-gasless-agent-payment.md)*
*関連: [ideation/04-ton-mev-infra.md](../ideation/04-ton-mev-infra.md)*
*作成: 2026-03-28*
