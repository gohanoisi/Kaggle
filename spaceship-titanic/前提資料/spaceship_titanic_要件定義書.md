# Spaceship Titanic プロジェクト ─ 進捗管理表

## 1. プロジェクト概況（2025-07-15 現在）
| 指標 | 値 | 備考 |
|---|---|---|
| 最新 CV Accuracy | **0.8113** | LightGBM + Optuna（exp_03） |
| Public LB Accuracy | **0.80126** | `sub_tuned_202507150930.csv` |
| 短期目標 | **0.8200 以上** | Ensemble & CV 改善で到達を狙う |

---

## 2. 実験ログ
| Exp ID | モデル | 特徴量セット | CV Acc | LB Acc | 提出CSV | MLflow Run | 備考 |
|---|---|---|---|---|---|---|---|
| exp_01 | LightGBM baseline | baseline | 0.7945 | 0.79097 | outputs/submissions/202507140809/sub_baseline_soya.csv | ✅ | 部下1 |
| exp_02 | LightGBM + FE | Cabin/Group/Spend | 0.8101 | — | — | ✅ | 部下2, 提出なし |
| exp_03 | LightGBM tuned | exp_02 + Optuna | 0.8113 | 0.80126 | outputs/submissions/202507150930/sub_tuned_202507150930.csv | ✅ | 部下2 |
| exp_04 | CatBoost GPU | +FE | ⏳ | ⏳ | — | ⏳ | 新部下 |
| exp_05 | XGBoost GPU | +FE | ⏳ | ⏳ | — | ⏳ | 新部下 |
| exp_06 | StratifiedGroupKFold | LGBM tuned | ⏳ | ⏳ | — | ⏳ | CV 設計検証 |
| exp_07 | Stacking Ensemble | LGBM + Cat + XGB | ⏳ | ⏳ | — | ⏳ | 最終候補 |

---

## 3. タスクボード
| 状態 | タスク | 担当 | 期日 | メモ |
|---|---|---|---|---|
| ✅ Done | Cabin & GroupSize 特徴量 | 部下2 | 7/15 | exp_02 |
| ✅ Done | Optuna チューニング | 部下2 | 7/15 | exp_03 |
| 🟡 In-Progress | CatBoost GPU 実装・CV | 新部下 | 7/17 | exp_04 |
| 🟡 In-Progress | XGBoost GPU 実装・CV | 新部下 | 7/17 | exp_05 |
| 🟡 In-Progress | StratifiedGroupKFold で LGBM 再検証 | 新部下 | 7/18 | exp_06 |
| ⏳ To-Do | Stacking / Blending Ensemble | 新部下 | 7/20 | exp_07 |
| ⏳ To-Do | 追加特徴量（TE, 相互作用） | 新部下 | 7/22 | exp_08 |
| ⏳ To-Do | ドキュメント整理 & 最終提出 | 全員 | 7/28 | — |

---

## 4. リスク & 対応策（更新）
| リスク | 影響 | 優先度 | 対応策 | 担当 |
|---|---|---|---|---|
| CV-LB ギャップ | 精度停滞 | 高 | StratifiedGroupKFold, 外れ Fold 検証 | 新部下 |
| 過学習 | 本番精度低下 | 中 | EarlyStopping, Feature pruning | 新部下 |
| GPU 資源制限 | 学習停止 | 中 | Colab Pro or ローカル GPU 併用 | 各自 |
| MLflow ログ忘れ | 再現不可 | 中 | run テンプレ遵守 | 各自 |

---

## 5. 次回レビュー予定
- **日時**: 2025-07-17 18:00 (JST)
- **トピック**:
  1. CatBoost / XGBoost CV & LB 結果（exp_04, exp_05）
  2. StratifiedGroupKFold 検証結果（exp_06）
  3. Ensemble 方針と提出戦略

---
> 更新履歴
> - 2025-07-14: 初版
> - 2025-07-15: exp_02, exp_03 追加
> - 2025-07-15 PM: 実験ログ詳細化、タスク・期限更新

