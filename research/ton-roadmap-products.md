# TON Foundation ロードマップ・プロダクト詳細

> データソース: ton.org/en/roadmap, ton.org/en/appkit-alpha-launch, ton.org/en/ton-pay-a-new-payments-layer
> 調査日: 2026-03-28

---

## 1. ロードマップ全体像（2026年前半）

### ✅ リリース済み

| プロダクト | リリース | 概要 |
|---|---|---|
| WalletKit | 2025 | ウォレット接続・残高・送受信の標準ライブラリ |
| AppKit Alpha | Feb 2026 | DeFi統合・ウォレット管理・React/TypeScript対応 |
| TON Pay 1.0 | 2025末 | TMA向け決済SDK。1秒決済、1セント以下コスト |
| Builders Portal 2.0 | 2026 Q1 | 開発者ポータル刷新 |

### 🚀 Q1 2026 ローンチ予定

| プロダクト | 概要 | 注目度 |
|---|---|---|
| **AgenticKit** | AIエージェント向けキット（詳細未公開） | ★★★ |
| **Rust Node v1** | TON Rustノード実装 | 技術基盤 |
| **TON Factory** | プレビルドモジュールによる高速プロジェクト立ち上げ | 開発者向け |

### 🏗️ Q2 2026 ローンチ予定

| プロダクト | 概要 | 注目度 |
|---|---|---|
| **TON Pay 2.0** | 決済レイヤー第2弾 | ★★★ |
| **AppKit 正式版** | MCP対応・Embedded Wallet・Gas Bank含む | ★★★ |
| Builders Portal 3.0 | ポータル第3弾 | — |
| Tolk Dev Tools | Tolkスマートコントラクト言語ツール | 技術基盤 |
| **New TON Consensus** | コンセンサスアルゴリズム更新 | 技術基盤 |

---

## 2. AgenticKit（Q1 2026 ローンチ予定）

### 公式が開示している情報

TON公式ロードマップに「AIエージェント向けキット」として明記されているが、詳細は非公開。
Gateway 2026（5月1-2日、ドバイ）での正式発表が濃厚。

### 推測されるコンポーネント（ロードマップ文脈から）

| 推測コンポーネント | 根拠 |
|---|---|
| エージェント向けウォレットプリミティブ | "ウォレットの所在問題"（Max Crown記事）への解決策 |
| MCP サーバースキャフォールド | AppKit正式版のMCP対応と統合 |
| TON Connect エージェント拡張 | TON Connect 3.0との統合可能性 |
| TON Pay 統合 | M2M決済レールとしての位置付け |
| セッション鍵管理 | AgentGuardが先行実装している時限・支出制限 |

### elizaOS との比較

| 観点 | elizaOS (Solana) | AgenticKit (TON/予測) |
|---|---|---|
| アプローチ | TypeScript, 200+プラグイン, OSS | SDK形式、TON Foundation公式 |
| 実行環境 | 汎用（X/Discord/Telegram/Chain） | Telegram-native に特化 |
| ウォレット統合 | マルチチェーン対応 | TON/Telegram Wallet中心 |
| コミュニティ | 1,350+コントリビューター | 公式バックアップ + ハッカソンエコシステム |
| 決済 | x402/Solana Pay | TON Pay + x402 |
| アイデンティティ | 汎用 | Telegram ID 統合の可能性 |

---

## 3. AppKit（Alpha → 正式版）

### 現在のAlpha機能（2026-02-19 リリース）

| 機能カテゴリ | 詳細 |
|---|---|
| Wallet management | 接続・残高・送金（TON/Jetton/USDT） |
| DeFi protocol integration | 統一スワップ/ステーキングインターフェース |
| 技術スタック | TypeScript + React + TON Connect |

### 正式版（Q2 2026）追加予定機能

| 機能 | 概要 | 意義 |
|---|---|---|
| **On-Ramp Integration** | アプリ内でカード購入 | Web2ユーザーの入口 |
| **Cross-Chain On-Ramp** | Solana USDCから入金 | 他チェーンからの流入 |
| **Gas Bank** | 開発者がガス代を肩代わり | Gasless UXの実現 |
| **Intent Transactions** | ウォレット接続+署名を1アクションに | UXの極限簡素化 |
| **Embedded Wallets** | アプリ内でウォレット完結 | Web2オンボーディング |
| **MCP support** | LLM-friendlyなAIエージェント統合 | AI Agent開発者向け |

### 競合製品との比較

| プロダクト | 発行元 | 対象チェーン | 特徴 |
|---|---|---|---|
| AppKit | TON Foundation | TON | Telegram統合、MCP対応 |
| Coinbase AgentKit | Coinbase | Base/EVM | x402, プログラマブルガードレール |
| elizaOS | Community | Solana/EVM | フレームワーク, 200+plugins |
| Trust Wallet TWAK | Trust Wallet | 25+チェーン | クロスチェーン, DCA |
| SpawnDock (community) | TON builder | TON | 2分スキャフォールド, 266 claps |

---

## 4. TON Pay

### TON Pay 1.0（リリース済み）

| 項目 | 内容 |
|---|---|
| 主な用途 | Telegram Mini Apps向けチェックアウト・決済・レポーティング |
| 決済速度 | 1秒以下 |
| 平均コスト | 1セント以下 |
| 対応ウォレット | Tonkeeper, MyTonWallet, Tonhub, OpenMask, Wallet Bot (TG) |
| 現状対象 | TMAのみ |

### TON Pay 2.0（Q2 2026 予定）

| 予定機能 | 詳細 |
|---|---|
| 新マーチャントカテゴリ拡大 | TMA外のWebアプリへの展開 |
| Webアプリ展開 | ブラウザベースのチェックアウト |
| Gasless transactions | ユーザーが同一トークンでガス代支払い（優先開発中） |
| クリプトリンクカード | "2026年中ローンチ予定"（Nikola Plecas, VP of Payments） |

### TON Pay vs 競合決済プロトコル

| プロトコル | 対象 | 特徴 | TONとの関係 |
|---|---|---|---|
| TON Pay 1.0/2.0 | TMA / Webアプリ | TON公式、マーチャント向け | 本体 |
| x402 | M2M / AI Agent | HTTP 402、Coinbase公式 | TONビルダーが実装中 |
| TonPay 402 | AI Agent | W5 × x402、支出ポリシー | コミュニティ実装 |
| ton402 | JWT決済ゲート | 決済→即時JWT | コミュニティ実装 |

---

## 5. Tolk（スマートコントラクト言語）

TON独自のスマートコントラクト言語。FunCの後継として設計。
- Q2 2026: Tolk Dev Tools リリース
- 型安全性・開発者体験の改善
- 既存FunCプロジェクトからの移行支援ツール

---

## 6. New TON Consensus（Q2 2026）

| 変更点 | 現状 | 新コンセンサス |
|---|---|---|
| アルゴリズム | BFT Consensus | (詳細未公開) |
| 影響範囲 | プロトコルコア | ブロック確定速度・セキュリティ改善 |
| 比較 | — | Solana Alpenglow (150ms) に対抗する可能性 |

---

*関連: [ton-strategy.md](ton-strategy.md) — TON戦略ポジション・公式メッセージ分析*
*関連: [hackathon-trends.md](hackathon-trends.md) — コミュニティのロードマップ先行実装状況*
*作成: 2026-03-28*
