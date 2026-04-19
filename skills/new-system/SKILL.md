---
name: new-system
description: 新しいシステム開発を最初から始めるときのマスタースキル。プラン立案→レビュー→再立案→人間チェック→実装→GitHub設定→デプロイの順でエージェントを使う手順を案内する。Use when starting development of a new system from scratch.
argument-hint: [システム名]
---

# 新規システム開発ガイド: $ARGUMENTS

以下の順番で進めます。**人間によるゲートチェック（Phase 4）を必ず挟みます。**

```
Phase 1: システム設計プラン立案    → @system-planner
Phase 2: エージェントレビュー      → @system-reviewer
Phase 3: プラン再立案              → @system-planner（修正版）
Phase 4: ★ 人間による最終チェック ★
Phase 5: 実装                     → @system-coder
Phase 6: GitHub設定               → @github-manager
Phase 7: デプロイ                 → @deploy-manager
```

---

## Phase 1: システム設計プラン立案

```
@system-planner $ARGUMENTSのシステム設計プランを立案してください
```

**完了の目安:**
- `docs/system_plan_$ARGUMENTS.md` が作成されている（ステータス: Draft）
- 要件・アーキテクチャ・キャッシュ設計・DB構成・計算基盤配置が記載されている

---

## Phase 2: エージェントレビュー

プランが作成されたら **すぐに** レビューへ（人間は待機）:

```
@system-reviewer docs/system_plan_*.md をレビューしてください
```

**完了の目安:**
- `docs/system_review_*.md` が作成されている
- 総合判定が明記されている

---

## Phase 3: プラン再立案

レビューフィードバックを反映:

```
@system-planner docs/system_review_*.md を読んでプランを修正してください
```

**完了の目安:**
- プランがバージョンアップされている（v1 → v2）
- 必須修正事項が全て対応されている

---

## Phase 4: ★ 人間による最終チェック ★

**ここで必ず立ち止まって確認してください。**

```markdown
## チェックリスト

### アーキテクチャ
- [ ] キャッシュ設計が自分の用途に合っているか
- [ ] DB構成（OLTP/OLAP/KVS）の選定に納得できるか
- [ ] 重い処理の配置（同期/非同期/バッチ）が適切か
- [ ] 単一障害点がないか

### 要件・スコープ
- [ ] MVPの範囲が適切か（大きすぎ/小さすぎないか）
- [ ] 技術スタックに納得できるか
- [ ] コスト見積もりが現実的か

### セキュリティ
- [ ] 認証・認可の設計に問題がないか
- [ ] 機密データの扱いが適切か
```

✅ OKなら `docs/system_plan_*.md` のステータスを **"Approved"** に変更してから次へ。
❌ 問題があれば @system-planner に追加フィードバックを伝えて再立案。

---

## Phase 5: 実装

**Approved になってから** 依頼:

```
@system-coder docs/system_plan_*.md に基づいて実装してください
```

**完了の目安:**
- `backend/` `frontend/` `infra/` の雛形が生成されている
- `docker-compose up` でローカルが起動する
- ヘルスチェックエンドポイントが応答する

---

## Phase 6: GitHub設定

```
@github-manager このプロジェクトのGitHub設定を行ってください
```

**完了の目安:**
- `.github/workflows/ci.yml` が作成されている
- ブランチ保護ルールが設定されている
- PRテンプレートが作成されている

---

## Phase 7: デプロイ

```
@deploy-manager ステージング環境へのデプロイを実施してください
```

---

## 全体の成果物チェックリスト

```
docs/
├── system_plan_*.md        ← 承認済みシステム設計プラン
├── system_review_*.md      ← エージェントレビュー結果
└── API_SPEC.md             ← API仕様書（system-coderが生成）

backend/                    ← バックエンドコード
frontend/                   ← フロントエンドコード
infra/
├── docker-compose.yml
└── scripts/

.github/
└── workflows/
    ├── ci.yml
    └── deploy.yml
```
