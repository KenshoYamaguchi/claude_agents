---
name: new-analysis
description: データ分析プロジェクトをゼロから始めるときのマスタースキル。分析プラン立案→レビュー→再立案→人間チェック→実装→インターフェースの全フローを案内する。Use when starting a new data analysis project.
argument-hint: [分析テーマ]
---

# データ分析プロジェクト: $ARGUMENTS

以下の順番で進めます。**人間によるゲートチェック（Phase 4）を必ず挟みます。**

```
Phase 1: 分析プラン立案        → @analysis-planner
Phase 2: エージェントレビュー  → @analysis-reviewer
Phase 3: 分析プラン再立案      → @analysis-planner（修正版）
Phase 4: ★ 人間による最終チェック ★
Phase 5: 分析コード実装        → @analysis-coder
Phase 6: GitHub設定            → @data-github-manager
Phase 7: インターフェース実装  → @viz-builder（任意）
```

---

## Phase 1: 分析プラン立案

```
@analysis-planner $ARGUMENTSの分析プランを立案してください
```

**完了の目安:**
- `docs/ANALYSIS_PLAN.md` が作成されている（ステータス: Draft）

---

## Phase 2: エージェントレビュー

プランが作成されたら **すぐに** レビューへ（人間は待機）:

```
@analysis-reviewer docs/ANALYSIS_PLAN.md をレビューしてください
```

**完了の目安:**
- `docs/REVIEW_FEEDBACK.md` が作成されている
- 総合判定が明記されている

---

## Phase 3: 分析プラン再立案

レビューフィードバックを反映:

```
@analysis-planner docs/REVIEW_FEEDBACK.md を読んで分析プランを修正してください
```

**完了の目安:**
- `docs/ANALYSIS_PLAN.md` がバージョンアップされている（v1 → v2）
- 必須修正事項が全て対応されている

---

## Phase 4: ★ 人間による最終チェック ★

**ここで必ず立ち止まって確認してください。**

確認する内容:
```markdown
## チェックリスト

### 問い・目的
- [ ] 分析の問いが自分の解きたい課題と一致しているか
- [ ] 答えが出た後の活用イメージが持てるか

### データ・手法
- [ ] 使用データに問題がないか（個人情報・ライセンス等）
- [ ] 分析手法に納得できるか
- [ ] 見落としている重要な変数・観点がないか

### アウトプット
- [ ] レポート・可視化の形式が用途に合っているか
- [ ] ダッシュボード等のインターフェースが必要か

### スコープ・リソース
- [ ] 期間・工数の見積もりは妥当か
- [ ] スコープが大きすぎないか（MVP思考）
```

✅ OKなら `docs/ANALYSIS_PLAN.md` のステータスを **"Approved"** に変更してから次へ:
```bash
# ステータスを手動で更新
# ステータス: Draft → Approved
```

❌ 問題があればフィードバックを @analysis-planner に伝えて再立案。

---

## Phase 5: 分析コード実装

**Approved になってから** 依頼:

```
@analysis-coder docs/ANALYSIS_PLAN.md に基づいて分析を実装してください
```

**完了の目安:**
- `analysis/` 以下にコードが実装されている
- `python -m pytest analysis/tests/` が通る
- `docs/REPORT.md` が生成されている

---

## Phase 6: GitHub設定

```
@data-github-manager このデータ分析プロジェクトのGitHub設定を行ってください
```

**完了の目安:**
- `.gitignore` にデータファイルが除外されている
- nbstripout が設定されている
- `.github/workflows/analysis-ci.yml` が作成されている

---

## Phase 7: インターフェース実装（任意）

ダッシュボードや共有可能なUIが必要な場合:

```
@viz-builder 分析結果を Streamlit でインタラクティブに可視化してください
```

---

## 全体の成果物チェックリスト

```
docs/
├── ANALYSIS_PLAN.md      ← 承認済み分析プラン
├── REVIEW_FEEDBACK.md    ← エージェントレビュー結果
└── REPORT.md             ← 最終分析レポート（生成物）

data/
├── raw/                  ← 生データ（.gitignore済み）
├── processed/            ← 前処理済み（.gitignore済み）
└── sample/               ← サンプルデータ（git管理）

analysis/
├── 00_data_validation.py
├── 01_eda.ipynb
├── 02_preprocessing.py
├── 03_analysis.ipynb
├── 04_visualization.py
├── utils/
└── tests/

app/                      ← インターフェース（任意）
├── app.py
└── ...

outputs/
├── figures/
└── tables/

.github/
└── workflows/
    ├── analysis-ci.yml
    └── app-deploy.yml    ← インターフェースある場合
```
