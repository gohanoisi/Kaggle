# Spaceship Titanic プロジェクト ─ 進捗管理表

## 1. プロジェクト概況（2025‑07‑14 現在）
| 指標 | 値 | 備考 |
|---|---|---|
| 最新 CV Accuracy | **0.7945** | LightGBM ベースライン（5‑Fold） |
| Public LB Accuracy | **0.79097** | 提出ファイル: `sub_baseline.csv` |
| 目標 | **0.8000 以上** | Week 1 で達成目標 |

---

## 2. 実験ログ（抜粋）
| Exp ID | モデル | 特徴量セット | CV Acc | LB Acc | MLflow Run | 提出 | メモ |
|---|---|---|---|---|---|---|---|
| exp_01 | LightGBM (100/0.1) | baseline | 0.7945 | 0.79097 | ✅ | ✅ | n_estimators=100 |
| exp_02 | — | — | — | — | (予定) | | Cabin 分解予定 |

> ※ 全実験は MLflow experiment `spaceship_titanic` に記録。

---

## 3. タスクボード
| 状態 | タスク | 担当 | 期日 | 備考 |
|---|---|---|---|---|
| ✅ Done | ベースライン Notebook 作成 | 旧部下 | 7/14 | exp_01 完了 |
| 🟡 In‑Progress | Cabin 分解／GroupSize 特徴量追加 | 新部下 | 7/16 | exp_02 |
| ⏳ To‑Do | Optuna で LGBM ハイパーチューニング | 新部下 | 7/18 | exp_03 |
| ⏳ To‑Do | CatBoost/XGBoost 試験 | 新部下 | 7/20 | exp_04 |
| ⏳ To‑Do | Stacking Ensemble | 新部下 | 7/24 | exp_05 |
| ⏳ To‑Do | ドキュメント整備 & 最終提出 | 全員 | 7/28 | — |

---

## 4. リスク & 対応策（更新）
| リスク | 影響 | 優先度 | 対応策 | 担当 |
|---|---|---|---|---|
| 精度停滞 (<0.80) | 目標未達 | 高 | 特徴量探索を優先し、Optuna で広く探索 | 新部下 |
| Colab 制限 | GPU 切断 | 中 | `save_model()` 後に Git push | 各自 |
| MLflow ログ漏れ | 再現不可 | 中 | `with mlflow.start_run()` をテンプレ化 | 新部下 |

---

## 5. 次回レビュー予定
- **日時**: 2025‑07‑16 18:00 (JST)
- **内容**:
  1. exp_02（Cabin/GroupSize 特徴量）の CV・LB 報告
  2. Optuna 探索パラメータの確認
  3. CatBoost/XGBoost 実装計画

---

> 更新履歴
> - 2025‑07‑14: ドキュメント初版作成（社長）

