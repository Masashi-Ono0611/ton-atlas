# アイデア2: GiftFi — Telegram Gifts 担保型 DeFiプロトコル

> 作成日: 2026-03-28
> 優先度: ★★★（最優先）
> カテゴリ: DeFi / NFT Finance

---

## 問題

### Telegram Giftsとは

Telegramが2024年末から展開している「ギフト」機能。特殊なNFTとして:
- Telegramユーザー間で送受信可能
- レアリティがあり（限定版・シリアル番号付き）
- 二次流通市場が自然発生（Fragment等）
- 価格は数ドル〜数千ドルまで
- 総流通額は不明だが、ハッカソンでアービトラージBot・指数商品が開発されるほどの市場規模

### 問題: 価値はあるが「眠った資産」

```
Telegram Gifts ユーザーの現状:
- ギフトを保有: 「売りたくないが、お金も欲しい」
- 価値はある: レアなGiftsは数千ドル相当
- 流動化できない: 保有したまま利回りも得られない
- 担保にできない: DeFiプロトコルがGifts担保に対応していない
```

**不動産やNFTと同じ問題: 「価値はあるが流動性がない資産」**

現在の解決策:
- 売る → 保有できない（気に入っているなら残したい）
- 持ち続ける → 機会コスト（他の運用ができない）
- アービトラージ → リスクを取る

**GiftFiは「担保にして借りる」第3の選択肢を作る。**

---

## ソリューション: GiftFi

### コアコンセプト

```
NFTレンディングの一般形（青山・BendDAO等）をTelegram Gifts専用で設計

ユーザー（借り手）:
  Telegram Gifts → GiftFi Vault（スマートコントラクト）にロック
        ↓
  USDT（担保率70%相当）を即時借入
        ↓
  返済期限内に USDT + 利息 を返済 → Giftsを取り戻す
  返済失敗 → Giftsが清算（オークション）
```

### アーキテクチャ

```
GiftFi プロトコル
├── Gift Vault Contract
│   ├── Gifts NFTの受け入れ・ロック・解放
│   ├── NFT価値オラクルとの連携
│   └── 清算トリガー管理
│
├── Price Oracle
│   ├── Fragment、GetGems等の流通市場データ収集
│   ├── レアリティ・シリアル番号による価値補正
│   └── TWAP（時間加重平均価格）で操作耐性
│
├── Lending Pool
│   ├── USDTの資金プール（貸し手から調達）
│   ├── 金利モデル（借入率に応じた可変金利）
│   └── 貸し手へのAPY分配
│
└── Liquidation Engine
    ├── 担保比率が閾値以下で清算開始
    ├── オークション（24時間）
    └── 差額はユーザーへ返却
```

### 担保・金利設計

| Giftsのレアリティ | LTV（担保率） | 清算閾値 |
|---|---|---|
| Limited（希少） | 70% | 80% |
| Special（中希少） | 60% | 75% |
| Standard（標準） | 50% | 70% |

**金利モデル（Aave型）:**

```
利率 = 基準金利（5%） + 利用率係数
利用率 = 借入総額 / プール総額

利用率50%以下: 5〜8% APY
利用率50〜80%: 8〜20% APY（急上昇）
利用率80%以上: 20〜50% APY（借り入れを抑制）
```

---

## 差別化・競合優位

### 他チェーンに相当するものがない

| 観点 | GiftFi | BendDAO (ETH NFTレンディング) | NFTfi |
|---|---|---|---|
| 対象資産 | Telegram Gifts（TON固有） | BAYC, Punks等のETH NFT | ETH NFT |
| ユーザー層 | Telegram 10億MAU | Web3ネイティブ | Web3ネイティブ |
| 清算流動性 | Fragment等のGifts市場 | NFT AMM（Blur等） | P2P交渉 |

**最大の差別化: Telegram Giftsは他のどのチェーンにも存在しないTON固有資産。**

### コミュニティ信号

ハッカソン調査で確認済みのGifts関連プロジェクト（全てGiftFiより上位レイヤー）:
- Giftstat（価格分析）→ GiftFiのオラクルデータソースになれる
- Morgan Gift Plugins（アービトラージ）→ GiftFiの清算アービトラージャーになれる
- Giftindex（インデックス商品）→ GiftFiの担保インデックスになれる

**エコシステムが自然に形成されており、GiftFiはその「金融レイヤー」になれる。**

---

## 市場規模の推定

### ボトムアップ推計

```
現在のTelegram Gifts市場:
- Fragment でのGift出来高: 不明（APIアクセスが必要）
- Giftindexの存在: 指数商品が成立するほどの市場
- Morgan Gift Pluginsのアービトラージ: 6取引所をまたぐほどの流動性

仮定:
- Gifts流通総額: $50M〜$500M（推定）
- LTV 60%: 最大 $30M〜$300Mの借入需要
- 年利10%: 年間 $3M〜$30M の利息収入（プロトコルシェア20% = $0.6M〜$6M）
```

---

## 技術実装詳細

### TON スマートコントラクト設計（Tolk/FunC）

```func
// GiftVault.fc
;; Telegram Gifts NFTを受け入れてUSDTを貸し出す

(int, slice) recv_internal(int my_balance, int msg_value, cell in_msg_full, slice in_msg_body) {
    ;; 1. NFTの受け入れ確認
    ;; 2. Oracle価格の取得
    ;; 3. LTV計算
    ;; 4. USDT転送メッセージの送信
}
```

### Price Oracle 設計

**課題:** TONにChainlink相当のオラクルが成熟していない

**解決策:**
1. Fragment等のAPIを集計するオフチェーンオラクル
2. 複数データソースのTWAP計算
3. 価格操作対策: 大きな変動時のliquidation遅延

---

## ビジネスモデル

| 収益源 | 詳細 |
|---|---|
| 金利スプレッド | 借入金利 - 貸し手APY = プロトコル収益（20%） |
| 清算手数料 | 清算発生時の5%手数料 |
| プロトコルトークン | ガバナンストークンによる流動性マイニング |

---

## リスクと対策

| リスク | 対策 |
|---|---|
| Gifts市場の流動性不足 → 清算困難 | 低いLTV設定（50〜70%）+ 段階的清算 |
| Price Oracleの操作 | TWAP、複数データソース、circuit breaker |
| TONスマートコントラクトのバグ | 段階的なTVL上限設定、監査 |
| Telegram社がGiftsの仕様変更 | TON Foundation等との情報収集 |

---

## 参考：既存NFTレンディングプロトコル

| プロトコル | チェーン | TVL | 特徴 |
|---|---|---|---|
| BendDAO | ETH | $50M+ | バランス型プール方式 |
| NFTfi | ETH | $100M+ | P2Pマッチング |
| Arcade.xyz | ETH | — | 機関向け |
| Pine Protocol | ETH/SOL | — | フラッシュローン型 |

---

*関連: [hackathon-trends.md](../research/hackathon-trends.md) — Gifts関連プロジェクト群*
*関連: [defi-trends.md](../research/defi-trends.md) — RWA/レンディングトレンド*
