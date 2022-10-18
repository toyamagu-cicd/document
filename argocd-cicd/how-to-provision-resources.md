# How to Provision Resources

## Purpose

リソースプロビジョニング方法を述べること

## 手順

*: 手動

1. GitHubリソースプロビジョニング
    - *GitHubユーザー・Organization作成
    - *GitHub App作成と、Terraform実行。[手順][github-apps]
1. EKSクラスタとArgoCDプロビジョニング
    - OAuth appsを作成。[手順][oauth-apps]
    - `resources/argocd` で Terraform実行
1. K8sマニフェストリポジトリセットアップ
    - Terraformによって作成された `argocd-cicd-k8s-manifest` リポジトリの `main` , `dev` ブランチに[K8sマニフェストのサンプル][argocd-cicd-k8s-manifest]をコピー。
1. アプリケーションリポジトリセットアップ
    - `dev` ブランチ
      - Terraformによって作成された `argocd-cicd-application` リポジトリの `dev` ブランチに[アプリケーションのサンプル][argocd-cicd-application]をコピー。
      - `.github/workflows/push.yaml` の環境変数を自分の環境に合わせて変更
      - `dev` ブランチにpushしてCIが正常終了することを確認
      - ArgoCDでRefleshすると、新規アプリケーションがデプロイされる
    - `main` ブランチ
      - `dev` ブランチから `main` ブランチへの PR作成・Merge
      - K8sマニフェストリポジトリでPRが作成されているのでMerge
      - ArgoCDでRefleshすると、新規アプリケーションがデプロイされる

[github-apps]: ./github-apps.md#github-apps
[oauth-apps]: ./github-apps.md#oauth-apps
[argocd-cicd-k8s-manifest]: https://github.com/toyamagu-cicd/argocd-cicd-k8s-manifest
[argocd-cicd-application]: https://github.com/toyamagu-cicd/argocd-cicd-application
