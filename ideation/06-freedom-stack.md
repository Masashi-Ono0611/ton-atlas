# 06. TON Freedom Stack — 検閲耐性の通信・Web・決済レイヤー

**優先度**: ★★ (中期・独自性高い)
**カテゴリ**: インフラ / 社会インフラ
**関連研究**:
- [sovereignty/decentralized-communication.md](../research/sovereignty/decentralized-communication.md)
- [sovereignty/freedom-tech-philosophy.md](../research/sovereignty/freedom-tech-philosophy.md)
- [ton/roadmap-products.md](../research/ton/roadmap-products.md)

---

## 課題: TONのインフラは存在するが「使われていない」

TONはすでに世界水準の検閲耐性インフラを持っている:

| 機能 | 実装状況 | 利用率 |
|---|---|---|
| TON Proxy (Tor-like routing) | ✅ 稼働中 | 極低 |
| TON Sites (.ton domain) | ✅ 稼働中 | 極低 |
| TON Storage (IPFS-like) | ✅ 稼働中 | 極低 |
| TON DNS | ✅ 稼働中 | 中程度 |
| Telegram E2E (Secret Chat) | ✅ 稼働中 | 低 (通常チャット比) |

**問題の構造: 技術はある、プロダクトがない**

```
現状: [TON Proxy] [TON Sites] [TON Storage] — それぞれ独立、UX未整備
                                                  ↓
理想: 統合された「自由のためのブラウザ」体験
```

### 追加の課題: 「最後の1マイル」問題

政府によるインターネット完全シャットダウン時、TON Proxyも機能しない。
bitchat (Bluetooth Mesh) はオフライン通信を実現したが、**決済レイヤーがない**。

---

## 提案: 2層構造の Freedom Stack

### Layer A: TON Sovereign Browser
### Layer B: MeshPay (オフライン決済)

この2つは独立したプロダクトとして出発し、最終的に統合される。

---

## Layer A: TON Sovereign Browser

### コンセプト

「.tonサイトにアクセスするための、プライバシー優先の専用ブラウザ」

bitchatが「インターネット不要のメッセージング」を実現したように、
TON Sovereign Browserは「検閲できないWebブラウジング」を実現する。

### 差別化ポイント (既存TONウォレット比)

| 機能 | Tonkeeper等 | TON Sovereign Browser |
|---|---|---|
| 主目的 | 資産管理・DApp | コンテンツブラウジング |
| TON Proxyデフォルト | △ (オプション) | ✅ 常時オン |
| .ton サイト表示 | 部分的 | ✅ ネイティブ |
| TON Storage ファイル | ❌ | ✅ |
| プライバシーモード | ❌ | ✅ (IPアドレス隠蔽) |
| Nostr統合 | ❌ | ✅ |
| 決済UI | ✅ | ✅ (マイクロペイメント) |

### ターゲットユーザー

**Primary**: 検閲環境下のユーザー (中国、ロシア、イラン、ベラルーシ等)
- Telegramは使えるが、TONの高度な検閲耐性機能を知らない
- 900M人の中に何百万人も存在する

**Secondary**: ジャーナリスト・活動家・プライバシー意識の高いユーザー
- Tor Browser のようなポジション

**Tertiary**: 開発者・DAO・DeFiプロトコル
- 検閲耐性の高い「フロントエンド」が必要なプロジェクト
- スマートコントラクトがあってもUIをテイクダウンされるリスク (Uniswap前例)

### アーキテクチャ

```
┌──────────────────────────────────────────────────────┐
│              TON Sovereign Browser                   │
├──────────────────────────────────────────────────────┤
│  UI Layer                                            │
│  ┌─────────────────┐  ┌──────────────┐              │
│  │ .ton Site Viewer │  │ TON Wallet   │              │
│  │  (censorship-   │  │  (payments)  │              │
│  │  resistant web) │  │              │              │
│  └─────────────────┘  └──────────────┘              │
├──────────────────────────────────────────────────────┤
│  Privacy Layer                                       │
│  ┌─────────────────────────────────────────────┐     │
│  │  TON Proxy (ADNL)                           │     │
│  │  [User IP] → [Node1] → [Node2] → [Site]   │     │
│  │             encrypted    encrypted           │     │
│  └─────────────────────────────────────────────┘     │
├──────────────────────────────────────────────────────┤
│  Content Layer                                       │
│  ┌──────────────┐  ┌──────────────┐  ┌───────────┐  │
│  │  TON Sites   │  │ TON Storage  │  │  Nostr    │  │
│  │  (.ton DNS)  │  │  (files)     │  │  relays   │  │
│  └──────────────┘  └──────────────┘  └───────────┘  │
└──────────────────────────────────────────────────────┘
```

### コンテンツのマネタイズ: Sovereign Micropayments

```
ユーザーが.tonサイトを閲覧
     ↓
サイトオーナーが "0.01 TON to read" を設定
     ↓
ブラウザが自動的に TON ストリーミング決済を処理
     ↓
オーナーはサーバーを持たずに収益化できる
     ↓
政府がドメインを没収できない (TON DNS)
     広告なし・検閲なし・テイクダウンなし
```

**これは「Substack × Tor × IPFS」の統合**

### デベロッパー向けツール: Sovereign Deploy Kit

```bash
# 1行で.ton検閲耐性サイトをデプロイ
npx ton-sovereign-deploy ./build/

# 出力:
# ✅ Files uploaded to TON Storage: bag-id-abc123
# ✅ .ton site registered: mysite.ton
# ✅ TON DNS configured
# 🔗 Access via: mysite.ton (TON Proxy required)
# 🔗 Fallback URL: ton://bag-id-abc123 (direct TON Storage)
```

ターゲット: DeFiプロトコル・DAO・メディア・ジャーナリスト

---

## Layer B: MeshPay (オフライン TON 決済)

### コンセプト

「インターネットなしで TON を送れる」

bitchat + TON の決済能力を統合:
- インターネットシャットダウン中でもTON決済が可能
- 署名済みトランザクションをBluetooth Meshで伝播
- オンライン復帰時に自動的にTONネットワークへブロードキャスト

### 技術設計

```
オフライン決済フロー:

1. Sender (スマホA)
   TON_Txを秘密鍵で署名 → 有効な署名済みTx生成
   ↓ Bluetooth BLE

2. Relay Node (スマホB, C, D)
   署名の有効性を検証 (インターネット不要: 公開鍵で検証可)
   → 有効なら次のノードへ伝播
   → キャッシュとして一時保存

3. Gateway Node (スマホE, インターネット接続あり)
   TONネットワークへブロードキャスト
   → ファイナリティ確認
   → メッシュネットワーク逆方向に確認を伝播
```

```typescript
// MeshPay SDK API設計
interface MeshPayTransaction {
  to: string;       // TON address
  amount: bigint;   // nanoTON
  memo?: string;
  ttl: number;      // 有効期限 (秒), メッシュ上で消去されるまで
  signed: Uint8Array; // 署名済みTxバイナリ
}

// オフライン送信
const tx = await meshPay.createOfflineTx({
  to: "EQD...",
  amount: toNano("10"),
  ttl: 3600 // 1時間以内にブロードキャストされなければ無効
});

// BLEメッシュに投入
await meshPay.broadcastToMesh(tx);

// 確認待ち (ゲートウェイノードがオンラインになると通知)
meshPay.onConfirmed(tx.id, (result) => {
  console.log("Confirmed in block:", result.blockId);
});
```

### ユースケース

| シナリオ | 詳細 |
|---|---|
| 抗議活動中のインターネットシャットダウン | イランやミャンマーでの実例: 政府が回線を切断 → TON/Telegramも使えない → MeshPayなら継続 |
| 農村・離島での送金 | インターネットインフラ不足地域での日常的送金 |
| 災害時 | 地震・洪水でISPが破壊された状況での金融アクセス |
| セキュリティ意識の高い取引 | オンラインの痕跡を最小化したい取引 |

### MeshPay × bitchat統合

bitchatはすでにBLEメッシュ通信を実装済み。
MeshPayはbitchatのメッシュネットワークの上に決済レイヤーを追加する構造にできる:

```
bitchat network (BLE Mesh)
       ↓
MeshPay plugin for bitchat:
  - 決済Txをbitchatメッセージとして伝播
  - メッセージ受信者が自動的にリレーノードになる
  - bitchatのユーザーがゲートウェイになることでTONにブロードキャスト
```

**これはbitchatをforkする必要なく、プラグインとして構築できる。**

---

## ロードマップ

### Phase 1: TON Sovereign Deploy Kit (0-3ヶ月)
- TON Storage + TON Sites の開発者向けCLIツール
- `npx ton-sovereign-deploy` で1コマンドデプロイ
- アーリーアダプター: 検閲耐性フロントエンドが必要なDeFiプロトコル
- **成功指標**: .ton サイト数の増加, デプロイされたdApps数

### Phase 2: TON Sovereign Browser (3-9ヶ月)
- TON Proxy統合ブラウザ (Electronベース or 専用スマホアプリ)
- .ton / TON Storage / Nostr のネイティブサポート
- マイクロペイメント統合
- **成功指標**: MAU, 検閲環境下のユーザー獲得

### Phase 3: MeshPay MVP (6-12ヶ月)
- iOS/Android BLE Mesh + TON Tx伝播 PoC
- bitchatオープンソースの活用検討
- **成功指標**: オフライン → オンライン時のTx成功率

### Phase 4: 統合 (12ヶ月+)
- Sovereign Browser + MeshPay の統合
- Telegram Mini App との統合
- BitchatへのMeshPayプラグイン提供

---

## 競合分析

| プロダクト | 重複度 | 差別化 |
|---|---|---|
| Tor Browser | △ (匿名ブラウジング) | TONネイティブ決済なし、.ton対応なし |
| Brave Browser | △ (プライバシー) | TONエコシステム外 |
| TON Connect | △ (ウォレット) | ブラウジング体験なし |
| bitchat | △ (Bluetoothメッシュ) | 決済なし、TON非統合 |
| Briar | △ (オフラインメッシュ) | 決済なし、TON非統合 |

**直接競合ゼロ**: TON × ブラウジング × プライバシー × オフライン決済の統合は存在しない。

---

## なぜ今か

1. **Durov逮捕のアフターショック**: Telegramコミュニティが「プラットフォームリスク」を意識
2. **bitchat viral**: 自由通信への需要を実証（Jack Dorseyの影響力で広まった）
3. **TONインフラが整った**: Proxy/Sites/Storageは動作中、あとはUXだけ
4. **Telegram 900M**: フロントエンドのディストリビューションチャネルが存在する
5. **政治的時機**: EU Chat Control, 各国の暗号化バックドア議論 → ユーザーの需要増加

---

## 思想的ポジショニング

このプロダクトは単なるツールではなく、**主張**でもある。

> "Telegram promised 'Don't be evil.'
>  TON Freedom Stack makes 'Can't be evil' possible."

- Pavel Durov のビジョンの技術的実装
- Cypherpunkの哲学の現代的ユーザー体験
- 「自由は当たり前ではない、技術で守るものだ」というメッセージ

**これはTONエコシステムのブランドとしても強力:**
- "TON: The censorship-resistant social layer"
- ハッカソン・Gateway 2026での訴求力が高い

---

## 関連ideation

- [01-gasless-agent-payment.md](01-gasless-agent-payment.md) — Gasless TxはMeshPayでも必要
- [05-kya-ton-identity.md](05-kya-ton-identity.md) — Freedom Stackでの匿名ID管理
