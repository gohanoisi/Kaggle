# Spaceship Titanic Kaggle コンペ ─ 現状概要（2025‑07‑21）

このドキュメントは “社長の友人エンジニア” へ向けた一括共有用メモです。チーム内の経緯・データ状況・試行錯誤の履歴を把握できるよう、要点をまとめています。

---

## 1. コンペ基本情報

| 項目   | 内容                                                                                                                  |
| ---- | ------------------------------------------------------------------------------------------------------------------- |
| コンペ  | [Spaceship Titanic – Predict if passengers were transported](https://www.kaggle.com/competitions/spaceship-titanic) |
| 目的変数 | `Transported` (True/False)                                                                                          |
| 評価指標 |                                                                                                                     |

| **Accuracy** (公開 LB / Private LB) |                                                                                                                |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| データ                               | `train.csv` (8,681 行), `test.csv` (4,277 行), 13 列                                                              |
| 主な列                               | `PassengerId`, `HomePlanet`, `CryoSleep`, `Cabin`, `Destination`, `Age`, `VIP`, `RoomService`～`VRDeck`, `Name` |
| 注記                                | `PassengerId` 先頭 4 桁が同一 “Group” を表す — CV 分割時に要注意                                                               |

---

## 2. 現在のディレクトリ構成

```
project_root/
├ data/  … 原始 CSV
├ notebook/  … 開発 Notebooks (#セル番号付)
│   ├ spaceship_titanic_001.ipynb  # baseline
│   ├ spaceship_titanic_002.ipynb  # FE + LGBM tuned
│   ├ spaceship_titanic_003.ipynb  # CatBoost GPU (best LB)
│   └ spaceship_titanic_004.ipynb  # XGBoost GPU (ver4)
├ outputs/
│   ├ leaderboard/  # 公開 LB スナップショット
│   └ submissions/
│       ├ ver1/  baseline 0.79097
│       ├ ver2/  LGBM tuned 0.80126
│       ├ ver3/  CatBoost GPU 0.80266 ★現状最高
│       └ ver4/  XGB GPU 0.79752 (設定ミス)
└ docs/  … 要件定義・進捗管理（Canvas 同期）
```

---

## 3. これまで試した主なアイデア

### 3.1 前処理・特徴量

- **Cabin** → `Deck` / `CabinNum` (整数) / `Side` に split
- **PassengerId** → `GroupID` + `GroupSize`
- 金額 5 列合計 → `TotalSpending` & `LogTotalSpending`
- 年齢ビン (`AgeBand`)、`HomePlanet × Destination` → `PlanetRoute`
- `SpendingPerAge`, `SpendingPerGroup` 比率
- **Target Encoding**：`HomePlanet`, `Destination`, `PlanetRoute`

### 3.2 モデル & スコア

| Exp | モデル                 | 特徴量          | CV Acc       | Public LB   |
| --- | ------------------- | ------------ | ------------ | ----------- |
| 01  | LightGBM baseline   | One‑Hot only | 0.7945       | 0.79097     |
| 02  | LGBM + FE           | 上記 FE        | 0.8101       | –           |
| 03  | LGBM tuned (Optuna) | 同上           | 0.8113       | 0.80126     |
| 04  | **CatBoost (GPU)**  | + TargetEnc  | **0.8164**   | **0.80266** |
| 05  | XGBoost (GPU)       | + FE         | \~0.804 (推定) | 0.79752\*   |

> \*XGB ver4 は `tree_method` 設定誤りで GPU 利用できず性能低下。

---

## 4. ボトルネック・課題

1. **CV vs LB ギャップ**：SGKFold 未適用の過去実験では +0.01 程度の過大評価。
2. **0.82 の壁**：上位勢は 0.82〜、トップは 0.96～1.00（リーク無し前提で “最適解” がある？）
3. **特徴量過多による過学習**：CatBoost で重要度が低い列が多く、削減余地あり。

---

## 5. 今後のアプローチ案

1. **StratifiedGroupKFold** で LGBM を再学習 → ギャップ解析（exp\_06）
2. **Auto Feature Selection**：BorutaPy / SHAP → 上位特徴量だけで再学習（exp\_08）
3. **Ensemble (Stacking)**：CatBoost + LGBM + XGB OOF → LR\_meta、閾値最適化（exp\_07）
4. **Hard Cases Switch**：OOF 0.45–0.55 に別モデル（k‑NN, TabNet）で置換
5. **Pseudo‑Labeling**：CatBoost 0.82 相当モデルで test 高確率予測を再学習

---

## 6. 現在求めている知見 / アドバイス

1. **0.82+ へ跳ねる特徴量や前処理** のヒント
2. **AutoML 的に最適特徴量サブセット** を選ぶライブラリ・ワークフロー
3. CatBoost / XGBoost **GPU チューニングの定石**（学習率と depth の適切レンジ）
4. **閾値最適化**（Accuracy 最大化）での落とし穴
5. 上位勢が採用しがちな **特殊エンコーディング**・**擬似ラベル戦略** の事例

---

> 以上が現時点の全体像です。フィードバックやアイデアをいただけると大変助かります！

