# mac-git-practice-lab

Mac Terminal Practice用のGit練習リポジトリです。

このリポジトリでは、GitHubを使って次の流れを練習します。

1. リポジトリをcloneする
2. ブランチを作る
3. ファイルを修正する
4. 変更内容を確認する
5. commitする
6. GitHubへpushする
7. Pull Requestを作る
8. 管理者が取り込む

GitLabを使う場合も、手元で使う `git` コマンドはほぼ同じです。
違いは最後のPull Requestが、GitLabではMerge Requestと呼ばれる点です。

## 最初に知っておくこと

Gitは、手元の変更履歴を管理する道具です。
GitHubは、そのGitリポジトリを共有するWebサービスです。

つまり、次のように分けて考えます。

| 名前 | 役割 |
| --- | --- |
| Git | 手元で履歴を作るコマンド |
| GitHub | リモートリポジトリを置くサービス |
| repository | ファイルと履歴を管理する場所 |
| clone | リモートリポジトリを手元にコピーすること |
| branch | 作業を分けるための線 |
| commit | 変更を履歴として保存すること |
| push | 手元のcommitをGitHubへ送ること |
| pull | GitHub側の変更を手元へ取り込むこと |
| Pull Request | 自分の変更を取り込んでもらう依頼 |

## 練習の全体像

この練習では、各メンバーが自分の名前のブランチを作り、`members/` に自己紹介ファイルを追加します。

例:

```text
members/yamada.md
members/sato.md
members/tanaka.md
```

自分のファイルだけを追加・編集するので、コンフリクトが起きにくく、Gitの基本操作に集中できます。

## 1. cloneする

まず、練習用リポジトリを手元にコピーします。

```bash
cd ~/practice
git clone https://github.com/akilab/mac-git-practice-lab.git
cd mac-git-practice-lab
```

状態を確認します。

```bash
git status
git remote -v
git log --oneline
```

## 2. ブランチを作る

`main` で直接作業せず、自分用のブランチを作ります。

ブランチ名は、次のような形にします。

```text
feature/add-your-name
```

例:

```bash
git switch -c feature/add-yamada
```

確認します。

```bash
git branch
git status
```

## 3. 自己紹介ファイルを作る

`members/` の中に、自分の名前のMarkdownファイルを作ります。

例:

```bash
cp templates/member-profile.md members/yamada.md
```

ファイルを開いて、内容を編集します。

```bash
open members/yamada.md
```

Terminalだけで編集したい場合は、次のように中身を確認できます。

```bash
cat members/yamada.md
```

## 4. 変更内容を確認する

どのファイルを変更したか確認します。

```bash
git status
```

差分を確認します。

```bash
git diff
```

新規ファイルは、`git diff` だけでは中身が見えにくいことがあります。
その場合は、ファイルを追加予定にしてから確認します。

```bash
git add members/yamada.md
git diff --staged
```

## 5. commitする

変更を履歴として保存します。

```bash
git commit -m "Add yamada profile"
```

commitできたか確認します。

```bash
git log --oneline
git status
```

## 6. GitHubへpushする

自分のブランチをGitHubへ送ります。

```bash
git push -u origin feature/add-yamada
```

`-u` を付けると、次回から同じブランチでは `git push` だけで送れるようになります。

## 7. Pull Requestを作る

push後、GitHubの画面を開くとPull Request作成ボタンが表示されます。

Pull Requestには、次のように書きます。

```text
タイトル:
Add yamada profile

説明:
- members/yamada.md を追加しました
- Gitのclone, branch, add, commit, pushを練習しました
```

## 8. 取り込まれた後にmainを更新する

Pull Requestが取り込まれたら、自分の手元の `main` を最新にします。

```bash
git switch main
git pull
```

不要になった作業ブランチを消します。

```bash
git branch -d feature/add-yamada
```

GitHub側のブランチも消したい場合:

```bash
git push origin --delete feature/add-yamada
```

## よく使うコマンド早見表

| コマンド | 意味 |
| --- | --- |
| `git status` | 今の状態を見る |
| `git diff` | 変更内容を見る |
| `git add ファイル` | commit対象に入れる |
| `git diff --staged` | commit予定の差分を見る |
| `git commit -m "message"` | 変更を履歴として保存する |
| `git log --oneline` | commit履歴を見る |
| `git switch ブランチ名` | ブランチを切り替える |
| `git switch -c ブランチ名` | ブランチを作って切り替える |
| `git push` | 手元のcommitをリモートへ送る |
| `git pull` | リモートの変更を手元へ取り込む |

## GitHubとGitLabの違い

学習段階では、GitHubを基準に覚えて問題ありません。
GitHubとGitLabで、手元のGitコマンドはほとんど同じです。

| 項目 | GitHub | GitLab |
| --- | --- | --- |
| 取り込み依頼 | Pull Request | Merge Request |
| リモートURL | `github.com/...` | `gitlab.com/...` または会社GitLab |
| CI/CD | GitHub Actions | GitLab CI/CD |
| 権限管理 | Organization / Team | Group / Project |
| 手元のGitコマンド | ほぼ同じ | ほぼ同じ |

GitLabを使う時も、基本の流れは同じです。

```bash
git clone リポジトリURL
git switch -c feature/作業名
git add .
git commit -m "変更内容"
git push -u origin feature/作業名
```

その後、GitLabの画面でMerge Requestを作ります。

## 困った時

今の状態を見る:

```bash
git status
```

直前の変更内容を見る:

```bash
git diff
```

まだ `git add` していない変更を取り消す:

```bash
git restore ファイル名
```

`git add` を取り消す:

```bash
git restore --staged ファイル名
```

どのブランチにいるか見る:

```bash
git branch
```

まずは、分からなくなったら `git status` を見るところから始めます。
