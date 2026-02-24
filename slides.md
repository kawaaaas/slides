---
marp: true
theme: default
paginate: true
style: |
  @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;700;900&family=JetBrains+Mono:wght@400;700&display=swap');

  :root {
    --bg: #0f172a;
    --bg-light: #1e293b;
    --accent: #60a5fa;
    --text: #e2e8f0;
    --muted: #94a3b8;
    --border: rgba(255,255,255,0.08);
  }

  section {
    font-family: "Noto Sans JP", "Hiragino Kaku Gothic ProN", sans-serif;
    font-size: 26px;
    background: var(--bg);
    color: var(--text);
    padding: 56px 72px 72px;
    line-height: 1.8;
    justify-content: flex-start;
    text-align: left;
  }

  section::after {
    font-family: "JetBrains Mono", monospace;
    font-size: 13px;
    color: var(--muted);
  }

  /* --- Headings --- */
  h1 {
    font-weight: 900;
    font-size: 40px;
    color: var(--text);
    border-bottom: 3px solid var(--accent);
    padding-bottom: 0.25em;
    margin-bottom: 0.7em;
    line-height: 1.3;
  }

  h2 {
    font-weight: 700;
    font-size: 30px;
    color: var(--muted);
    margin-bottom: 0.6em;
  }

  h3 {
    font-weight: 700;
    font-size: 25px;
    color: var(--accent);
    margin-bottom: 0.3em;
    margin-top: 0.6em;
  }

  /* --- Inline --- */
  strong { color: var(--accent); font-weight: 700; }
  a { color: var(--accent); text-decoration: underline; text-underline-offset: 3px; }

  /* --- Code --- */
  code {
    font-family: "JetBrains Mono", monospace;
    font-size: 0.85em;
    background: var(--bg-light);
    color: #93c5fd;
    padding: 0.15em 0.5em;
    border-radius: 4px;
  }

  pre {
    background: #151f32 !important;
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 1em 1.2em !important;
  }

  pre code {
    background: none;
    padding: 0;
    font-size: 20px;
    color: var(--text);
  }

  /* --- Lists --- */
  ul, ol { line-height: 1.9; }
  li::marker { color: var(--accent); }

  /* --- Blockquote --- */
  blockquote {
    background: var(--bg-light);
    border-left: 4px solid var(--accent);
    padding: 0.7em 1.3em;
    border-radius: 0 8px 8px 0;
    color: var(--text);
    font-size: 24px;
    margin: 0.8em 0;
  }

  /* --- Table (force override default theme) --- */
  section table {
    width: 100%;
    border-collapse: collapse;
    font-size: 22px;
    border: none;
    background: transparent;
  }

  section table tr {
    background: transparent !important;
    border: none !important;
  }

  section table th {
    color: #e2e8f0 !important;
    background: #1e293b !important;
    font-weight: 700;
    padding: 0.6em 1em;
    text-align: left;
    border: none !important;
    border-bottom: 2px solid #60a5fa !important;
  }

  section table td {
    color: #e2e8f0 !important;
    background: transparent !important;
    padding: 0.6em 1em;
    text-align: left;
    border: none !important;
    border-bottom: 1px solid rgba(255,255,255,0.08) !important;
  }

  section table strong {
    color: #60a5fa !important;
  }

  /* ================================
     SLIDE VARIANTS
     ================================ */

  /* --- Title --- */
  section.title {
    justify-content: center;
    text-align: center;
  }

  section.title h1 {
    font-size: 56px;
    border-bottom: none;
    padding-bottom: 0;
    margin-bottom: 0.2em;
  }

  section.title h2 {
    font-size: 24px;
    font-weight: 400;
    color: var(--muted);
  }

  /* --- Divider --- */
  section.divider {
    justify-content: center;
    text-align: center;
    background: var(--bg-light);
  }

  section.divider h1 {
    font-size: 48px;
    border-bottom: none;
    margin-bottom: 0.15em;
  }

  section.divider h2 {
    font-size: 24px;
    font-weight: 400;
    color: var(--muted);
  }

  /* --- TL;DR --- */
  section.tldr blockquote {
    background: rgba(96,165,250,0.08);
    border-left-width: 5px;
    padding: 1em 1.8em;
    font-size: 25px;
  }

  /* --- Quotes --- */
  section.quotes blockquote {
    margin: 0.5em 0;
    padding: 0.5em 1.2em;
    font-size: 22px;
  }

  /* --- Refs --- */
  section.refs { font-size: 21px; }
  section.refs ul { line-height: 2.2; }
---

<!-- _class: title -->
<!-- _paginate: false -->

# 仕様駆動開発のすすめ

## Claude Code作者が語る開発のコツと、AWSのKiro

---

<!-- _class: tldr -->

# TL;DR

> AIコーディングで重要なのは **2点だけ**
>
> **1.** **ちゃんとplanする** -- コーディング前に意図を明文化する
> **2.** **検証ループを整える** -- AIが自律的に品質を確認できる仕組みを作る
>
> Kiroはこの2点を **IDEレベルで仕組み化** したツール。
> 考え方自体は Claude Code でも普段のチャットでも使える。

---

# はじめに

- 川崎は自然言語処理の専門家ではないです
- ゆるい感じで進めていくので、気になったらコメントでも質問でも是非！
- このスライドはすべてAI × Marpで作成してます、時間なくすみません

---

<!-- _class: divider -->

# 1. 導入

## Claude Code作者が語る開発のコツ

---

# 読んでほしい記事（3本）

### 1. Boris Cherny 氏（Claude Code 作者）

Claude Codeを作った本人が「自分はこう使っている」と語った内容の翻訳記事

### 2. Boris Tane 氏（Cloudflare 所属）

"Boris違い"だが、独自の深い実践から生まれた方法論（Cloudflare勤務のエンジニア）

### 3. ごく個人的な Claude Code プラクティス集

2026年2月公開。実践者目線のリアルな工夫が詰まっている

---

# 3者が共通して言っていること

Hooks、スラッシュコマンドなど細かい Tips も多いが、
**一番大事なこと**は意外とシンプル。

> **Plan** と **検証ループ**。この2つ。

---

<!-- _class: quotes -->

# 共通点 1 -- ちゃんとplanする

**Boris Cherny:**

> Planモードから始める。良いプランができるまでClaudeと議論し、そこから実装へ移る

**Boris Tane:**

> コードを書く前にプランをレビュー・アノテーションするサイクルを1〜6回繰り返す。`don't implement yet` が最重要

**Hawkie:**

> Planモードから始め、違和感があれば積極的にフィードバック。最低限の要件や仕様をこの段階で確認しておく

---

<!-- _class: quotes -->

# 共通点 2 -- 検証ループを整える

**Boris Cherny:**

> Claudeに作業を **検証する方法を与えること** が最も重要。フィードバックループがあれば品質が2〜3倍

**Boris Tane:**

> 実装中は `continuously run typecheck` を指示する

**Hawkie:**

> 何も指示しないと正常系しかテストしない。CLAUDE.mdにTDDで進めるよう明記し、RED → GREEN → REFACTOR のサイクルを明示する

---

# その他 Tips の位置づけ

CLAUDE.md、Hooks、MCP、並列実行……
これらは生産性やコンテキスト管理の工夫。

**plan と 検証ループがあった上での話。**

> **plan と テスト（検証ループ）を意識するだけ** で開発品質は大きく変わる。

---

<!-- _class: divider -->

# 2. Kiroとは

---

# Kiro 概要

**AWS製のAIコーディングツール**（2025年7月リリース、同年GA）

|          | IDE                         | CLI                        |
| -------- | --------------------------- | -------------------------- |
| 形態     | VSCode フォーク             | ターミナル型エージェント   |
| 位置づけ | Cursor / Antigravity と同列 | Claude Code / Codex に近い |
| 前身     | --                          | Amazon Q Developer         |

IDEに **仕様駆動開発（Spec-Driven Development）が組み込まれている**。今回はこちらの話。

---

# Vibe モードと Spec モード

### Vibe モード

従来のAIコーディング。チャットで対話しながら書いてもらうスタイル。
プロトタイピングや小さなタスク向き。

### Spec モード（Kiro の最大の特徴）

アイデアを壁打ちして仕様を固め、その仕様に基づいて実装する。
複雑なフィーチャー開発やチーム開発向き。

---

<!-- _class: divider -->

# 3. 仕様駆動開発とは

---

# 仕様駆動開発の定義

コーディングの前に仕様を明文化し、**その仕様を正として開発する**手法。

- 要件定義 → 設計 → 実装 → テスト を段階的に進める
- 事前の仕様策定により、品質向上と一貫性確保を図る
- 規模の大小に関わらず有効

---

# Kiro 固有のものではない

仕様駆動開発は Kiro 以外のツールでも実践できる。

- **spec-kit** -- GitHub が公開している OSS ツールキット
- **cc-sdd** -- Claude Code で仕様駆動開発を始めるための OSS ツール

> 「仕様を先に書く」思想は TDD や BDD と通じる

---

<!-- _class: divider -->

# 4. Kiroにおける仕様駆動開発

---

# Steering（ステアリング）

`.kiro/steering/` に配置する Markdown ファイル群。
Spec に依らない **全体的な開発ルール** をここに書く。

| ファイル              | 役割                                     |
| --------------------- | ---------------------------------------- |
| `product.md`          | プロダクトの目的・ユーザー・ビジネス目標 |
| `tech.md`             | 使用技術スタック・ライブラリ・制約       |
| `structure.md`        | ファイル構成・命名規則・アーキテクチャ   |
| `code-conventions.md` | コードスタイル・命名パターン             |
| `tdd.md`              | テスト方針                               |

---

# Steering の読み込み制御

inclusion 設定で **3段階に制御** できる。

| 設定                       | 挙動                       |
| -------------------------- | -------------------------- |
| **常時読み込み**           | 毎回コンテキストに含まれる |
| **キーワード自動読み込み** | 関連する話題で自動ロード   |
| **手動参照**               | 明示的に参照したときだけ   |

コンテキスト節約と情報の適切な提供を両立させる仕組み。

---

# Spec の 3 ファイル構造

Spec モードで開発すると、Kiro は以下の 3 ファイルを生成する。

### `requirements.md`

ユーザーストーリーと受け入れ基準（EARS 記法）
`THE System SHALL allow authenticated users to view active listings.`

### `design.md`

システムアーキテクチャ、シーケンス図、実装方針

### `tasks.md`

実装タスクのリスト（依存関係順）。Kiro が実装しながら更新。

---

# Spec の種類

### Feature Spec（Requirements-First）

何を作るかは明確だが技術的な道筋が未決 → **要件 → 設計 → タスク**

### Feature Spec（Design-First）

技術アーキテクチャがすでに決まっている → **設計 → 要件逆算 → タスク**

### Bugfix Spec

バグ修正専用。3 セクション構成：
**現在の挙動** / **期待する挙動** / **変えてはいけない挙動**

---

<!-- _class: divider -->

# 5. 検証ループ

## プロパティベーステスト

---

# AI コード生成の問題

AI がコードを生成したとき、**そのコードが仕様通りかどうか** をどう確認するか。

従来のユニットテストは「この入力ならこの出力」という個別ケースの確認。
テストを書く側が思いつかなかったエッジケースは漏れる。

---

# プロパティベーステスト（PBT）

|                | ユニットテスト       | PBT                          |
| -------------- | -------------------- | ---------------------------- |
| **書き方**     | 入力A → 出力B を確認 | 任意の入力で性質が成り立つか |
| **ケース数**   | 手書きで用意した分   | 数百〜数千をランダム生成     |
| **得意な場面** | 特定ケースの確認     | 不変条件の検証               |

---

# PBT の例：交通信号シミュレーター

### 要件（EARS 記法）

`THE System SHALL ensure no two directions are green at the same time.`

### ユニットテスト

「北が青のとき、南は赤か」を 1 ケース確認

### PBT

ランダムな信号状態パターンを何千通りも生成し、
常にこの性質が成り立つか検証

---

# Kiro における PBT

GA版で追加された機能。

- EARS 記法の要件から自動的に **プロパティを抽出**
- ランダムなテストケースを **大量生成して実行**
- 反例が見つかったら、実装・テスト・仕様のどれを修正すべきか提示
- Bugfix Spec では「変えてはいけない挙動」も PBT で検証

> 仕様とコードの一致を **機械的に検証** する仕組み

---

<!-- _class: divider -->

# 6. 成功させるコツ

---

# 1 -- まず Steering を整備する

- Spec を書く前に、全体の開発ルールを Steering に入れておく
- 複数 Spec に跨る事項（技術・命名規則・テスト方針）は Steering が担う
- Steering が整っていれば各 Spec がシンプルになり、AI が一貫したルールで動く

---

# 2 -- Spec を大きくしすぎない（最重要）

- 要件が大きいと漏れやすい
- レビュー負荷が下がり、自分も AI も迷いにくくなる
- **独立したドメインに閉じた範囲で分割する**
  -- 機能を小さく切るというより、関心事を分離して依存しない単位にする

---

# 3 -- 用途に合わせた Spec を選ぶ

Feature Spec（Requirements-First / Design-First）、Bugfix Spec など
**用途に合った Spec を選ぶ**。

特にバグ修正には **Bugfix Spec** が有効。
「**変えてはいけない挙動**」を最初に明示する。

---

# 4 -- Plan 工程に強いモデルを使う

仕様策定フェーズが最も重要。**ここでモデルをケチらない。**

Opus と Sonnet では仕様の質に差が出る。
実装フェーズでも同様で、モデルの品質は常に結果に影響する。

---

# 5 -- コンテキストを節約する

Kiro は Steering や仕様ファイルを常時読み込むため、コンテキストが圧迫されやすい。

- Steering の内容を **厳選・精査**
- 特定ユースケース限定の情報は Skills に退避
- MCP を目的に合わせて選別
- **Powers を活用** -- MCP・Steering・Hooks を 1 パッケージにまとめ、
  キーワードに反応して必要なときだけ動的にロード。
  Figma / Supabase / Stripe 等のパートナー製 Power もある。

---

<!-- _class: divider -->

# 7. まとめ

---

# 持ち帰ってほしいこと

### 1. Kiro というツールの存在

VSCode フォークで、Spec-Driven Development が組み込まれた AWS 製 AI IDE。

### 2. 仕様駆動開発という考え方

コードを書く前に意図を明文化し、その仕様を正として開発する。Kiro 固有ではない。

### 3. 今日からできること

- Claude Code → 最初に **Plan モード** で計画させる
- 普段の Chat → 「**まず計画を立てて**」と一言加える
- 検証ループを作る → linter からでも OK

---

<!-- _class: refs -->

# 参考リンク

- [Kiro 公式](https://kiro.dev) / [Docs: Specs](https://kiro.dev/docs/specs/) / [Docs: Steering](https://kiro.dev/docs/steering/)
- [Kiro Powers](https://kiro.dev/powers/) / [Property-Based Testing](https://kiro.dev/blog/property-based-testing/)
- [Spec types（AWS Blog）](https://aws.amazon.com/jp/blogs/news/specs-bugfix-and-design-first/)
- [Boris Cherny 氏の記事（翻訳）](https://zenn.dev/mohy_nyapan/articles/a07975837386f7)
- [Boris Tane 氏の記事](https://boristane.com/blog/how-i-use-claude-code/)
- [ごく個人的な Claude Code プラクティス集](https://zenn.dev/hawkymisc/articles/2cace1f599cc06)
- [spec-kit（GitHub OSS）](https://github.com/github/spec-kit) / [cc-sdd](https://github.com/gotalab/cc-sdd)
- [実例リポジトリ](https://github.com/kawaaaas/serverless-spa-construct-test)
