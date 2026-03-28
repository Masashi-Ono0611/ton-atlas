# 分散型通信技術ランドスケープ

> 最終更新: 2026-03-28
> 検閲耐性・プライバシー重視の通信インフラ調査

---

## 概要

インターネット遮断・監視・プラットフォーム検閲に対する技術的回答として、
複数のアーキテクチャアプローチが並行して進化している。

```
レイヤー別分類
─────────────────────────────────────────────────────────
物理レイヤー   Bluetooth Mesh (bitchat, Briar) / LoRa (Meshtastic)
               WiFi Direct / 独自無線プロトコル
─────────────────────────────────────────────────────────
トランスポート  Tor (玉ねぎルーティング) / I2P (にんにくルーティング)
               TON Proxy (ADNL) / Yggdrasil / CJDNS
─────────────────────────────────────────────────────────
ストレージ     IPFS / Filecoin / TON Storage / Arweave
               Swarm (Ethereum) / Storj
─────────────────────────────────────────────────────────
アイデンティ   暗号鍵ペア (Nostr) / ZKP (KYA)
ティ            SBT (Soulbound Token) / W3C DID
─────────────────────────────────────────────────────────
メッセージング  Nostr / Matrix (Federation) / Session
               Signal (中央集権E2E) / Briar / bitchat
─────────────────────────────────────────────────────────
決済           Bitcoin / Monero (プライバシーチェーン)
               Zcash / TON / Lightning Network
─────────────────────────────────────────────────────────
```

---

## bitchat (2025年1月〜)

### 概要
Jack Dorsey (Block Inc, 旧Twitter CEO) が開発したBluetooth Meshメッセージングアプリ。
"Freedom technology"（自由の技術）として明示的に位置づけ。

### 技術仕様
- **プロトコル**: Bluetooth Low Energy (BLE) Mesh
- **通信範囲**: 1デバイス約30m、メッシュで数km圏まで延伸
- **ルーティング**: メッセージは近隣デバイスをホップしながら伝播
- **暗号化**: エンドツーエンド暗号化（受信者の公開鍵で暗号化）
- **ストレージ**: ローカルのみ（サーバーなし）、TTL（Time-To-Live）ベースで消去
- **ID**: 電話番号・アカウント不要。公開鍵がアイデンティティ

### 特徴
- インターネット完全不要で動作
- 中央サーバーなし → 遮断する対象が存在しない
- オープンソース (iOS/Swift)
- 起動後急速に拡散 → 抑圧・検閲環境での需要を実証

### 技術的制約
- Bluetoothの物理特性: 建物・障害物で大幅に距離減衰
- 低帯域幅: テキスト・小さなデータのみ実用的
- バッテリー消費: BLE常時スキャンでバッテリー負荷
- 同時接続数: BLEの仕様上限が存在

### TONへの示唆
- オフライン通信の需要は実証済み
- 「決済」レイヤーが存在しない → 統合余地あり
- Telegram + TON + Bluetooth Mesh = 世界最強の検閲耐性コミュニケーション+決済スタック

---

## TON オーバーレイネットワーク

### ADNL (Abstract Datagram Network Layer)
TONの独自P2Pネットワーク層。Tor類似のアーキテクチャで、IPアドレスを隠蔽する。

```
通信フロー:
[ユーザー] → [TON Proxy Node 1] → [TON Proxy Node 2] → [宛先サーバー]
               (IPアドレス匿名化)    (ルーティング暗号化)
```

### TON Proxy
- Tor相当のオニオンルーティング
- ユーザーのIPアドレスを隠蔽
- 政府・ISPによるブロックを回避
- ノードは誰でも運営可能

### TON Sites (.ton ドメイン)
- 従来のDNS不使用 → 政府によるDNSブロックが無効
- TON DHT (Distributed Hash Table) 上でコンテンツ解決
- 静的サイト・Web3 DAppsをホスト可能
- `.ton` ドメインはTON DNS上で管理
- Webサイトとして: 検閲耐性の高い「代替Web」

### TON Storage
- IPFS相当の分散ファイルストレージ
- コンテンツアドレス型: URLではなくコンテンツハッシュで参照
- 単一サーバーの削除が不可能
- TON Siteのバックエンドストレージとして機能

### TON DNS
- ウォレットアドレス・TON Siteを .ton ドメインに紐付け
- 分散型: ICANN等の中央機関が存在しない
- 名前空間はTONスマートコントラクトで管理

### アクセス方法
- ブラウザ拡張機能 (TON Proxy Chrome Extension)
- Tonkeeper等のウォレットアプリ内ブラウザ
- MyTonWallet内TON Siteブラウザ
- 設定済みプロキシ経由

### 現状の課題
- 認知度低: 900M Telegramユーザーの大多数が存在を知らない
- UX未整備: 「ブラウザを開く」体験がない
- コンテンツ不足: .ton サイト数が少ない（鶏卵問題）
- 速度: Proxyを経由するため通常より遅い

---

## Nostr プロトコル

### 概要
Notes and Other Stuff Transmitted by Relays.
極めてシンプルな分散型ソーシャルプロトコル。

### アーキテクチャ
```
[クライアント] ←→ [リレー1] ←→ [クライアント]
                ↕
              [リレー2]
              ↕
            [リレーN]
```

- **ID**: 公開/秘密鍵ペアのみ。ユーザー名・電話番号不要
- **リレー**: 誰でも運営可能な中継サーバー群
- **検閲耐性**: 特定リレーがBANしても他リレー経由で到達可能
- **シンプルさ**: WebSocket + JSON のみ。仕様が単純

### エコシステム (2026年)
| クライアント | 特徴 |
|---|---|
| Damus | iOS、最初の大規模クライアント |
| Primal | Web/iOS/Android、UX優秀 |
| Snort | Web、軽量 |
| Amethyst | Android |

- Jack Dorsey: 14 BTC (約$500K) を開発者に寄付
- Zap (Lightning Network支払い) 統合: コンテンツに直接マイクロペイメント
- NIP (Nostr Implementation Possibilities): 仕様拡張の提案制度

### TON統合の可能性
- Nostr キーペア = 秘密鍵 → TONウォレットと統合可能
- ZapsのTON版: Nostrコンテンツに直接TON/Jettonで支払い
- Telegram × Nostr クロスポスト: Telegramチャネル → Nostr自動転送

---

## Matrix / Element

### 概要
分散型リアルタイム通信プロトコル。Federation（連合）モデル。

### 特徴
- **Federation**: 異なるサーバーが相互接続（Emailに近い）
- **ブリッジ**: Slack/Discord/IRC/Telegram等との橋渡し
- **採用実績**: フランス政府、ドイツ連邦軍、NASA等
- **E2E暗号化**: デフォルト有効

### Tor/P2Pと異なる点
- 完全にサーバーレスではない（Federationはサーバー依存）
- ただし特定企業が支配できない構造
- 自分でホームサーバーを運営可能

---

## Session Messenger

| 項目 | 詳細 |
|---|---|
| 電話番号 | **不要** |
| 中央サーバー | **なし** |
| ルーティング | オニオンルーティング (Service Nodes) |
| チェーン | Oxen Network (旧Loki) |
| メタデータ | 最小化 (IPアドレス・タイムスタンプを隠蔽) |
| フォーク元 | Signal (暗号化プロトコル継承、中央サーバー除去) |

- ユーザーID = ランダム生成の公開鍵
- Session Nodes: Oxenトークンをステーク、ルーティング提供
- 当局への協力が技術的に不可能な設計

---

## Briar

- Android向けBluetooth/WiFi Direct/Torメッセンジャー
- bitchatの先駆け (2014年〜)
- ジャーナリスト・活動家向けに設計
- メタデータなし: 誰がいつ誰に送ったか不明
- インターネット完全不要モード
- 弱点: iOS非対応、UXが難しい

---

## Meshtastic (LoRa Mesh)

| 項目 | 詳細 |
|---|---|
| 通信方式 | LoRa (低電力長距離無線) |
| 通信距離 | 見通しで10-250km |
| 帯域幅 | 非常に低い (最大数kbps) |
| インターネット | 不要 |
| 用途 | 山岳・農村・緊急時 |
| デバイス | 専用ハードウェア ($30-50) |

- Bluetoothより圧倒的に長距離
- ただし専用デバイス必要、テキストのみ実用的
- ハイキング・災害時・農村コミュニティで活用

---

## 技術比較マトリックス

| 技術 | インターネット不要 | 完全非中央集権 | 決済統合 | UX成熟度 | TON接続性 |
|---|:---:|:---:|:---:|:---:|:---:|
| bitchat (BLE) | ✅ | ✅ | ❌ | 中 | 未統合 |
| TON Proxy+Sites | ❌ | ✅ | ✅ | 低 | ネイティブ |
| Nostr | ❌ | ✅ | ⚡(Zap) | 中 | 可能 |
| Matrix | ❌ | △(Federation) | ❌ | 高 | ブリッジ可 |
| Session | ❌ | ✅ | ❌ | 高 | 未統合 |
| Briar (BLE) | ✅ | ✅ | ❌ | 低 | 未統合 |
| Meshtastic (LoRa) | ✅ | ✅ | ❌ | 低 | 未統合 |
| Tor | ❌ | ✅ | ❌ | 中 | 類似 |

---

## TON固有の競争優位

### 他が持てないもの
1. **900M Telegramユーザー**: Nostrの100万、Sessionの100万とは桁が違う
2. **既製インフラ**: TON Proxy/Sites/Storage/DNS は既存・稼働中
3. **ウォレット統合**: Telegram Wallet / Tonkeeper が標準搭載
4. **法的耐性の実績**: Durov逮捕後もTelegram/TONは動作継続

### 未解決の課題
1. **オフライン決済**: Bluetooth Mesh + TON トランザクションの統合は未実装
2. **TON Sites認知度**: 機能はあるが使っている人がほぼいない
3. **ブラウザ体験**: .ton サイト専用ブラウザが存在しない
4. **開発者ツール**: TON Proxy/Sitesのデプロイツールが貧弱

---

## 関連ファイル

- 思想的背景: [freedom-tech-philosophy.md](freedom-tech-philosophy.md)
- TON公式ロードマップとの関連: [../ton/roadmap-products.md](../ton/roadmap-products.md)
- プロダクト提案: [../../ideation/06-freedom-stack.md](../../ideation/06-freedom-stack.md)
