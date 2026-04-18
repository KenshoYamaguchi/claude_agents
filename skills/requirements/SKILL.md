---
name: requirements
description: 要件定義テンプレートを現在のプロジェクトに展開するスキル。PRD・技術スタック比較表・ER図のMarkdownテンプレートを docs/ に生成する。Use when you need to manually create requirement documents.
argument-hint: [システム名]
---

# 要件定義テンプレート展開

以下のファイルを `docs/` に生成してください。

## 生成するファイル

### `docs/PRD.md`

```markdown
# PRD: $ARGUMENTS

> 作成日: [日付]  
> ステータス: Draft

## 1. 概要

### 目的
<!-- このシステムが解決する課題 -->

### 対象ユーザー
| ユーザー種別 | 説明 | 規模感 |
|------------|------|--------|
| エンドユーザー | ... | 〜XXX人 |
| 管理者 | ... | XXX人 |

### スコープ
**対象:** ...  
**対象外:** ...

## 2. 機能要件

### MVP（必須機能）
- [ ] 機能1: 説明
- [ ] 機能2: 説明

### Phase 2（将来機能）
- [ ] 機能3: 説明

## 3. 非機能要件

| 項目 | 要件 | 備考 |
|------|------|------|
| レスポンスタイム | 95%ile < 500ms | ... |
| 同時接続数 | XXX ユーザー | ... |
| 可用性 | 99.9% | ... |
| セキュリティ | 認証方式: JWT | ... |
| データ保持期間 | XXX年 | ... |

## 4. 制約・前提条件

- 予算: ...
- 期間: ...
- 既存システムとの連携: ...
- 使用言語/フレームワークの制約: ...

## 5. 成功指標（KPI）

| 指標 | 目標値 | 計測方法 |
|------|--------|---------|
| ... | ... | ... |

## 6. リスク

| リスク | 影響度 | 対策 |
|--------|--------|------|
| ... | 高/中/低 | ... |
```

### `docs/TECH_STACK.md`

```markdown
# 技術スタック提案: $ARGUMENTS

## 候補比較

### バックエンド

| | 候補A | 候補B | 候補C |
|--|-------|-------|-------|
| 言語 | Go | Python | TypeScript |
| FW | Gin | FastAPI | NestJS |
| 学習コスト | 中 | 低 | 低 |
| パフォーマンス | ◎ | ○ | ○ |
| エコシステム | ○ | ◎ | ○ |
| 採用のしやすさ | ○ | ◎ | ○ |

### フロントエンド

| | 候補A | 候補B |
|--|-------|-------|
| FW | Next.js | Nuxt 3 |
| 言語 | TypeScript | TypeScript |
| レンダリング | SSR/SSG | SSR/SSG |

### インフラ

| | 候補A | 候補B |
|--|-------|-------|
| ホスティング | Railway | Fly.io |
| DB | PostgreSQL | MySQL |
| ストレージ | S3互換 | ... |

## 推奨案

**バックエンド:** [選択] — [理由]  
**フロントエンド:** [選択] — [理由]  
**DB:** [選択] — [理由]  
**ホスティング:** [選択] — [理由]
```

### `docs/ER_DIAGRAM.md`

```markdown
# データモデル: $ARGUMENTS

## エンティティ関係図

\`\`\`mermaid
erDiagram
    USER {
        bigint id PK
        varchar email UK
        varchar name
        timestamp created_at
        timestamp updated_at
    }

    %% エンティティを追加してください
\`\`\`

## テーブル定義

### users
| カラム | 型 | 制約 | 説明 |
|--------|-----|------|------|
| id | BIGINT | PK, AUTO | ... |
| email | VARCHAR(255) | NOT NULL, UNIQUE | ... |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | ... |
```
