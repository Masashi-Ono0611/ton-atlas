# Ideation — プロダクトアイデア一覧

> 作成日: 2026-03-28
> ベース: research/ 全調査ドキュメントから導出

---

## アイデア一覧

| # | プロダクト名 | カテゴリ | 優先度 | コア価値 |
|---|---|---|---|---|
| 01 | [AgentPay](01-gasless-agent-payment.md) | Infrastructure / Payment | ★★★ | TON上のAIエージェント向けGasless決済SDK |
| 02 | [GiftFi](02-telegram-gifts-defi.md) | DeFi / NFT Finance | ★★★ | Telegram Gifts担保型レンディングプロトコル |
| 03 | [IntentLayer TON](03-ton-intent-solver.md) | DeFi / Protocol | ★★ | TON上のIntent-based DEX + Solverネットワーク |
| 04 | [TON MEV Infra](04-ton-mev-infra.md) | Infrastructure / Research | ★★ | TONのFlashbots — MEVインフラのOSS化 |
| 05 | [KYA on TON](05-kya-ton-identity.md) | Identity / Infrastructure | ★★ | Telegram IDを活用したKnow Your Agentプリミティブ |
| 06 | [TON Freedom Stack](06-freedom-stack.md) | Infrastructure / Social | ★★ | TON Proxy+Sites+Storage統合ブラウザ + Bluetooth Mesh決済 |

---

## アイデア選定の根拠

全て以下の条件を満たすアイデアのみ選定:

1. **TON固有の差別化がある** — 他チェーンで実現できない、またはTONが構造的に有利
2. **既存研究資産が活用できる** — MEV/Flashbotsの知識が直接転用可能
3. **ハッカソン競合が薄い** — 200件のプロジェクト調査でホワイトスペースを確認
4. **TON公式ロードマップとアライン** — AgenticKit/AppKit/TON Pay 2.0と補完関係

Freedom Stack (#06) は上記に加え「TON/Telegramの思想的DNA」という軸で選定。
Durov逮捕・bitchat viral・EU Chat Control — プラットフォームリスクへの需要が実証された。

---

## 各アイデアの差別化の根拠

### AgentPay（★★★ 最優先）

- **なぜ今**: AppKit Gas Bankより先に汎用SDKを出すことでデファクト化できる
- **技術的根拠**: searcher-sponsored-txのbundleパターンをTONに移植
- **競合**: なし（AppKit Gas Bankは2026Q2予定、AgentGuardはセッション制御のみ）
- **TON固有性**: Gasless UX × Telegram 10億MAU = 最大のオンランプ効果

### GiftFi（★★★ 最優先）

- **なぜ今**: Gifts市場が成熟し始め、上位レイヤー（担保・レンディング）がまだ空白
- **技術的根拠**: NFTレンディング（BendDAO型）のTON実装
- **競合**: なし（ハッカソン200件でGiftsレンディングは0件）
- **TON固有性**: Telegram Gifts = TONにしか存在しない資産クラス

### IntentLayer TON（★★）

- **なぜ今**: DeDust/STONfiのAMM中心のTON DEXにMEV保護とベストレートを提供
- **技術的根拠**: MEVサーチャーの最適経路探索 = Solverの最適経路探索
- **競合**: PanoramaBlock（マルチチェーン）、Esprito（単純NL注文）
- **TON固有性**: TON AMM間のCoW + actor modelでの非同期バッチ

### TON MEV Infra（★★）

- **なぜ今**: TON DeFi成長前にインフラを確立 = Flashbots/Jitoが証明したパターン
- **技術的根拠**: 既存のFlashbots知識が最大限活用できる
- **競合**: なし（ハッカソン200件でMEVに言及したプロジェクトが0件）
- **TON固有性**: actor model × バリデータtip設計という未踏の技術的チャレンジ

### KYA on TON（★★）

- **なぜ今**: a16z「2026年のKYAが最重要プリミティブ」、TONはエージェントID空白
- **技術的根拠**: ZKP + Telegram IDのハッシュで軽量KYA
- **競合**: TAMP/AWiki（開発中、標準化されていない）
- **TON固有性**: Telegram電話番号認証 = Orb不要のWorld ID代替

---

## ポートフォリオ視点での組み合わせ

```
AgentPay (ガス代解決)
    ↓ ユーザーがTONゼロでDeFiを使える
IntentLayer TON (ベストレート保証)
    ↓ ユーザーが最良価格で取引できる
KYA on TON (エージェントID)
    ↓ エージェントが信頼されて取引できる
GiftFi (Gifts流動化)
    ↓ TON固有資産がDeFiに統合される
TON MEV Infra (市場安定化)
    ↓ MEVがエコシステムに還元される

→ 「TON × AIエージェント × DeFi」の完全スタック

[別軸]
TON Freedom Stack
    ↓ TON Proxy/Sites/Storage × MeshPay
→ 「Can't be evil」のアーキテクチャを900M Telegramユーザーへ
```

---

## 次のステップ

1. **AgentPayとGiftFiのPoC実装** — 最優先で技術検証
2. **コミュニティフィードバック** — ハッカソン参加でMVPを検証
3. **TON Foundation連携** — AgenticKit/AppKit との統合検討
4. **Gateway 2026（5月）での発表** — 最大の露出機会

---

*ベース研究: [research/_index.md](../research/_index.md)*
