---
name: github-workflow
description: GitHubでの開発フローの手順書。PR作成・レビュー・マージ・リリースの標準的な進め方を案内する。Use when you need guidance on the GitHub development workflow.
---

# GitHub 開発ワークフロー

## 日常の開発フロー

### 1. 作業開始
```bash
# mainを最新にしてからブランチを切る
git checkout main
git pull origin main
git checkout -b feature/[機能名]
```

**ブランチ命名規則:**
- `feature/[機能名]` — 新機能
- `fix/[バグ内容]` — バグ修正
- `chore/[内容]` — 設定・依存関係
- `docs/[内容]` — ドキュメント

### 2. コミット
```bash
git add [ファイル]        # ファイルを個別に指定（git add . は使わない）
git commit -m "[type]: [概要]"
```

**コミットメッセージの type:**
- `feat:` 新機能
- `fix:` バグ修正
- `refactor:` リファクタリング
- `docs:` ドキュメント
- `chore:` 設定・依存関係
- `test:` テスト
- `perf:` パフォーマンス改善

### 3. PR作成
```bash
git push origin feature/[機能名]
gh pr create --title "[type]: [概要]" --body "$(cat .github/PULL_REQUEST_TEMPLATE.md)"
```

### 4. レビュー依頼・対応
```bash
# レビュアーを指定
gh pr edit [PR番号] --add-reviewer [GitHubユーザー名]

# レビューコメントを確認
gh pr view [PR番号] --comments

# 修正後に再プッシュ（コミットを追加）
git add [ファイル]
git commit -m "fix: レビュー指摘対応"
git push origin feature/[機能名]
```

### 5. マージ
```bash
# Squash merge を推奨（main のログをきれいに保つ）
gh pr merge [PR番号] --squash --delete-branch
```

---

## リリースフロー

```bash
# バージョンタグを打つ
git tag -a v1.0.0 -m "Release v1.0.0"
git push origin v1.0.0

# GitHub Release を作成
gh release create v1.0.0 --title "v1.0.0" --notes "## 変更内容\n- ..."
```

---

## よく使う gh コマンド

```bash
# PR一覧
gh pr list

# 自分のPRを確認
gh pr status

# CIの状況確認
gh run list --limit 10

# Issue作成
gh issue create --title "[タイトル]" --label "bug"

# リポジトリをブラウザで開く
gh repo view --web
```

---

## Secrets の設定

```bash
# Secretを設定
gh secret set DATABASE_URL

# Secret一覧
gh secret list
```

**絶対にコードに書かないもの:**
- APIキー、トークン
- DB接続文字列
- パスワード

→ 全て GitHub Secrets または `.env`（.gitignore済み）で管理
