---
marp: true
theme: default
paginate: true
header: "仕様駆動開発のすすめ"
footer: ""
style: |
  section {
    font-family: "Hiragino Kaku Gothic ProN", "Noto Sans JP", sans-serif;
    font-size: 24px;
  }
  section.title {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
  }
  section.title h1 {
    font-size: 48px;
    margin-bottom: 0.2em;
  }
  section.title h2 {
    font-size: 28px;
    font-weight: normal;
    color: #555;
  }
  section.tldr {
    background: #f0f4ff;
  }
  section.tldr blockquote {
    border-left: 6px solid #3b82f6;
    background: white;
    padding: 1em 1.5em;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.08);
  }
  section.section-title {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
    background: linear-gradient(135deg, #1e3a5f 0%, #2563eb 100%);
    color: white;
  }
  section.section-title h1 {
    font-size: 52px;
    text-shadow: 0 2px 4px rgba(0,0,0,0.2);
  }
  section.section-title header,
  section.section-title footer {
    color: rgba(255,255,255,0.6);
  }
  table {
    font-size: 20px;
    width: 100%;
  }
  th {
    background: #2563eb;
    color: white;
  }
  td, th {
    padding: 0.5em 0.8em;
  }
  code {
    font-size: 20px;
  }
  blockquote {
    border-left: 4px solid #2563eb;
    padding: 0.5em 1em;
    color: #374151;
    background: #f8fafc;
  }
  strong {
    color: #1e40af;
  }
  h1 {
    color: #1e3a5f;
  }
  ul {
    line-height: 1.7;
  }
---

<!-- _class: title -->
<!-- _header: "" -->
<!-- _paginate: false -->

# 仕様駆動開発のすすめ

## Claude Code作者が語る開発のコツと、AWSのKiro

---

<!-- _class: tldr -->

# TL;DR

> AIを使ったコーディングを成功させる核心は、**シンプルな2点だけ**
>
> 1. **ちゃんとplanする**（コーディング前に意図を明文化する）
> 2. **検証ループを整える**（AIが自律的に品質を確認できる仕組みを作る）
>
> Kiroは、この2点を**IDEレベルで誰でも踏めるように仕組み化**したツール。
> でも考え方は、Claude Codeでも普段のチャットでも、すぐに使える。

---

# はじめに

**この発表について：**

- 川崎は自然言語処理の専門家ではない
- 議論になったら、コメントでも質問してね

---

<!-- _class: section-title -->

# 1. 導入
## Claude Code作者が語る開発のコツ

---

# 読んでほしい記事（3本）

**① Boris Cherny氏（Claude Code作者）の開発手法**
Claude Codeを作った本人が「自分はこう使っている」と語った内容の翻訳記事。

**② Boris Tane氏（Cloudflare所属）の記事**
"Boris違い" だが、独自の深い実践から生まれた方法論。

**③ ごく個人的なClaude Codeプラクティス集**
2026年2月公開の最近話題の記事。実践者目線のリアルな工夫が詰まっている。

---

# 3者が共通して言っていること

一見するとHooksやスラッシュコマンドなど細かいTipsも多い。

でも**一番大事なこと**は、意外とシンプル。

<!-- 次のスライドで詳しく -->

---

# 共通点 ① ちゃんとplanする

**Boris Cherny:**
> 「Planモードから始める。良いプランができるまでClaudeと議論し、そこから実装へ移る」

**Boris Tane:**
> 「コードを書く前にプランをレビュー・アノテーションするサイクルを1〜6回繰り返す。`don't implement yet`が最重要」

**Hawkie:**
> 「Planモードから始め、違和感があれば積極的にフィードバックする。最低限の要件や仕様をこの段階で確認しておく」

---

# 共通点 ② 検証ループ（テスト）を整える

**Boris Cherny:**
> 「Claudeに作業を**検証する方法を与えること**が最も重要。このフィードバックループがあれば品質が2〜3倍になる」

**Boris Tane:**
> 「実装中は`continuously run typecheck`を指示する」

**Hawkie:**
> 「何も指示しないと正常系しかテストしない。CLAUDE.mdにTDDで進めるよう明記し、RED→GREEN→REFACTORのサイクルを明示する」

---

# その他の細かいTipsの位置づけ

CLAUDE.md、Hooks、MCP、並列実行……
これらは生産性を上げる工夫であり、コンテキスト管理の問題を解決するもの。

**開発の根幹はそこじゃない。**

> どんなツールを使っていても、**planとテスト（検証ループ）を意識するだけ**で開発品質が大きく変わる。

---

<!-- _class: section-title -->

# 2. Kiroとは

---

# Kiro 概要

**AWS製のAIコーディングツール（2025年7月リリース、同年GA）**

- KiroにはIDEとCLIがある（別物）
  - **IDE**: VSCodeフォーク（Cursor、Antigravityと同じ立ち位置）
  - **CLI**: Claude CodeやCodexに近いターミナル型エージェント
- IDEに**仕様駆動開発（Spec-Driven Development）が組み込まれている**
- 今回はIDEの話

---

# VibeモードとSpecモード

**Vibeモード**
従来のAIコーディング。チャットで対話しながら書いてもらうスタイル。
プロトタイピングや小さなタスクに向いている。

---

**Specモード（Kiroの最大の特徴）**
アイデアを壁打ちして仕様を固め、その仕様に基づいて実装する。
仕様を明確にしたいとき、複雑なフィーチャー開発やチーム開発に向いている。

---

<!-- _class: section-title -->

# 3. 仕様駆動開発とは

---

# 仕様駆動開発の定義

コーディングの前に仕様を明文化し、その仕様を正として開発する手法。

- 要件定義 → 設計 → 実装 → テストを段階的に進める
- 事前の仕様策定により、品質向上と一貫性確保を図る
- 仕様を明確にしたいときに有効（規模の大小に限らず）

---

# Kiro固有のものではない

仕様駆動開発の考え方は、Kiro以外のツールでも実践できる。

- **spec-kit**: GitHubが公開しているOSSのツールキット
- **cc-sdd**: Claude Codeで仕様駆動開発を始めるためのOSSツール

> 「仕様を先に書く」という思想自体は、TDDやBDDと通じる普遍的なもの

---

<!-- _class: section-title -->

# 4. Kiroにおける仕様駆動開発

---

# Steering（ステアリング）

`.kiro/steering/` に配置するMarkdownファイル群。
Specに依らない全体的な開発ルールをここに書く。

**典型的なステアリングファイル例：**
- `product.md` — プロダクトの目的・ユーザー・ビジネス目標
- `tech.md` — 使用技術スタック・ライブラリ・制約
- `structure.md` — ファイル構成・命名規則・アーキテクチャ
- `code-conventions.md` — コードスタイル・命名パターン
- `tdd.md` — テスト方針

---

# Steeringの読み込み制御

inclusion設定により、3段階で制御できる。

| 設定 | 挙動 |
|---|---|
| 常時読み込み | 毎回コンテキストに含まれる |
| キーワード自動読み込み | 関連する話題で自動ロード |
| 手動参照 | 明示的に参照したときだけ |

> コンテキストの節約と情報の適切な提供を両立

---

# Specの3ファイル構造

Specモードで開発すると、Kiroは以下の3ファイルを生成する。

**`requirements.md`**
ユーザーストーリーと受け入れ基準（EARS記法）を記述。
例: `THE System SHALL allow authenticated users to view active listings.`

**`design.md`**
システムアーキテクチャ、シーケンス図、実装方針を記述。

**`tasks.md`**
具体的な実装タスクのリスト（依存関係順）。
Kiroが実装を進めながらタスクを更新していく。

---

# Specの種類

**Feature Spec（Requirements-First）**
何を作るかは明確だが技術的な道筋が未決の場合。
要件→設計→タスクの順に進む。

**Feature Spec（Design-First）**
技術アーキテクチャがすでに決まっている場合。
設計→要件逆算→タスクの順に進む。

**Bugfix Spec**
バグ修正専用。「現在の挙動」「期待する挙動」「変えてはいけない挙動」の3セクション。
リグレッション防止に特化。

> 用途に合わせてSpecを選ぶことが大事

---

<!-- _class: section-title -->

# 5. 検証ループ
## プロパティベーステスト

---

# AIコード生成の根本的な問題

AIがコードを生成したとき、
**「そのコードが仕様通りか」をどうやって確認するか？**

従来のユニットテストは**「例示テスト」**。
「この入力ならこの出力」という個別のケースを確認するにすぎない。

テストを書く人（人間でもAIでも）が
思いつかなかったエッジケースは漏れる。

---

# プロパティベーステスト（PBT）とは

| | ユニットテスト | プロパティベーステスト |
|---|---|---|
| テストの書き方 | 「入力Aのとき出力はBであること」 | 「任意の入力について、この性質が成り立つこと」 |
| テストケース数 | 手書きで用意した数だけ | 数百〜数千をランダム生成 |
| 向いている場面 | 仕様の特定ケースを確認 | 仕様の普遍的な性質（不変条件）を検証 |

---

# PBTの例：交通信号シミュレーター

**要件（EARS記法）：**
`THE System SHALL ensure no two directions are green at the same time.`

**ユニットテスト：**
「北が青のとき、南は赤か」を1ケース確認

**PBT：**
ランダムな信号状態パターンを何千通りも生成し、
常にこの性質が成り立つか検証

---

# KiroにおけるPBTの位置づけ

GA版で追加された新機能。

- KiroはEARS記法で書かれた要件から、自動的にプロパティを抽出
- そのプロパティに対してランダムなテストケースを大量生成して実行
- 反例が見つかったら、実装・テスト・仕様のどれを修正すべきかをサーフェス
- Bugfix Specでは「変えてはいけない挙動」も含めてPBTが生成

> これが「仕様とコードが本当に一致しているか」を機械的に検証する仕組み

---

<!-- _class: section-title -->

# 6. 仕様駆動開発を成功させるコツ

---

# ① まずSteeringを整備する

- Specを作り始める前に、全体的な開発ルールをSteeringに書いておく
- 複数のSpecに跨る事項はSteeringが担う
  - 使う技術・命名規則・テスト方針など
- Steeringが整っていると、各Specがシンプルになり、AIが毎回同じルールを守ってくれる

---

# ② Specを大きくしすぎない

**これが最重要のベストプラクティス。**

- 要件が大きいと、どこかが漏れやすくなる
- レビュー負荷が下がり、自分もAIも迷いにくくなる
- **独立したドメインに閉じた範囲で分割する**ことがベストプラクティス
  （機能を小さく切るというより、関心事を分離して互いに依存しない単位にする）

---

# ③ 用途に合わせたSpecを選ぶ

Feature Spec（Requirements-First / Design-First）、Bugfix Specなど、
用途に合ったSpecを選ぶ。

特にバグ修正には**Bugfix Spec**が有効。
「変えてはいけない挙動」を最初に明示するのがポイント。

---

# ④ Plan工程に強いモデルを使う

仕様策定フェーズが最も重要なので、ここでモデルをケチらない。

OpusとSonnetでは、仕様の質に大きな差が出る。
実装フェーズでも同様で、モデルの品質は常に結果に影響する。

---

# ⑤ コンテキストを節約する

KiroはSteeringや仕様ファイルを常時読み込むため、コンテキストが圧迫されやすい。

- Steeringの内容を厳選・精査する
- 特定ユースケースにしか使わない情報はSkillsに退避
- MCPを目的に合わせて選別する
- **Powersを活用する**
  - MCP・Steering・Hooksを1パッケージにまとめたもの
  - キーワードに反応して必要なときだけ動的にロード
  - Figma、Supabase、Stripe等のパートナー製Powerも存在

---

<!-- _class: section-title -->

# 7. まとめ

---

# 今日持ち帰ってほしいこと

**① Kiroというツールが存在すること**
VSCodeフォークで、Spec-Driven Developmentが組み込まれたAWS製のAI IDE。

**② 仕様駆動開発という考え方**
コードを書く前に意図を明文化し、その仕様を正として開発する。
Kiro固有ではなく、どんなツールでも意識できる普遍的な考え方。

**③ 今日からできること**
- Claude Codeを使うとき → 最初にPlanモードで計画させてみよう
- 普段のChat → 「まず計画を立てて」と一言加えるだけで変わる
- 検証ループを必ず作ろう → まずlinterからでもOK

---

# 参考リンク

- [Kiro公式](https://kiro.dev) / [Docs: Specs](https://kiro.dev/docs/specs/) / [Docs: Steering](https://kiro.dev/docs/steering/)
- [Kiro Powers](https://kiro.dev/powers/) / [PBT](https://kiro.dev/blog/property-based-testing/)
- [Spec types（AWS Blog）](https://aws.amazon.com/jp/blogs/news/specs-bugfix-and-design-first/)
- [Boris Cherny氏の記事（翻訳）](https://zenn.dev/mohy_nyapan/articles/a07975837386f7)
- [Boris Tane氏の記事](https://boristane.com/blog/how-i-use-claude-code/)
- [ごく個人的なClaude Codeプラクティス集](https://zenn.dev/hawkymisc/articles/2cace1f599cc06)
- [spec-kit](https://github.com/github/spec-kit) / [cc-sdd](https://github.com/gotalab/cc-sdd)
- [実例リポジトリ](https://github.com/kawaaaas/serverless-spa-construct-test)
