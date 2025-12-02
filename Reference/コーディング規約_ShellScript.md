# Shell Script（bash/zsh）コーディング規約

## macOS Shell Scriptのベストプラクティス

Shell Scriptには公式の厳格なスタイルガイドはありませんが、可読性と保守性を高めるためのベストプラクティスをまとめました。
Google Shell Style GuideとAppleのベストプラクティスを参考にしています。

---

## 基本構造

### ファイルの基本テンプレート
```bash
#!/bin/bash
# または #!/bin/zsh

################################################################################
# スクリプト名: script.sh
# 作成日: YYYY-MM-DD
# 作成者: [名前]
# 説明: [このスクリプトの目的]
################################################################################

# エラー時に即座に終了
set -e

# 未定義変数の使用でエラー
set -u

# パイプライン内のエラーを検知
set -o pipefail

# ここに処理を記述

exit 0
```

### シェバン（shebang）の選択
```bash
#!/bin/bash           # bashを使用
#!/bin/zsh            # zshを使用（macOS 10.15以降のデフォルト）
#!/usr/bin/env bash   # PATHからbashを検索（移植性高）
#!/usr/bin/env zsh    # PATHからzshを検索（移植性高）
```

---

## コメント

### コメントの書き方
```bash
# 標準的なコメント
# この行は処理を行わない

################################################################################
# セクション区切り
################################################################################

# 複数行のコメント
# 1行目の説明
# 2行目の説明

: <<'END_COMMENT'
ブロックコメント
複数行のコメントを書く場合に使用
END_COMMENT
```

---

## 変数

### 変数の命名
```bash
# 小文字 + アンダースコア（ローカル変数・推奨）
input_file="input.txt"
output_dir="/path/to/output"
max_count=100

# 大文字 + アンダースコア（環境変数・グローバル変数）
export MY_VAR="value"
readonly CONSTANT_VALUE="const"

# キャメルケース（非推奨）
inputFile="input.txt"  # 避ける
```

### 変数の設定と参照
```bash
# 変数の設定
var_name="value"

# 変数の参照（推奨: 引用符とブレース）
echo "${var_name}"

# 読み取り専用変数
readonly CONST_VAR="constant"

# 配列
array=(item1 item2 item3)
echo "${array[0]}"      # 最初の要素
echo "${array[@]}"      # 全要素
echo "${#array[@]}"     # 要素数
```

### 特殊変数
```bash
$0          # スクリプト名
$1, $2, ... # コマンドライン引数
$#          # 引数の数
$@          # すべての引数（配列として）
$*          # すべての引数（文字列として）
$?          # 直前のコマンドの終了ステータス
$$          # 現在のプロセスID
```

---

## 文字列操作

### 引用符の使い方
```bash
# ダブルクォート: 変数展開される
echo "Hello ${name}"

# シングルクォート: 変数展開されない
echo 'Hello ${name}'

# バッククォート（非推奨）
result=`command`

# $() の使用（推奨）
result=$(command)
```

### 文字列のチェック
```bash
# 空文字列のチェック
if [[ -z "${var}" ]]; then
    echo "変数が空です"
fi

# 非空文字列のチェック
if [[ -n "${var}" ]]; then
    echo "変数に値があります"
fi

# 文字列比較
if [[ "${var}" == "value" ]]; then
    echo "一致"
fi

# 部分一致
if [[ "${var}" == *"substring"* ]]; then
    echo "含まれています"
fi
```

---

## 条件分岐

### if文
```bash
# 基本的なif文
if [[ "${var}" == "value" ]]; then
    echo "一致"
elif [[ "${var}" == "other" ]]; then
    echo "その他"
else
    echo "不一致"
fi

# ファイルの存在確認
if [[ -f "file.txt" ]]; then
    echo "ファイルが存在します"
fi

if [[ ! -f "file.txt" ]]; then
    echo "ファイルが存在しません"
fi

# ディレクトリの存在確認
if [[ -d "directory" ]]; then
    echo "ディレクトリが存在します"
fi

# 実行可能かチェック
if [[ -x "script.sh" ]]; then
    echo "実行可能です"
fi

# 数値比較
if [[ ${num} -eq 10 ]]; then
    echo "等しい"
fi

# -eq: 等しい
# -ne: 等しくない
# -lt: より小さい
# -le: 以下
# -gt: より大きい
# -ge: 以上
```

### case文
```bash
case "${var}" in
    pattern1)
        echo "パターン1"
        ;;
    pattern2)
        echo "パターン2"
        ;;
    *)
        echo "その他"
        ;;
esac
```

---

## ループ

### for文（ファイル）
```bash
# カレントディレクトリのファイル
for file in *.txt; do
    echo "${file}"
done

# 配列のループ
files=(file1.txt file2.txt file3.txt)
for file in "${files[@]}"; do
    echo "${file}"
done

# 再帰的に検索（find使用）
find /path/to/dir -name "*.txt" | while read -r file; do
    echo "${file}"
done
```

### for文（数値）
```bash
# 1から10まで
for i in {1..10}; do
    echo "${i}"
done

# Cスタイル
for ((i=1; i<=10; i++)); do
    echo "${i}"
done

# 10から1まで（逆順）
for i in {10..1}; do
    echo "${i}"
done
```

### while文
```bash
# 条件を満たす間ループ
count=0
while [[ ${count} -lt 10 ]]; do
    echo "${count}"
    ((count++))
done

# ファイルの各行を処理
while IFS= read -r line; do
    echo "${line}"
done < file.txt
```

---

## 関数

### 基本的な関数
```bash
# 関数定義
function_name() {
    echo "関数内の処理"
    return 0
}

# 関数呼び出し
function_name

# 引数付き関数
function_with_args() {
    local arg1="$1"
    local arg2="$2"
    echo "引数1: ${arg1}"
    echo "引数2: ${arg2}"
    return 0
}

# 関数呼び出し
function_with_args "value1" "value2"
```

### 戻り値のある関数
```bash
# return は終了ステータスのみ（0-255）
get_value() {
    echo "返される値"
    return 0
}

# 標準出力をキャプチャ
result=$(get_value)
echo "戻り値: ${result}"

# 終了ステータスの確認
get_status() {
    return 42
}

get_status
status=$?
echo "ステータス: ${status}"
```

---

## エラーハンドリング

### 基本的なエラーチェック
```bash
# コマンドの実行
command
if [[ $? -ne 0 ]]; then
    echo "エラーが発生しました" >&2
    exit 1
fi

# または
if ! command; then
    echo "エラーが発生しました" >&2
    exit 1
fi

# 成功時のみ次のコマンドを実行
command && echo "成功"

# 失敗時のみ次のコマンドを実行
command || echo "失敗"
```

### set オプション
```bash
# エラー時に即座に終了
set -e

# 未定義変数の使用でエラー
set -u

# パイプライン内のエラーを検知
set -o pipefail

# 全てを有効化
set -euo pipefail

# デバッグモード（実行内容を表示）
set -x
```

### trap を使ったクリーンアップ
```bash
# 終了時に必ず実行される処理
cleanup() {
    echo "クリーンアップ処理"
    rm -f /tmp/temp_file
}

trap cleanup EXIT

# エラー時の処理
error_handler() {
    echo "エラーが発生しました: 行 $1" >&2
    exit 1
}

trap 'error_handler ${LINENO}' ERR
```

---

## ファイル・ディレクトリ操作

### パスの扱い
```bash
# スペースを含むパスは引用符で囲む
cd "/path/with spaces"
cp "file with spaces.txt" "destination/"

# パスの結合
base_dir="/workspace"
sub_dir="data"
full_path="${base_dir}/${sub_dir}"

# ホームディレクトリ
home_path=~
home_path="${HOME}"
```

### よく使う操作
```bash
# ディレクトリ作成
mkdir -p "folder"       # 親ディレクトリも作成

# ファイルコピー
cp "source.txt" "dest.txt"
cp -r "folder" "new_folder"  # ディレクトリごとコピー

# ファイル移動
mv "source.txt" "dest.txt"

# ファイル削除
rm "file.txt"
rm -f "file.txt"        # 強制削除
rm -rf "folder"         # ディレクトリごと削除

# リンク作成
ln -s "/path/to/original" "symlink"  # シンボリックリンク
```

### スクリプト自身のパス取得
```bash
# スクリプトのディレクトリ
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

# スクリプトのファイル名
SCRIPT_NAME="$(basename "${BASH_SOURCE[0]}")"

# スクリプトの絶対パス
SCRIPT_PATH="${SCRIPT_DIR}/${SCRIPT_NAME}"
```

---

## 日付・時刻

```bash
# 現在の日付・時刻
date

# フォーマット指定
date +"%Y-%m-%d"              # 2025-11-22
date +"%Y%m%d_%H%M%S"          # 20251122_153045
date +"%Y年%m月%d日"          # 2025年11月22日

# 日付計算（macOS）
date -v +1d +"%Y-%m-%d"        # 明日
date -v -7d +"%Y-%m-%d"        # 1週間前
date -v +1m +"%Y-%m-%d"        # 1ヶ月後

# 日付計算（GNU date - brew install coreutils）
gdate -d "tomorrow" +"%Y-%m-%d"
gdate -d "7 days ago" +"%Y-%m-%d"
gdate -d "1 month" +"%Y-%m-%d"
```

---

## 入出力

### 標準入力・標準出力
```bash
# 標準出力
echo "メッセージ"
printf "%s\n" "メッセージ"

# 標準エラー出力
echo "エラーメッセージ" >&2

# リダイレクト
command > output.txt           # 上書き
command >> output.txt          # 追記
command 2> error.txt           # エラー出力のみ
command > output.txt 2>&1      # 標準出力とエラー出力
command &> output.txt          # 同上（短縮版）
```

### ユーザー入力
```bash
# 入力を受け取る
read -r user_input
echo "入力: ${user_input}"

# プロンプト付き
read -p "名前を入力: " name
echo "こんにちは、${name}さん"

# パスワード入力（非表示）
read -s -p "パスワード: " password
echo
```

---

## ベストプラクティス

### 1. シェバンを明示
```bash
#!/bin/bash
# または
#!/usr/bin/env bash
```

### 2. set オプションを使用
```bash
set -euo pipefail
```

### 3. 変数は引用符とブレースで囲む
```bash
# 推奨
echo "${var}"

# 避ける
echo $var
```

### 4. ローカル変数を使用
```bash
function_name() {
    local local_var="value"
    echo "${local_var}"
}
```

### 5. エラーチェックを行う
```bash
if ! command; then
    echo "エラー: command failed" >&2
    exit 1
fi
```

### 6. trap を使用したクリーンアップ
```bash
cleanup() {
    # クリーンアップ処理
}
trap cleanup EXIT
```

### 7. shellcheck を使用
```bash
# インストール
brew install shellcheck

# チェック実行
shellcheck script.sh
```

---

## 避けるべきこと

### 1. 引用符なしの変数展開
```bash
# NG
echo $var
cp $file $dest

# OK
echo "${var}"
cp "${file}" "${dest}"
```

### 2. バッククォートの使用
```bash
# NG
result=`command`

# OK
result=$(command)
```

### 3. [ ] の使用（[[ ]] を推奨）
```bash
# NG
if [ "$var" = "value" ]; then

# OK
if [[ "${var}" == "value" ]]; then
```

### 4. eval の過度な使用
```bash
# eval は危険なので避ける
# どうしても必要な場合のみ使用
```

---

## デバッグ

```bash
# デバッグモード: コマンドを表示
set -x

# 特定の部分のみデバッグ
echo "デバッグ開始"
set -x
command1
command2
set +x
echo "デバッグ終了"

# verbose モード
set -v
```

---

## まとめ

### 推奨事項
1. **シェバンを明示**
2. **set -euo pipefail を使用**
3. **変数は引用符とブレースで囲む**
4. **ローカル変数を使用**
5. **エラーチェックを実施**
6. **trap でクリーンアップ**
7. **shellcheck でチェック**
8. **コメントを適切に記載**

### 参考資料
- [Google Shell Style Guide](https://google.github.io/styleguide/shellguide.html)
- [ShellCheck](https://www.shellcheck.net/)

---

**最終更新日**: 2025年11月22日
**バージョン**: 1.0
