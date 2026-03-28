# アイデア5: KYA on TON — Telegram IDを活用した「Know Your Agent」プリミティブ

> 作成日: 2026-03-28
> 優先度: ★★
> カテゴリ: Identity / Infrastructure

---

## 問題

### Agent Economyのアイデンティティ危機

AIエージェントが増殖するにつれ、「このエージェントは誰が背後にいて、信頼できるか？」という問いが最重要プリミティブになる（a16z KYA予測）。

**現状の問題:**

```
エージェントが増えると:
- どのエージェントが悪意を持っているか分からない
- エージェントに騙された時に誰を訴えるか不明
- エージェント同士のA2A取引でデフォルトリスク

現在の「解決策」とその限界:
- ERC-8004 (ETH): EVM専用、TONユーザーには届かない
- World ID: Orb（虹彩スキャン端末）が必要、インフラコストが高い
- TAMP/AWiki (TON): 開発中、標準化されていない
```

### World IDのボトルネック

Sam AltmanのWorld（旧Worldcoin）は最も前進したKYAソリューションだが:

```
World ID のフロー:
1. 最寄りのOrb（虹彩スキャン端末）まで行く
2. 虹彩スキャン → ZKP生成
3. World IDを取得
4. エージェントに委任

問題:
- Orbは世界の主要都市にしか設置されていない
- 新興国・農村ではアクセス不可
- TON/Telegramユーザーの多い新興国でのボトルネック
```

---

## ソリューション: KYA on TON

### コアコンセプト

**Telegramの既存認証インフラを使った軽量KYA**

```
Telegramが持っているもの:
1. 電話番号認証（SIM + SMS OTP）
2. アカウントの利用履歴（アクティビティスコア）
3. Telegramアカウント ↔ TON Wallet の対応関係
4. Telegram Passportによる身分証明

これらを使えば:
- Orb不要でKYAプリミティブを実現
- Telegram 10億ユーザーが既に認証済み
- 新興国でも完全にアクセス可能
```

### アーキテクチャ

```
KYA on TON プロトコル

┌─────────────────────────────────────────┐
│ 人間（プリンシパル）                     │
│ ├── Telegram account（電話番号認証済み） │
│ └── TON Wallet（TON Connect連携）        │
└────────────────────┬────────────────────┘
                     │ Delegation
┌────────────────────▼────────────────────┐
│ AgentID Registry（TON スマートコントラクト）│
│                                          │
│ 保存内容:                                │
│ ├── Telegram account hash（プライバシー保護）│
│ ├── TON Wallet address                  │
│ ├── エージェントMCPエンドポイント          │
│ ├── 委任スコープ（何を許可するか）         │
│ ├── 有効期限                             │
│ └── Reputation score                    │
└──────────────────────────────────────────┘
                     │ Verify
┌────────────────────▼────────────────────┐
│ 他エージェント・プロトコル                │
│ 「このエージェントは認証済み人間が背後？」 │
│ → AgentID Registry で確認 → Yes/No      │
└──────────────────────────────────────────┘
```

### プライバシー保護設計

```
Telegramアカウントを直接公開せず:

Step 1: Telegramアカウントのハッシュを生成
  hash = ZKP(telegram_user_id + salt)
  → telegram_user_id は分からないが、同一人物かは確認できる

Step 2: 「1人1エージェント上限」の証明
  → ZKPで「このhashは未使用」を証明（World IDと同じアプローチ）

Step 3: エージェント委任
  → 人間の署名（Telegram Wallet経由）でエージェントに権限委任
```

---

## World ID vs KYA on TON

| 観点 | World ID | KYA on TON |
|---|---|---|
| 認証方法 | 虹彩スキャン（Orb） | Telegram電話番号 + SIM |
| インフラ | Orb端末（物理設備が必要） | Telegramアプリ（既存） |
| 新興国アクセス | 制限あり（Orb設置都市のみ） | 完全アクセス（SIMがあれば） |
| プライバシー | ZKP（虹彩hash） | ZKP（Telegram ID hash） |
| エコシステム | Coinbase AgentKit連携 | TON/Telegram native |
| 認証強度 | 高（生体認証） | 中（電話番号認証） |
| コスト | Orb設置・運営コスト | ほぼゼロ（Telegram既存インフラ） |

**トレードオフ:**
- World IDの方が認証強度は高い（生体認証 > 電話番号）
- KYA on TONはアクセス民主性と低コストで優れる

**TONの戦略的ポジション:**
新興国・大衆市場では「使えること」が最優先。
完璧な認証より、誰でもアクセスできる認証の方が価値がある。

---

## ERC-8004との相互運用

TON-native KYAと同時に、ERC-8004との互換性を設計する。

```
クロスチェーンエージェントID:

TON AgentID ──bridge──▶ ETH ERC-8004 Registry
                        ← verify ←

TONで認証したエージェントがETH DeFiでも使える
ETHで認証したエージェントがTON DeFiでも使える
```

**実装:**
- TON → ETH: TON AGentID のERC-8004 compatible JSON出力
- ETH → TON: ERC-8004のTON-Agent Registry へのミラーリング

---

## ユースケース

### ① A2A（エージェント間）取引の信頼基盤

```
エージェントAが仕事を外注:
1. A2Aマーケット（TAMP等）でエージェントBを探す
2. エージェントBのKYA on TONを確認
   → 「このエージェントは認証済み人間が背後にいる」
3. 仕事を依頼、TON Payで支払い
4. 問題が起きたら人間（Telegram ID）を追跡可能
```

### ② DeFiへのアクセス制御

```
KYCが必要なDeFiプロトコル（Ondo USDY等）:
- 普通: 全ユーザーがKYC書類提出
- KYA on TON: Telegram認証で軽量KYC（規制対応が必要な場合は追加フロー）
```

### ③ エージェントレピュテーション

```
エージェントが仕事を完了するたびにスコアが蓄積:
- 完了率: 99%
- 平均完了時間: 5分
- 評価スコア: 4.8/5
- 総決済額: 1,000 USDT

→ レピュテーションが担保として機能
→ 高スコアエージェントは低担保でA2A信用取引が可能
```

---

## 実装ロードマップ

| フェーズ | 期間 | 内容 |
|---|---|---|
| Phase 1 | 0〜8週 | TONスマートコントラクト設計（AgentID Registry） |
| Phase 2 | 8〜16週 | Telegram Widget認証 + ZKP生成 |
| Phase 3 | 16〜24週 | Reputation Registry + A2Aマーケット統合 |
| Phase 4 | 24週〜 | ERC-8004相互運用 + クロスチェーン展開 |

---

## 収益モデル

| 収益源 | 詳細 |
|---|---|
| AgentID 登録料 | エージェント1体のID登録: 1 TON（年間更新） |
| Reputation API | エージェントスコアのAPIアクセス: 従量課金 |
| KYA検証料 | DeFiプロトコルがKYA確認する際の手数料 |
| Enterprise | 機関向けのコンプライアンス対応モジュール |

---

## リスク

| リスク | 対策 |
|---|---|
| Telegramが電話番号認証を制限・変更 | TON DNSとの組み合わせで代替認証レイヤーを構築 |
| SIMスワップ攻撃（電話番号認証の弱点） | アカウント年齢・活動スコアで補完（古いアカウントほど信頼） |
| TON Foundationが公式実装する | 先行してOSSで標準を確立することで貢献者ポジションを確保 |
| プライバシー規制（GDPR等） | ZKPによるハッシュのみを保存し、IDは明示しない設計 |

---

*関連: [chain-ethereum-base.md](../research/chain-ethereum-base.md) — ERC-8004詳細*
*関連: [cross-chain-protocols.md](../research/cross-chain-protocols.md) — エージェントID標準比較*
*関連: [hackathon-trends.md](../research/hackathon-trends.md) — TAMP/AWikiの先行実装*
