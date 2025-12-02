# Git クイックリファレンス

## よく使うGitコマンド一覧

---

## 基本操作

### リポジトリの初期化・クローン
```bash
# 新規リポジトリの作成
git init

# 既存リポジトリのクローン
git clone https://github.com/user/repo.git
git clone https://github.com/user/repo.git my-folder

# 特定のブランチをクローン
git clone -b branch-name https://github.com/user/repo.git
```

### 状態確認
```bash
# 現在の状態を確認
git status

# 変更内容を確認
git diff              # 未ステージの変更
git diff --staged     # ステージ済みの変更
git diff HEAD         # すべての変更

# コミット履歴を確認
git log
git log --oneline     # 1行表示
git log --graph       # グラフ表示
git log -n 5          # 最新5件
git log --author="name"  # 特定の作成者
```

---

## ステージング・コミット

### ファイルの追加
```bash
# 特定のファイルをステージ
git add file.txt
git add *.py

# すべての変更をステージ
git add .
git add -A

# 対話的にステージ
git add -p
```

### コミット
```bash
# コミット
git commit -m "コミットメッセージ"

# ステージングとコミットを同時に
git commit -am "メッセージ"

# 直前のコミットを修正
git commit --amend
git commit --amend -m "新しいメッセージ"

# 空コミット
git commit --allow-empty -m "Empty commit"
```

### 変更の取り消し
```bash
# ステージングを取り消し
git reset HEAD file.txt
git restore --staged file.txt

# ファイルの変更を取り消し
git checkout -- file.txt
git restore file.txt

# 直前のコミットを取り消し（変更は保持）
git reset --soft HEAD^

# 直前のコミットを取り消し（変更も破棄）
git reset --hard HEAD^
```

---

## ブランチ操作

### ブランチの作成・切り替え
```bash
# ブランチ一覧
git branch
git branch -a         # リモートも含む

# ブランチ作成
git branch new-branch

# ブランチ切り替え
git checkout branch-name
git switch branch-name

# ブランチ作成と切り替えを同時に
git checkout -b new-branch
git switch -c new-branch

# リモートブランチから作成
git checkout -b local-branch origin/remote-branch
```

### ブランチの削除
```bash
# ローカルブランチを削除
git branch -d branch-name

# 強制削除
git branch -D branch-name

# リモートブランチを削除
git push origin --delete branch-name
```

---

## マージ・リベース

### マージ
```bash
# 現在のブランチに他のブランチをマージ
git merge branch-name

# Fast-forwardを無効化
git merge --no-ff branch-name

# マージを中止
git merge --abort

# コンフリクト解決後
git add conflicted-file.txt
git commit
```

### リベース
```bash
# 現在のブランチを他のブランチにリベース
git rebase main

# インタラクティブリベース
git rebase -i HEAD~3

# リベースを中止
git rebase --abort

# リベース継続
git rebase --continue
```

---

## リモート操作

### リモートリポジトリの設定
```bash
# リモート一覧
git remote -v

# リモートを追加
git remote add origin https://github.com/user/repo.git

# リモートを削除
git remote remove origin

# リモート名を変更
git remote rename old-name new-name
```

### プッシュ
```bash
# リモートにプッシュ
git push origin main

# 初回プッシュ（upstream設定）
git push -u origin main

# すべてのブランチをプッシュ
git push --all

# タグをプッシュ
git push --tags

# 強制プッシュ（注意）
git push -f origin main
```

### プル・フェッチ
```bash
# リモートの変更を取得してマージ
git pull origin main

# リモートの変更を取得（マージしない）
git fetch origin

# すべてのリモートブランチを取得
git fetch --all

# プルしてリベース
git pull --rebase origin main
```

---

## スタッシュ

```bash
# 変更を一時退避
git stash
git stash save "作業中の変更"

# スタッシュ一覧
git stash list

# スタッシュを適用
git stash apply
git stash apply stash@{0}

# スタッシュを適用して削除
git stash pop

# スタッシュを削除
git stash drop stash@{0}
git stash clear  # すべて削除
```

---

## タグ

```bash
# タグ一覧
git tag

# タグを作成
git tag v1.0.0
git tag -a v1.0.0 -m "Version 1.0.0"

# タグを削除
git tag -d v1.0.0

# リモートのタグを削除
git push origin --delete v1.0.0

# タグをチェックアウト
git checkout v1.0.0
```

---

## 履歴の確認

```bash
# コミット履歴
git log
git log --oneline --graph --all

# 特定ファイルの履歴
git log -- file.txt
git log -p file.txt  # 差分も表示

# 特定のコミットを表示
git show commit-hash

# ファイルの各行の最終変更者を表示
git blame file.txt

# 差分統計
git diff --stat
```

---

## 検索

```bash
# コミットメッセージを検索
git log --grep="keyword"

# コード内を検索
git grep "keyword"
git grep "keyword" branch-name

# 変更を検索
git log -S "keyword"  # 特定のコードが追加/削除されたコミット
```

---

## 設定

### ユーザー設定
```bash
# グローバル設定
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# リポジトリごとの設定
git config user.name "Your Name"
git config user.email "your.email@example.com"

# 設定一覧
git config --list
git config --global --list
```

### エディタ設定
```bash
# デフォルトエディタを設定
git config --global core.editor "code --wait"  # VS Code
git config --global core.editor "vim"
```

### エイリアス設定
```bash
# エイリアスを設定
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.lg "log --oneline --graph --all"
```

---

## .gitignore

### 基本的な記法
```
# コメント

# 特定のファイル
file.txt

# 特定の拡張子
*.log
*.tmp

# 特定のディレクトリ
node_modules/
__pycache__/

# ディレクトリ内の特定のファイル
logs/*.log

# 例外（!で指定）
!important.log

# ネストしたパターン
**/temp/
```

### よく使うパターン
```
# Python
__pycache__/
*.py[cod]
*.egg-info/
venv/
.env

# Node.js
node_modules/
npm-debug.log

# OS
.DS_Store
Thumbs.db
desktop.ini

# IDE
.vscode/
.idea/
*.swp

# ビルド成果物
dist/
build/
*.exe
```

---

## トラブルシューティング

### コンフリクトの解決
```bash
# コンフリクトが発生したファイルを確認
git status

# ファイルを編集してマーカーを削除
# <<<<<<< HEAD
# =======
# >>>>>>> branch-name

# 解決後
git add conflicted-file.txt
git commit
```

### 間違えた場合の対処
```bash
# 直前のコミットを取り消し
git reset --soft HEAD^

# 特定のコミットに戻る
git reset --hard commit-hash

# 特定のコミットを打ち消す
git revert commit-hash

# ファイルを特定のコミット時点に戻す
git checkout commit-hash -- file.txt
```

### リモートと同期
```bash
# ローカルをリモートに強制的に合わせる
git fetch origin
git reset --hard origin/main

# リモートブランチを削除した後のクリーンアップ
git fetch --prune
git remote prune origin
```

---

## 便利なコマンド

```bash
# 最後のコミットのハッシュを取得
git rev-parse HEAD

# ブランチの最終コミット日時
git for-each-ref --sort=-committerdate refs/heads/

# 未追跡ファイルを削除
git clean -n   # 削除されるファイルを確認
git clean -f   # 実行
git clean -fd  # ディレクトリも含む

# 空のコミット（CI/CD再実行など）
git commit --allow-empty -m "Trigger CI"
```

---

**最終更新日**: 2025年11月13日
**バージョン**: 1.0
