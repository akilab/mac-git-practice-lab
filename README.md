変更テスト
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
| stage | 次のcommitに入れる変更を置く場所 |
| commit | 変更を履歴として保存すること |
| push | 手元のcommitをGitHubへ送ること |
| pull | GitHub側の変更を手元へ取り込むこと |
| Pull Request | 自分の変更を取り込んでもらう依頼 |

## 練習の全体像

この練習では、各メンバーが自分の名前のブランチを作り、`members/` にある自分用のHTMLファイルを編集します。

例:

```text
members/akr.html
members/rui.html
members/ayu.html
members/rie.html
```

自分のファイルだけを編集するので、コンフリクトが起きにくく、Gitの基本操作に集中できます。

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
git switch -c feature/update-akr
```

確認します。

```bash
git branch
git status
```

`git switch -c` は、新しいブランチを作成して、そのブランチへ切り替えるコマンドです。
`feature/` は必須ではありません。
次のようにしてもブランチは作れます。

```bash
git switch -c update-akr
```

ただし、`feature/update-akr` のように種類を付けておくと、後から見た時に「機能追加や作業用のブランチ」だと分かりやすくなります。

昔から使われている `checkout` でも、同じようにブランチを作れます。

```bash
git checkout -b feature/update-akr
```

現在は、ブランチの切り替えには `git switch`、ファイルを戻す操作には `git restore` と、役割を分けて説明されることが増えています。

## 3. 自分のHTMLファイルを編集する

`members/` の中に、自分のHTMLファイルがあります。

例:

```bash
open members/akr.html
```

ファイル内の回答欄を編集します。
他の人のファイルは編集しません。

```bash
open members/akr.html
```

Terminalだけで編集したい場合は、次のように中身を確認できます。

```bash
cat members/akr.html
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
git add members/akr.html
git diff --staged
```

`git add .` は、今いるディレクトリ以下の変更をまとめてcommit対象に入れるコマンドです。
便利ですが、不要な一時ファイル、他の人のファイル、まだ確認していない変更まで入ることがあります。
最初の練習では、`git add members/akr.html` のようにファイル名を指定し、「何をcommitするのか」を自分で確認します。

## 5. stage, commit, pushの違い

Gitでは、編集したファイルをいきなりcommitするのではなく、まずstageに入れます。

```text
作業中のファイル
  ↓ git add
stage、つまりcommit予定
  ↓ git commit
手元の履歴
  ↓ git push
GitHub上のブランチ
```

それぞれの意味は次の通りです。

| 操作 | 何をしているか |
| --- | --- |
| `git add members/akr.html` | 次のcommitに入れる変更を選ぶ |
| `git diff --staged` | commit予定の変更を見る |
| `git commit -m "..."` | 手元のリポジトリに履歴を作る |
| `git push` | 手元のcommitをGitHubへ送る |

commitは手元に履歴を作る操作です。
commitしただけでは、まだGitHubには送られていません。
GitHubに送るには、後で `git push` を実行します。

## 6. commitする

変更を履歴として保存します。

```bash
git commit -m "Update akr profile"
```

commitできたか確認します。

```bash
git log --oneline
git status
```

## 7. GitHubへpushする

自分のブランチをGitHubへ送ります。

```bash
git push -u origin feature/update-akr
```

`origin` は、clone元のGitHubリポジトリについた名前です。
`git remote -v` で確認できます。

`-u` は upstream を設定するオプションです。
手元の `feature/update-akr` と、GitHub上の `feature/update-akr` を対応付けます。
これにより、次回から同じブランチでは `git push` だけで送れるようになります。

ここで行っているのは、Pull Requestではありません。
`git push` は、作業ブランチをGitHubへ送る操作です。
Pull Requestは、その後にGitHubの画面で「このブランチをmainへ取り込んでください」と依頼する操作です。

## 8. Pull Requestを作る

push後、GitHubの画面を開くとPull Request作成ボタンが表示されます。

Pull Requestには、次のように書きます。

```text
タイトル:
Add yamada profile

説明:
- members/akr.html の回答欄を更新しました
- Gitのclone, branch, add, commit, pushを練習しました
```

## 9. 取り込まれた後にmainを更新する

Pull Requestが取り込まれたら、自分の手元の `main` を最新にします。

```bash
git switch main
git pull
```

`main` は共有される本線のブランチです。
作業中は `feature/update-akr` にいますが、`main` が消えたわけではありません。
作業が終わったら `git switch main` で本線へ戻れます。

Pull Requestが取り込まれると、GitHub上の `main` が更新されます。
`git pull` は、その更新を手元の `main` に持ってくるコマンドです。

不要になった作業ブランチを消します。

```bash
git branch -d feature/update-akr
```

`git branch -d` は、手元に残っている作業ブランチを削除します。
Pull Requestが取り込まれた後なら、その作業ブランチの変更は `main` に入っているため、手元の作業ブランチは片付けて構いません。

GitHub側のブランチも消したい場合:

```bash
git push origin --delete feature/update-akr
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
git add 変更したファイル
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
