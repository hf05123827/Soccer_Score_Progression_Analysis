# Soccer_Score_Progression_Analysis

　サッカーの試合中のスコアの推移を確率モデルで表し、*Dixon&Robinson* (1998)モデルの改善を目指す修士研究のリポジトリ。

## 研究の目的
　*Dixon&Robinson* (1998)モデルは、試合中のスコアの遷移に二変量の出生過程を仮定し、
0-0、1-0のようなスコア状況ごとに得点のハザードを変化させることで、得点の推移を表現している。

　本研究の目的は、このモデルの改善である。
StatsBombのイベントデータを用いて得点間の待ち時間を経験的に分析し、スコア状況の区分以外にハザードに影響する特徴を抽出する。
抽出した特徴をモデルに組み込み、元のモデルとAIC・BICで比較する。

## 研究の流れ
1. **Empirical_Analysis**：得点間の待ち時間を生存時間分析（Kaplan-Meier曲線・ログランク検定）などで分析し、ハザードに影響する特徴を探す。
2. **Models**：見つけた特徴をハザードに組み込んだモデルを作り、*Dixon&Robinson* (1998)モデルと比較する。

## リポジトリ構成
```
Soccer_Score_Progression_Analysis/
├── README.md
├── DATAAVAILABILITY.md       # 使用データ（StatsBomb Open Data）について
├── Empirical_Analysis/       # 得点間の待ち時間の経験的分析
│   ├── Skill_Gap/            # 実力差による違い
│   ├── Match_Status/         # 試合状況（スコア・直近の得点者）による違い
│   └── Time_Zone/            # 時間帯による違い
└── Models/                   # 確率モデルの実装と推定
```

## Empirical_Analysis
　得点間の待ち時間に影響を与える要因を、**実力差**・**試合状況**・**時間帯**の3つに分けて分析する。
詳細は [Empirical_Analysis/README.md](Empirical_Analysis/README.md) を参照。

| フォルダ | 内容 | 主なノートブック |
|---|---|---|
| `Skill_Gap` | 両チームの勝ち点差（シーズン終了時・暫定）の区分ごとに、次の得点までの待ち時間を比較する | `survival_final_point_gap.ipynb`, `survival_provisional_point_gap.ipynb` |
| `Match_Status` | スコア差、直近の得点者、スコアにどちらの得点で到達したかによって、次の得点までの待ち時間を比較する | `survival_score_difference.ipynb`, `survival_recent_score.ipynb`, `survival_score_progression_patterns.ipynb` |
| `Time_Zone` | 得点時刻の分布（ヒストグラム・カーネル密度推定） | `scoring_time.ipynb` |

　各フォルダの `figures/` に図を保存している。

## Models
　**Empirical_Analysis** で抽出した特徴をハザードに組み込み、*Dixon&Robinson* (1998)モデルと比較する。
詳細は [Models/README.md](Models/README.md) を参照。

| ファイル | 内容 |
|---|---|
| `dixon_robinson_model.ipynb` | *Dixon&Robinson* (1998) モデルVI（退場者の効果なし）の実装と、各リーグ・シーズンでの推定 |

## データ
　[StatsBomb Open Data](https://github.com/statsbomb/open-data) の以下の大会・シーズンを使用している。
データ本体はリポジトリに含めていない。詳細とクレジットは [DATAAVAILABILITY.md](DATAAVAILABILITY.md) を参照。

- 男子：Premier League、La Liga、Serie A、Ligue 1（いずれも2015/16）、Indian Super League 2021/22
- 女子：FA Women's Super League（2018/19、2019/20、2020/21、2023/24）、Liga F 2023/24、Frauen-Bundesliga 2023/24

## 参考文献
- Dixon, M. J. and Robinson, M. E. (1998). A birth process model for association football matches. *Journal of the Royal Statistical Society: Series D (The Statistician)*, 47(3), 523–538.
