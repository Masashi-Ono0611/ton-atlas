# 自由技術の思想的背景

> 最終更新: 2026-03-28
> 言論・通信・資産保有の自由を技術で解決するという思想的系譜

---

## 概念の核心

```
「Don't be evil」(Googleの約束)
        ↓ パラダイムシフト
「Can't be evil」(暗号学的な強制)

「信頼してください」(Web2のTOS)
        ↓
「信頼は不要です」(数学が保証する)
```

これは約束の問題ではなく、**アーキテクチャの問題**である。
中央集権的なサービスは、いつでも技術的には裏切れる。
分散型暗号システムは、裏切ることが数学的に不可能になる。

---

## Cypherpunk運動の系譜 (1980年代〜)

### 前史: 公開鍵暗号の誕生
- **1976年**: Diffie-Hellmanによる公開鍵交換の発明
- **1977年**: RSA暗号 (Rivest, Shamir, Adleman)
- **1991年**: PGP (Pretty Good Privacy) - Phil Zimmermann
  - 個人が政府・企業に頼らず強力な暗号を使える時代の幕開け
  - 米国政府はZimmermannを輸出規制違反で3年間捜査

### Cypherpunks メーリングリスト (1992年〜)
- 設立: Eric Hughes, Tim May, John Gilmore
- 参加者: 500名超、後の暗号・プライバシー技術の多くがここで育つ

#### 「A Cypherpunk's Manifesto」(Eric Hughes, 1993)
> "Privacy is not secrecy. A private matter is something one doesn't want the whole world to know,
> but a secret matter is something one doesn't want anybody to know.
> Privacy is the power to selectively reveal oneself to the world."

> プライバシーとは秘密ではない。プライバシーとは、自分が世界に何を開示するかを
> 選択できる力のことだ。

#### 「Crypto Anarchist Manifesto」(Timothy May, 1988/1994)
> "A specter is haunting the modern world, the specter of crypto anarchy."
> 暗号技術は、政府が国民を監視・統制する能力を根本から破壊する。

### 主要な節点

| 年 | 出来事 | 意義 |
|---|---|---|
| 1993 | Eric Hughes "Cypherpunk's Manifesto" | 運動の哲学的基盤確立 |
| 1995 | Netscape SSL導入 | 商業的暗号の普及 |
| 2001 | BitTorrent | 分散コンテンツ配信の実証 |
| 2004 | Tor公開 (EFF支援) | 匿名通信インフラの一般公開 |
| 2006 | WikiLeaks設立 | 内部告発の技術的保護 |
| **2008** | **Bitcoin whitepaper** | **金融主権の技術的実現** |
| 2011 | Silk Road (Tor+Bitcoin) | プライバシーコインの最初の大規模用途（違法含む） |
| **2013** | **Edward Snowden暴露** | **NSA大量監視プログラムの実証** |
| 2013 | Telegram創設 (Durovs) | 検閲耐性メッセージングの大衆化 |
| 2016 | Signal Protocolの標準化 | E2EEの業界標準確立 |
| 2018 | Telegram vs FSB → 2年間ロシアでBANされながら継続 | 技術的検閲耐性の実証 |
| **2022** | **Tornado Cash制裁** | **コードは言論か? の法的闘争** |
| **2024** | **Pavel Durov逮捕** | **プラットフォーム開発者への直接的脅迫** |
| **2025** | **bitchat公開** | **Bluetoothメッシュによるオフライン自由通信** |
| 2026 | TON AgenticKit | AIエージェント × 自由技術の融合 |

---

## 3つの自由の柱

### 1. 通信の自由 (Communication Freedom)
- 誰と何を話したか、見られない
- 政府・企業が通信を遮断できない
- 物理インフラ（ISP、電話）に依存しない

**技術的回答**: E2EE (Signal), Tor, bitchat (Bluetooth Mesh), TON Proxy

### 2. 資産保有の自由 (Financial Sovereignty)
- 銀行口座が凍結されない
- インフレで資産が毀損されない
- 海外送金が許可なく制限されない

**技術的回答**: Bitcoin, Monero, TON/Jetton, Lightning Network

### 3. 情報の自由 (Information Freedom)
- Webサイトが政府命令でテイクダウンされない
- 「不便な真実」を削除できない
- DNSブロックが効かない

**技術的回答**: IPFS, TON Storage + TON Sites, Arweave, Tor Hidden Services

---

## 現在の脅威環境 (2026年)

### 地政学的検閲

| 国/地域 | 状況 |
|---|---|
| **中国** | Great Firewall: Google/Facebook/WhatsApp/Telegram全ブロック。VPN取締り強化中 |
| **ロシア** | Telegram: 2018-2020年ブロック(後解除)、Meta/Twitter/Instagram現在もブロック |
| **イラン** | Mahsa Amini抗議(2022): 完全インターネットシャットダウン。Instagram/WhatsAppブロック |
| **北朝鮮** | 一般市民のインターネットアクセス原則禁止 |
| **インド** | 中国系アプリ200+をBANの前例(2020-) |
| **EU** | Chat Control提案: E2EE暗号化されたメッセージの強制スキャン検討中 |
| **米国** | FBI: 暗号化バックドア要求継続。TikTok禁止令 |

### プラットフォーム依存リスク

**2024年Pavel Durov逮捕事件**
- フランスで逮捕、犯罪コンテンツの共犯疑惑
- Telegramが児童搾取・薬物・詐欺の温床として捜査対象
- 結果: Durovは保釈後もフランスに軟禁状態
- **含意**: 開発者個人を逮捕することで、プラットフォームに「協力」を強制できる

**「チリングエフェクト」**:
- Durov逮捕以降、プライバシー重視アプリの開発者が萎縮
- E2EEデフォルト化を撤回・遅延するプロジェクトが複数出現
- Metaは要求があればDM内容を当局提供することを明言

**「Tornado Cash」(2022)**
- 米国財務省がコードをサンクション対象に
- スマートコントラクトのデプロイアドレスを制裁リストに追加
- オープンソースコードを「書いた・デプロイした・使った」だけで犯罪化の試み
- **含意**: コードは言論か？スマートコントラクトは財産か？

---

## 「Can't be evil」アーキテクチャの系譜

### Tier 1: 約束 (Promise)
- Google "Don't be evil" → 廃止
- Telegram "We won't share your chats" → 技術的には可能、法的圧力に弱い

### Tier 2: 連合 (Federation)
- Matrix / Mastodon: サーバーオペレーターの信頼が必要
- 単一企業の支配はないが、国家がサーバーオペレーターを強制できる

### Tier 3: 分散 (Decentralized)
- Nostr: リレーをBANされても他リレーで到達可能
- IPFS: コンテンツを1サーバーから削除しても他ノードに残る
- TON Sites: .tonドメインはICAN/政府が止められない

### Tier 4: P2P (Peer-to-Peer, No Server)
- Bitcoin: バリデーターを全員逮捕しても、別の誰かが継続できる
- bitchat: サーバーが存在しないため遮断不可能
- Briar/Meshtastic: インターネット不要

### Tier 5: 物理的独立 (Physical Independence)
- Bluetooth/LoRa Mesh: ISPの回線すら不要
- 政府がインターネットを完全シャットダウンしても動作

---

## TON/Telegramの特殊な立ち位置

### Duroveの個人的経歴が製品設計に埋め込まれている

```
2006: Pavel Durov, VKontakte (ロシア版Facebook)を設立
2012: ロシア政府がVKを反対派ユーザーリストの提出要求
      → Durov拒否 → 2014年CEO解任・ロシア出国
2013: Telegram設立 (Nikolaiと共に)
      → 設計思想: "We're not going to give up user data under any circumstances"
2018: Telegramがロシアにて全面ブロック
      → TON開発加速 (ブロックチェーンによる検閲耐性強化)
2024: Durov, フランスで逮捕
      → 釈放後も: Telegarmのモデレーションポリシーを一部変更
                  ただしE2EEは維持、プロトコル自体は変更なし
```

### TONのDNA
- Telegram の検閲耐性 DNA は TON に受け継がれている
- TON Proxy / TON Sites / TON Storage は「単なる機能」でなく、**哲学の実装**
- Pavel: "Telegram will never share private messages with governments"
- TON: "The system is designed so nobody can control it, including us"

---

## Jack Dorseyの「Freedom Technology」フレーミング (2024〜)

### X(旧Twitter)での議論
- Dorsey: "Bitcoin is freedom technology" (長年の主張)
- 2024: X社における言論モデレーション論争 (イーロン・マスク買収後)
- Dorsey: Bluesky → Nostrへの関心移行
- 2025年1月: **bitchat公開** — "This is what I mean by freedom technology"

### 「Freedom Technology」の定義 (Dorsey)
1. 特定の国家・企業に依存しない
2. 誰も操作・検閲できない
3. 誰もが参加できる（許可不要）
4. 強制できない（使わせることも禁止することも不可）

---

## 「通信の自由」を技術で解決するアプローチ別分類

| アプローチ | 代表例 | 弱点 |
|---|---|---|
| **暗号化のみ** | iMessage, Signal | サーバーは中央集権、当局が強制可能 |
| **連合モデル** | Matrix, Mastodon | サーバーオペレーターは当局の標的に |
| **P2P完全分散** | Nostr, Bitcoin | スケーラビリティ・UXの課題 |
| **物理層独立** | bitchat (BLE), Meshtastic (LoRa) | 距離・帯域の物理的制約 |
| **オーバーレイネットワーク** | Tor, I2P, TON Proxy | インターネット自体が必要 |
| **ハイブリッド** | (未実現) | TONが最有力候補 |

**ハイブリッドアプローチの理想形**:
```
通常時: TON Proxy (Tor-like) + TON Sites (censorship-resistant web)
障害時: Bluetooth Mesh (bitchat-like) + 署名済み取引をオフラインで伝播
統合: Telegram × TON × Bluetooth = 通信 × 決済 × Web
```

---

## なぜ今が重要か

1. **AI監視の強化**: AI by governments for mass surveillance は加速している
2. **規制の転換点**: EU Chat Control, 各国の暗号化バックドア要求が本格化
3. **前例の蓄積**: Durov逮捕, Tornado Cash制裁 — 「コードを書いた者」への責任追求
4. **需要の実証**: bitchatが急速に拡散 — 潜在需要はある
5. **TONのタイミング**: AgenticKit, Gateway 2026 — インフラ整備が完了しつつある

> "The arc of technology bends toward freedom, but only if engineers bend it." — Eric Hughes精神の現代的解釈

---

## 関連ファイル

- 技術詳細: [decentralized-communication.md](decentralized-communication.md)
- TONインフラ詳細: [../ton/roadmap-products.md](../ton/roadmap-products.md)
- プロダクト提案: [../../ideation/06-freedom-stack.md](../../ideation/06-freedom-stack.md)
