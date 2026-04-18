---
name: new-system
description: 新しいシステム開発を最初から始めるときのマスタースキル。要件定義→バックエンド→フロントエンド→GitHub設定→デプロイの順でエージェントを使う手順を案内する。Use when starting development of a new system from scratch.
argument-hint: [システム名]
---

# 新規システム開発ガイド

$ARGUMENTSの開発を以下の順番で進めます。

## フェーズ概要

```
Phase 1: 要件定義      → @requirements-analyst
Phase 2: バックエンド  → @backend-architect
Phase 3: フロントエンド → @frontend-architect
Phase 4: GitHub設定    → @github-manager
Phase 5: デプロイ      → @deploy-manager
```

---

## Phase 1: 要件定義

まず `@requirements-analyst` に依頼してください:

```
@requirements-analyst $ARGUMENTSの要件定義を行ってください
```

**完了の目安:**
- `docs/PRD.md` が作成されている
- `docs/TECH_STACK.md` が作成されている
- `docs/ER_DIAGRAM.md` が作成されている

---

## Phase 2: バックエンド構成

PRD承認後、`@backend-architect` に依頼:

```
@backend-architect docs/PRD.md を読んでバックエンドを構成してください
```

**完了の目安:**
- `backend/` ディレクトリが作成されている
- `docs/API_SPEC.md` が作成されている
- `docs/DB_SCHEMA.md` が作成されている
- `docker-compose.yml` が作成されている

---

## Phase 3: フロントエンド構成

バックエンド構成後、`@frontend-architect` に依頼:

```
@frontend-architect docs/API_SPEC.md を読んでフロントエンドを構成してください
```

**完了の目安:**
- `frontend/` ディレクトリが作成されている
- `docs/COMPONENT_DESIGN.md` が作成されている

---

## Phase 4: GitHub管理設定

コード雛形完成後、`@github-manager` に依頼:

```
@github-manager GitHubリポジトリの設定・CI/CD・テンプレートを整備してください
```

**完了の目安:**
- `.github/workflows/ci.yml` が作成されている
- `.github/workflows/deploy.yml` が作成されている
- `.github/PULL_REQUEST_TEMPLATE.md` が作成されている
- ブランチ保護ルールが設定されている

---

## Phase 5: デプロイ

GitHub設定完了後、`@deploy-manager` に依頼:

```
@deploy-manager ステージング環境へのデプロイを実施してください
```

**完了の目安:**
- ステージング環境が動作している
- ヘルスチェックが通っている
- `docs/DEPLOY_LOG.md` に記録されている

---

## 全体の成果物チェックリスト

```
docs/
├── PRD.md              ← 要件定義書
├── TECH_STACK.md       ← 技術スタック
├── ER_DIAGRAM.md       ← ER図
├── API_SPEC.md         ← API仕様書
├── DB_SCHEMA.md        ← DB設計
├── COMPONENT_DESIGN.md ← コンポーネント設計
└── DEPLOY_LOG.md       ← デプロイ履歴

backend/                ← バックエンドコード
frontend/               ← フロントエンドコード
docker-compose.yml      ← ローカル開発環境
.github/
├── workflows/
│   ├── ci.yml
│   └── deploy.yml
└── PULL_REQUEST_TEMPLATE.md
```
