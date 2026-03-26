# BoTorch MOBO Tutorial

BoTorch を使った多目的ベイズ最適化（MOBO）の学習リポジトリ。

論文 [Active learning-enabled multi-objective design of thermally conductive and mechanically compliant polymers](https://arxiv.org/abs/2603.23494) のワークフローをトレースしながら、以下を段階的に学ぶ。

## 学習ステップ

1. **単目的BO**: BoTorch の基本的な使い方
2. **MOBO + qNEHVI**: 多目的最適化でパレートフロントを探索
3. **DKL サロゲートモデル**: Deep Kernel Learning で予測精度を向上
4. **AL パイプライン**: Active Learning ループの構築

## セットアップ

```bash
uv sync
uv run jupyter lab
```
