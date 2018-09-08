# Git

## 便利コマンド

### ステージング

- `git add -u`: すでにレポジトリに追加されている(tracked=追跡してる)について更新があるものを全部追加。
- `git add -p`: 対話的にステージに追加するか決める。
- `git rm`: ファイルを削除するという変更をindexに追加する。
- `git mv`: リネームや移動をする。これは `git rm` して `git add` するのと同じ操作。

### コミット

- `git commit --allow-emtpy`: 空コミット(変更なし)でもcommitオブジェクトを作る。flakyなCI通したいとか？

### 取り消し/修正

- `git commit --amend`: 直前のコミットをなかったことにして新しいコミットオブジェクトを作る。addし忘れやコメントのtypoなおすとか。
- `git reset --soft`: 直前のコミットをなかったことにする。HEADの向き先を変えてるだけ。ORIG_HEADもしくはreflogから復旧可能。
- `git reset --hard`: 直前のコミットをなかったことにしてワークディレクトリも消す。ORIG_HEADもしくはreflogから復旧可能。

### 確認

- `git status`: だいたいみれる。
- `git diff --cached`: HEADとindexのdiffを確認する。 `git commit -v` 使うなら不要かも。
- `git show`: 未指定なら `git diff HEAD^ HEAD`。後ろに`-n <数値>` を指定したぶんだけさかのぼって差分を確認できる。

### その他

- `git pull --ff-only --prune`: fast-forwardな変更だけ取り込んで(ff-only)、リモートで削除されたブランチをローカルの参照からも消す(prune)。
- `git checkout -t origin/hoge`: リモート(ここではorigin)のhogeブランチをローカルにとってきて移動する。

## よくある事例集

### pull requestを出した後にbaseブランチが先に進んでしまった

`git pull --squash` を使う。

- master: baseブランチ
- fix-hoge: 自分の作ったやつ

とすると

```console
$ git status
On branch fix-hoge
Your branch is up to date with 'origin/fix-hoge.

nothing to commit, working tree clean
$ git pull --squash origin/master
```

commitオブジェクトは新しく作りなおされることに注意。

conflictは普通に解消する。

### pull requestした後でbaseブランチを切り替えたい、ただし変更前後のbaseブランチは結構離れてて大変

- master: 元のbaseブランチ
- develop: 変更した後のbaseブランチ
- fix-hoge: 自分の作ったやつ

とすると

```console
$ git checkout -t origin/fix-hoge
% git log --oneline -1
<commit id> なんか変更した
$ git checkout -b rescued develop
$ git cherry-pick <commit id>
$ git push -f rescued:fix-hoge
```

でいける。
