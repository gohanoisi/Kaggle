# Spaceship Titanic プロジェクト ─ 進捗管理表

## 1. プロジェクト概況（2025-07-16 現在）
| 指標 | 値 | 備考 |
|---|---|---|
| 最新 CV Accuracy | **0.8164** | CatBoost GPU + TargetEnc（exp_04） |
| Public LB Accuracy | **0.80266** | `ver3/soya_model3.csv` |
| 短期目標 | **0.8200 以上** | XGB + Ensemble & AutoFE で到達を狙う |

---

## 2. 実験ログ
| Exp ID | モデル | 特徴量セット | CV Acc | LB Acc | 提出CSV | 備考 |
|---|---|---|---|---|---|---|
| exp_01 | LightGBM baseline | baseline | 0.7945 | 0.79097 | ver1/soya_model1.csv | 部下1 |
| exp_02 | LightGBM + FE | Cabin/Group/Spend | 0.8101 | — | — | 部下2 |
| exp_03 | LightGBM tuned | +Optuna | 0.8113 | 0.80126 | ver2/soya_model2.csv | 部下2 |
| exp_04 | CatBoost GPU | +TargetEnc | 0.8164 | 0.80266 | ver3/soya_model3.csv | 部下3 |
| exp_05 | XGBoost GPU | +FE | ⏳ | ⏳ | (ver4) | 部下4 進行中 |
| exp_06 | LGBM + SGKFold | tuned params | ⏳ | ⏳ | (ver5) | CV 設計検証 |
| exp_07 | Stacking Ensemble | LGBM+Cat+XGB | ⏳ | ⏳ | (ver6) | 最終候補 |
| exp_08 | Auto Feature Select | Boruta/SHAP | ⏳ | ⏳ | — | 特徴量選択試験 |

---

## 3. タスクボード
| 状態 | タスク | 担当 | 期限 | メモ |
|---|---|---|---|---|
| ✅ 完了 | Cabin & GroupSize FE | 部下2 | 7/15 | exp_02 |
| ✅ 完了 | Optuna tuning (LGBM) | 部下2 | 7/15 | exp_03 |
| ✅ 完了 | CatBoost GPU 実装 | 部下3 | 7/16 | exp_04 |
| 🟡 進行中 | XGBoost GPU 実装 | 部下4 | **7/17** | exp_05 |
| 🟡 進行中 | LGBM + SGKFold 検証 | 部下4 | **7/17** | exp_06 |
| ⏳ 未着手 | Auto Feature Selection (Boruta/SHAP) | 部下4 | **7/19** | exp_08 |
| ⏳ 未着手 | Stacking / Blending Ensemble | 部下4 | **7/20** | exp_07 |
| ⏳ 未着手 | 高難度サンプル誤分類対策 | 部下4 | **7/22** | hard cases switch |
| ⏳ 未着手 | ドキュメント整理 & 最終提出 | 全員 | 7/28 | — |

---

## 4. リスク & 対応策（更新）
| リスク | 影響 | 優先度 | 対応策 |
|---|---|---|---|
| CV-LB ギャップ | 本番精度不足 | 高 | SGKFold, 外れ Fold 分析, hard cases switch |
| 過学習 | 汎化低下 | 中 | EarlyStopping, Feature Pruning, Ensemble |
| データ & FE 過多 | 学習コスト増 | 中 | AutoFE で重要特徴量抽出、冗長削除 |
| GPU 資源不足 | 学習停滞 | 中 | Colab Pro / ローカル RTX4070Ti 併用 |
| MLflow ログ漏れ | 再現不可 | 中 | Run テンプレ必須、CI でチェック |

---

## 5. 次回レビュー予定
- **日時**: 2025-07-17 18:00 JST
- **議題**:
  1. exp_05 (XGBoost GPU) 結果 & LB スコア
  2. exp_06 (SGKFold LGBM) ギャップ解析
  3. Auto Feature Selection 計画確認
  4. Ensemble 設計確定

---
> 更新履歴
> - 07/14: 初版
> - 07/15 AM: exp_02, exp_03 追加
> - 07/15 PM: 実験ログ詳細化
> - 07/16: exp_04 結果反映・タスク更新（社長）

