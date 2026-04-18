---
name: analysis-qa
description: 分析コードの品質チェックリスト。analysis-coder実装後、人間がレビューするときに使う。再現性・統計的妥当性・コード品質を確認する。Use before finalizing analysis code or creating a PR.
---

# 分析コード QAチェックリスト

## 1. 再現性

```bash
# クリーンな環境で再実行できるか確認
pip install -r requirements.txt
python analysis/00_data_validation.py
python -m pytest analysis/tests/ -v
```

- [ ] `requirements.txt` にバージョンが固定されている（`==` で指定）
- [ ] 乱数シードが全箇所で固定されている（`random.seed(42)`, `np.random.seed(42)`, `random_state=42`）
- [ ] ハードコードされたパスがない（`Path(__file__).parent` 等を使用）
- [ ] 環境変数で外部依存を管理している（DBのURL等）
- [ ] README に再現手順が書かれている

## 2. データ品質

```bash
python analysis/00_data_validation.py
```

- [ ] データのshape・型・欠損率が期待通りか確認している
- [ ] 前処理後のデータが処理前より小さくなった場合に理由が記録されている
- [ ] 訓練データとテストデータのリークがない
- [ ] 時系列データの場合、未来情報を使っていない

## 3. 統計的妥当性

- [ ] 分析プランで決めた手法を忠実に実装している
- [ ] 前提条件（正規性・独立性等）をコード内で確認している
- [ ] 信頼区間またはp値を計算・報告している
- [ ] 多重比較を行った場合、補正している（Bonferroni / BH法等）
- [ ] サンプルサイズの根拠がある（検出力計算 等）
- [ ] 相関を因果として記述していない

## 4. コード品質

```bash
ruff check analysis/ app/
mypy analysis/utils/ --ignore-missing-imports
```

- [ ] Lintエラーがない
- [ ] 関数の型ヒントがある
- [ ] 重複コードが関数化されている
- [ ] 使われていない変数・import がない
- [ ] マジックナンバーが定数化されている

## 5. テスト

```bash
python -m pytest analysis/tests/ -v --tb=short
```

- [ ] 前処理関数に単体テストがある（正常系・境界値・欠損あり）
- [ ] サンプルデータで全パイプラインが動作する
- [ ] エラーが適切にハンドリングされている（データ不足・型エラー等）

## 6. 可視化

- [ ] 全グラフに軸ラベル・タイトルがある
- [ ] グラフに単位が明記されている
- [ ] カラースキームが色覚多様性に配慮している（`viridis`, `colorblind` palette等）
- [ ] 図が `outputs/figures/` に保存されている
- [ ] 図のファイル名が内容を表している（`fig1.png` ではなく `sales_trend_2024.png`）

## 7. レポート (`docs/REPORT.md`)

- [ ] エグゼクティブサマリーが非専門家にも読める
- [ ] 主要な発見が数値で具体的に記述されている
- [ ] 限界・注意点が正直に書かれている
- [ ] 再現手順が正確に記載されている
- [ ] git commit hash が記録されている

## 8. セキュリティ・プライバシー

- [ ] 個人情報・機密データが `data/raw/` に隔離されており `.gitignore` 済み
- [ ] APIキー・パスワードがコードにハードコードされていない
- [ ] ノートブックのアウトプットが `nbstripout` でクリアされている
- [ ] `git log --all -- data/` で生データがコミット履歴にないか確認

```bash
# コミット済みの機密データ確認
git log --all -- "*.csv" "*.parquet" "*.pkl"
# 大きいファイルの確認
git rev-list --objects --all | git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' | sort -k3 -n | tail -20
```

---

## 最終コマンド集

```bash
# 全チェックを一括実行
ruff check analysis/ app/ && \
mypy analysis/utils/ --ignore-missing-imports && \
python -m pytest analysis/tests/ -v && \
python analysis/00_data_validation.py --data data/sample/ && \
echo "✅ 全チェック通過"
```
