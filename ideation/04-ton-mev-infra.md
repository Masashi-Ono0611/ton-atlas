# アイデア4: TON MEV Infrastructure — TONのFlashbots

> 作成日: 2026-03-28
> 優先度: ★★（中期）
> カテゴリ: Infrastructure / Research

---

## 問題

### TONのMEVインフラが空白である

| チェーン | MEVインフラ | 採用率 | 年間MEV |
|---|---|---|---|
| Ethereum | Flashbots / MEV-Boost | ブロックの90%+ | $2B+ |
| Solana | Jito | バリデータの80%+ | $500M+ |
| **TON** | **なし** | **— (0%)** | **$0（公表値）** |

**TONのMEVインフラは空白地帯。ETHやSolanaのような「サーチャーがバリデータに支払う」メカニズムが存在しない。**

これは2つの問題を意味する:

1. **機会損失**: TON DeFiにMEVが存在しているが、サーチャーの収益として計上・管理されていない
2. **ユーザー被害**: 構造的に保護されていないユーザーが損失を被っている

---

## ソリューション: TON Flashbots（仮称）

### コアコンセプト

TONのactor model に合わせた、バリデータへのtip付き優先メッセージ実行インフラ。

```
TON MEV Infrastructure の役割:

1. サーチャーがMEV機会を発見
   └── 清算、アービトラージ、タイミング優位

2. サーチャーがバリデータにtipを支払い優先実行を要求
   └── ETHのpriority fee、JitoのSOL tip に相当

3. バリデータがtip付きメッセージを優先処理

4. MEVシェア（ユーザーへのリベート）の仕組みを追加
   └── SUAVEのMEVシェアモデルを参考
```

### TON Actor Modelへの適応

ETHとJitoのモデルをそのままでは使えない。TON固有の設計が必要。

```
ETH Flashbots:
Searcher → Relay → Builder → Proposer(Validator)
問題: TONにはRelay/Builder概念がない

Jito (Solana):
Searcher → Jito Block Engine → Validator(Jito-Solana)
問題: TONのブロック構造が異なる

TON固有の課題:
- Masterchain + Workchains のシャード構造
- 非同期メッセージパッシング（原子性の概念が異なる）
- バリデータが「メッセージキュー内の処理順序」を選択

TON版の設計:
Searcher → TON MEV Gateway → Validator(拡張クライアント)
                ↑ tip（TON）を添付してメッセージキューに投入
                ↑ バリデータがtip最大のメッセージを優先処理
```

---

## 3つのフェーズ

### Phase 1: 研究・分析（OSS、学術貢献）

**TON MEVの構造的分析を公開**

```
調査対象:
1. TONバリデータのメッセージ処理順序の実態
   → ランダム？FIFO？別の基準？
2. 清算MEVの規模
   → Evaaのポジション分析、清算頻度・金額
3. DEXアービトラージの規模
   → DeDust/STONfi間の価格差履歴
4. Front-runningの事例
   → オンチェーンデータから類似パターンを検出
```

**成果物:**
- TON MEV Research Report（OSSリリース）
- TON Foundationへの提出・共有
- ハッカソン・Gateway 2026での発表

**なぜ価値があるか:**
- TONエコシステムにとって「知られていない問題」の可視化
- TON Foundationが対策を講じる動機付けになる
- MEVインフラ構築の正当性・需要を証明する

### Phase 2: バリデータtip プロトコル（PoC）

**最小限のtip実装**

```typescript
// TON MEV-tip メッセージ構造（仮）
interface PriorityMessage {
  regularMessage: TONMessage;  // 通常のメッセージ
  tip: {
    amount: bigint;            // tipのTON量
    recipient: string;         // バリデータアドレス
  };
  searcher: {
    signature: Buffer;         // サーチャーの署名
    bundleId: string;          // バンドルID
  };
}
```

**バリデータ向けクライアント拡張:**
- TON公式ノードにtip処理を追加するパッチ
- バリデータへのインセンティブ提示（追加収益）

**採用のためのハードル:**
- TONバリデータの賛同が必要（インセンティブが鍵）
- TON Foundation との調整
- セキュリティ監査

### Phase 3: MEVシェア（ユーザーリベート）

**SUAVEのMEVシェアモデルをTONに適応**

```
MEVシェアの動作:

1. ユーザーがDEX操作（DeDust等）
2. ユーザーの操作がMEV Gatewayを経由
3. Gatewayがサーチャーにオークション
4. サーチャーがMEV抽出 + ユーザーにリベート還元
5. ユーザーは知らないうちにMEVリベートを受け取っている

→ 「損をしていたMEV」が「得になるMEV」に変わる
```

---

## 競合・先行者優位

### なぜ今参入すべきか

```
MEVインフラの市場特性:

- ネットワーク効果が強い（サーチャー × バリデータ × 取引量）
- ファーストムーバーが事実上の標準になる
  → Flashbots on ETH, Jito on Solana が示す通り
- TONのDeFi成長とともにMEVの絶対額が増加
  → 今参入して先行標準化が経済的に最適

ETHのFlashbotsは2021年リリース → ETH TVLが$50Bになる前に標準化
TONのDeFi TVLは今$0.5B → 同じパターンを辿れば10〜100倍の余地
```

### 競合の不在

TONでMEVインフラを開発しているチームは現時点で確認されていない。
ハッカソン200件の中でもMEVに言及したプロジェクトは0件。

---

## 収益モデル

| フェーズ | 収益源 |
|---|---|
| Phase 1 | なし（研究投資）。ただしTON Foundation資金調達の可能性 |
| Phase 2 | バリデータtip手数料（tip金額の10%） |
| Phase 3 | MEVシェアプラットフォーム料（MEV利益の5〜10%） |

**長期的な最大価値:**
TONがETH程度のDeFi規模（TVL $50B）に成長すれば、
年間MEVが$1B超 → プラットフォーム料5%で年間$50M

---

## リスク

| リスク | 対策 |
|---|---|
| バリデータの賛同が得られない | インセンティブ設計（追加収益）、TON Foundation支持 |
| TON Foundation が公式に実装してしまう | 研究成果を貢献することで公式実装に関与する立場を確保 |
| TON actor modelの技術的制約でbudleが設計できない | Phase 1の研究で実現可能性を先に確認 |
| TON DeFiが成長しない | Phase 1はコスト低い（研究のみ）、Phase 2以降は成長確認後 |

---

## 既存MEV知識の活用度

```
searcher-sponsored-tx（既存実装）からの直接転用:
- Bundle概念: ✅ 直接転用（メッセージチェーンに変換）
- Priority fee: ✅ TON tipに転換
- Relay architecture: ✅ TON MEV Gatewayの設計参考
- Builder selection: ✅ バリデータ選択ロジックの参考
- Atomic inclusion: △ TON actor modelで再設計が必要
```

---

*関連: [mev-ton-opportunity.md](../research/mev-ton-opportunity.md) — TON MEV詳細分析*
*関連: [mev-landscape.md](../research/mev-landscape.md) — ETH/Solana MEVインフラ比較*
*関連: [01-gasless-agent-payment.md](01-gasless-agent-payment.md) — Sponsored Txとの接続*
