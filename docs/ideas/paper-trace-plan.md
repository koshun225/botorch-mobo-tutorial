# 論文トレース計画

## 対象論文

- **タイトル**: Active learning-enabled multi-objective design of thermally conductive and mechanically compliant polymers
- **URL**: https://arxiv.org/abs/2603.23494
- **著者**: Yuhan Liu, Jiaxin Xu, Renzheng Zhang, Meng Jiang, Tengfei Luo
- **分野**: cond-mat.mtrl-sci（材料科学）

## 論文の概要

ポリマーの「高熱伝導率（TC）× 低弾性率（柔軟性）」という相反する特性を同時に最適化するため、能動学習（AL）+ 多目的ベイズ最適化（MOBO）のフレームワークを構築した研究。

### 論文のパイプライン

```
1. 初期データ生成（高スループットMDシミュレーション）
2. Deep Kernel Learning (DKL) サロゲートモデル × 2（TC, Bulk Modulus）
3. Multi-Objective Bayesian Optimization (MOBO)
   - 獲得関数: qNEHVI (parallel noisy expected hypervolume improvement)
4. 候補ポリマーをMDで検証 → モデル更新 → ALループ
5. パレートフロント上の6候補を特定
6. 解釈性分析 + 合成可能性（synthesizability）評価
```

### 主要技術

| 技術 | 説明 |
|------|------|
| **ベイズ最適化 (BO)** | 高コスト関数を少ない試行で最適化する手法 |
| **MOBO** | 多目的BO。パレートフロントを効率的に発見する |
| **qNEHVI** | MOBO用の獲得関数。パレートフロントの超体積改善の期待値を最大化 |
| **DKL** | ニューラルネット + ガウス過程のハイブリッド。高次元入力に強い |
| **Active Learning** | モデルの不確実性が高いデータを優先的にサンプリング |
| **パレートフロント** | 多目的最適化で、どの目的でも他の解に劣らない解の集合 |

## 学習ステップ

### Step 1: 単目的BO（所要: 1-2時間）

BoTorch の基本を理解する。

- BoTorch 公式チュートリアルをベースに単目的BOを実装
- サロゲートモデル（SingleTaskGP）と獲得関数（EI）の動作を理解
- 1D/2Dの可視化で直感を掴む

### Step 2: MOBO + qNEHVI（所要: 半日）

多目的最適化に拡張する。

- 合成テスト関数（BraninCurrin等）で2目的最適化
- qNEHVI獲得関数の使い方を理解
- パレートフロントの可視化
- BoTorch公式: Multi-Objective BO チュートリアルを参考に

### Step 3: DKLサロゲートモデル（所要: 半日〜1日）

GPyTorchのDeep Kernel Learningを導入する。

- GPyTorch で DKL モデルを構築（MLP + GP）
- Step 2 の SingleTaskGP を DKL に置き換え
- 予測精度と不確実性推定の比較

### Step 4: 公開ポリマーデータで論文風パイプライン（所要: 1-2日）

論文のワークフロー全体を再現する。

- データソース候補:
  - Polymer Genome (pgdb.org): ポリマー特性DB
  - PolyInfo (polymer.nims.go.jp): NIMS のポリマーDB
  - 論文の supplementary data（公開されていれば）
- ALループを実装し、パレートフロントが更新される過程を可視化
- 解釈性分析（特徴量重要度等）

## 技術スタック

- **BoTorch**: MOBO / 獲得関数最適化
- **GPyTorch**: ガウス過程 / DKL
- **PyTorch**: NN基盤（DKLのバックボーン）
- **matplotlib**: 可視化
- **pandas / numpy**: データ処理
- **Jupyter**: 実験ノートブック

## 環境メモ

- M4 Mac, GPUなしでOK
- BO/MOBOはデータ数N=数百〜数千でCPUで十分高速
- DKLも小規模MLPなのでCPU実用的
- PyTorch MPSバックエンドは BoTorch/GPyTorch で一部非対応演算あり → `device="cpu"` で回避

## 発信アイデア

- Zenn記事「BoTorch で多目的ベイズ最適化を材料設計に使う」
- 化学メーカーDS視点の論文レビュー記事
- このリポジトリ自体が公開チュートリアルとして価値あり
