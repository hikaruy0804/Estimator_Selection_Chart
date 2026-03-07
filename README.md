# scikit-learn Estimator Selection Chart 日本語整理

## 1. まず最初の分岐

### 1) サンプル数は 50 件を超えているか

* **50件以下**: まずはデータを増やすことが推奨されます。
* **50件超**: 次へ進みます。 ([scikit-learn][1])

### 2) ラベル付きデータはあるか

* **ある**: 教師あり学習へ
* **ない**: 教師なし学習へ ([scikit-learn][1])

---

## 2. 教師あり学習（ラベルあり）

教師あり学習では、主に次の2つに分かれます。 ([scikit-learn][1])

* **カテゴリを予測する** → 分類
* **数値を予測する** → 回帰

---

## 3. 分類（カテゴリ予測）

### A. テキストデータか？

テキスト分類かどうかで、最初の候補が変わります。 ([scikit-learn][1])

#### テキストデータの場合

* **Naive Bayes**
* **Linear SVC**
* **SGDClassifier**

特にテキストのような高次元・疎行列データでは、**線形モデルや Naive Bayes** が最初の候補になりやすいです。サンプル数が多い場合は **SGDClassifier** が計算効率面で有利です。 ([scikit-learn][1])

#### テキストデータではない場合

* **k近傍法（KNeighborsClassifier）**
* **SVC**
* **Ensemble Classifiers**
* **Linear SVC**
* **SGDClassifier**

という流れで候補が分かれます。概ね、**小〜中規模なら SVC や k近傍法**、**大規模なら Linear SVC / SGDClassifier** が入り口になります。 ([scikit-learn][1])

---

### B. 分類でのざっくりした選び方

#### サンプル数が 100K 未満

* **Linear SVC**
* **SVC**
* **k近傍法**
* **Ensemble Classifiers**
* **Naive Bayes**

が候補に入りやすいです。 ([scikit-learn][1])

#### サンプル数が多い

* **SGDClassifier**
* **Linear SVC**

が先に検討されます。大規模データで計算量を抑えやすいためです。 ([scikit-learn][1])

---

## 4. 回帰（数値予測）

### A. 重要な特徴量は少なそうか？

チャートでは、回帰でまず **「少数の特徴だけが重要そうか」** を見ます。 ([scikit-learn][1])

#### 少数の特徴量が重要そう

* **Lasso**
* **ElasticNet**

が候補になります。どちらも不要な特徴量を抑え込みやすく、特に **Lasso** は特徴選択的に働きやすいです。 ([scikit-learn][1])

#### そうでもない

* **RidgeRegression**
* **SVR (kernel="linear")**
* **SVR (kernel="rbf")**
* **Ensemble Regressors**
* **SGDRegressor**

の方向に進みます。 ([scikit-learn][1])

---

### B. 回帰でのざっくりした選び方

#### サンプル数が 100K 未満

* **RidgeRegression**
* **SVR（linear / rbf）**
* **Ensemble Regressors**

が主な候補です。
線形で十分そうなら Ridge、非線形性がありそうなら SVR やアンサンブル系を試す流れです。 ([scikit-learn][1])

#### サンプル数が多い

* **SGDRegressor**

が先に候補になります。大規模データで学習しやすいためです。 ([scikit-learn][1])

---

## 5. 教師なし学習（ラベルなし）

ラベルがない場合、チャートでは主に次の2方向に分かれます。 ([scikit-learn][1])

* **構造を見つけたい** → クラスタリング
* **可視化・圧縮したい** → 次元削減

---

## 6. クラスタリング

### A. クラスター数が既知か？

* **既知**:

  * **KMeans**
  * 大規模なら **MiniBatch KMeans**
* **未知**:

  * **MeanShift**
  * **Spectral Clustering**
  * **Gaussian Mixture Model (GMM)** 系

という流れです。 ([scikit-learn][1])

### B. サンプル数の目安

* **10K未満**: KMeans, Spectral Clustering, GMM, MeanShift なども候補
* **10K以上**: **MiniBatch KMeans** が有力になりやすいです。 ([scikit-learn][1])

---

## 7. 次元削減

特徴量を圧縮したい、可視化したい場合の候補です。チャートでは主に次が挙がっています。 ([scikit-learn][1])

* **Randomized PCA**
* **LLE**
* **Isomap**
* **Spectral Embedding**
* **Kernel Approximation**

### ざっくりした見方

* **まず標準的に圧縮したい** → PCA 系
* **非線形な低次元構造を見たい** → LLE / Isomap / Spectral Embedding
* **大規模・カーネル法を軽く近似したい** → Kernel Approximation ([scikit-learn][1])

---

## 8. 迷ったときの簡易チートシート

### 分類

* **テキスト分類**: Naive Bayes, Linear SVC, SGDClassifier
* **小〜中規模の一般分類**: SVC, KNeighborsClassifier, Ensemble Classifiers
* **大規模分類**: Linear SVC, SGDClassifier ([scikit-learn][1])

### 回帰

* **特徴選択もしたい**: Lasso, ElasticNet
* **まず線形回帰系から**: RidgeRegression
* **非線形も考えたい**: SVR, Ensemble Regressors
* **大規模回帰**: SGDRegressor ([scikit-learn][1])

### クラスタリング

* **クラスター数が分かる**: KMeans / MiniBatch KMeans
* **クラスター数が分からない**: MeanShift, Spectral Clustering, GMM 系 ([scikit-learn][1])

### 次元削減

* **まず無難に**: PCA 系
* **非線形構造を見たい**: LLE, Isomap, Spectral Embedding ([scikit-learn][1])

---

## 9. 実務で補足したいポイント

### 1) チャートは「最初の候補選び」

最終的には、**交差検証**や**評価指標**で比較して決める前提です。scikit-learn でもモデル評価・ハイパーパラメータ調整の章が別途用意されています。 ([scikit-learn][2])

### 2) 前処理がかなり重要

scikit-learn では、**標準化が必要な推定器が多い** と案内されています。特に **SVM、SGD、k近傍法、線形モデル** では、`StandardScaler` などの前処理が重要です。 ([scikit-learn][3])

### 3) まずは複数モデルを試す

チャート自体も「ざっくりしたガイド」と明記しています。最初から1つに決め打ちするより、**2〜4種類をベースラインとして比較**するのが現実的です。 ([scikit-learn][1])

---

## 10. 実務向けの最小戦略

```text
分類:
  まず Linear SVC / RandomForest系 / LogisticRegression系 を比較
  テキストなら Naive Bayes / Linear SVC / SGDClassifier を比較

回帰:
  まず Ridge / RandomForest系 / Gradient Boosting系 を比較
  疎な真の特徴がありそうなら Lasso / ElasticNet も加える

クラスタリング:
  まず KMeans
  クラスター数不明なら MeanShift や GMM系も比較

次元削減:
  まず PCA
  非線形構造が重要なら Isomap / LLE / Spectral Embedding
```

これはチャートの思想を、実務で試しやすい形に落としたものです。 ([scikit-learn][1])


[1]: https://scikit-learn.org/stable/machine_learning_map.html "13. Choosing the right estimator — scikit-learn 1.8.0 documentation"
[2]: https://scikit-learn.org/stable/modules/cross_validation.html?utm_source=chatgpt.com "3.1. Cross-validation: evaluating estimator performance"
[3]: https://scikit-learn.org/stable/modules/preprocessing.html?utm_source=chatgpt.com "7.3. Preprocessing data"
