# 取り込み担当者向けメモ

このファイルは、Pull Requestを受け取る側の練習メモです。

## 確認すること

- `members/` の中にある本人のHTMLファイルだけが変更されているか
- 他の人のファイルを誤って変更していないか
- Pull Requestの説明が書かれているか
- `git status`、`git add`、`git commit`、`git push` の流れを本人が説明できるか

## GitHubでの取り込み

1. Pull Requestsタブを開く
2. 対象のPull Requestを開く
3. Files changedを見る
4. 問題なければApproveまたはコメントする
5. Merge pull requestで取り込む

## GitLabで同じことをする場合

GitLabではPull RequestではなくMerge Requestと呼びます。

手元のGitコマンドは同じです。
違いは、Web画面で作成する依頼の名前と、画面の配置です。

```text
GitHub: Pull Request
GitLab: Merge Request
```

## 取り込み後

メンバーには次の操作を案内します。

```bash
git switch main
git pull
git branch -d feature/add-your-name
```
