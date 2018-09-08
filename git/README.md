# Git

## pull requestを出した後にbaseブランチが先に進んでしまった

`git pull --squash` を使う。

- master: baseブランチ
- fix-hoge: 自分の作ったやつ

とすると

```console
$ LANG=C git status
On branch fix-hoge
Your branch is up to date with 'origin/fix-hoge.

nothing to commit, working tree clean$
git pull --squash origin/master
```

commitオブジェクトは新しく作りなおされることに注意。

conflictは普通に解消する。

## pull requestした後でbaseブランチを切り替えたい、ただし変更前後のbaseブランチは結構離れてて大変

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
