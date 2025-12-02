# Python トラブルシューティング

## よくあるエラーと解決方法

---

## 1. ModuleNotFoundError

### エラーメッセージ
```
ModuleNotFoundError: No module named 'pandas'
```

### 原因
- 必要なライブラリがインストールされていない
- 仮想環境が有効化されていない

### 解決方法
```bash
# ライブラリをインストール
pip install pandas

# 複数のライブラリを一括インストール
pip install -r requirements.txt

# 仮想環境を有効化（venv使用時 - macOS/Linux）
source venv/bin/activate
```

---

## 2. UnicodeDecodeError

### エラーメッセージ
```
UnicodeDecodeError: 'cp932' codec can't decode byte 0x81 in position 0
```

### 原因
- ファイルのエンコーディングが正しく指定されていない
- ファイルのエンコーディングとPythonの読み込み設定が一致していない

### 解決方法
```python
# UTF-8で明示的に指定
with open('file.txt', 'r', encoding='utf-8') as f:
    content = f.read()

# エラーを無視する場合
with open('file.txt', 'r', encoding='utf-8', errors='ignore') as f:
    content = f.read()

# pandasで読み込む場合
import pandas as pd
df = pd.read_csv('file.csv', encoding='utf-8')
```

---

## 3. FileNotFoundError

### エラーメッセージ
```
FileNotFoundError: [Errno 2] No such file or directory: 'data.csv'
```

### 原因
- ファイルパスが間違っている
- カレントディレクトリが想定と異なる
- ファイルが存在しない

### 解決方法
```python
import os
from pathlib import Path

# カレントディレクトリを確認
print(os.getcwd())

# 絶対パスを使用（macOS）
file_path = '/Users/hiraihironori/Desktop/WorkSpace/20251113/data.csv'

# Pathオブジェクトを使用（推奨・クロスプラットフォーム対応）
file_path = Path(__file__).parent / 'data.csv'

# ファイルの存在確認
if os.path.exists(file_path):
    # 処理
    pass
else:
    print(f"ファイルが見つかりません: {file_path}")
```

---

## 4. KeyError (辞書・DataFrame)

### エラーメッセージ
```
KeyError: 'column_name'
```

### 原因
- 存在しないキーや列名を指定している
- 列名のスペルミス
- 列名に余分なスペースが含まれている

### 解決方法
```python
import pandas as pd

# 列名を確認
print(df.columns.tolist())

# 列名の空白を削除
df.columns = df.columns.str.strip()

# 安全にアクセス
value = df.get('column_name', default_value)

# 列の存在確認
if 'column_name' in df.columns:
    value = df['column_name']
```

---

## 5. IndentationError

### エラーメッセージ
```
IndentationError: unexpected indent
```

### 原因
- インデント（字下げ）が不正
- タブとスペースが混在している

### 解決方法
```python
# 正しいインデント（スペース4つを推奨）
def example():
    if True:
        print("OK")

# VSCodeの設定でタブをスペースに変換
# settings.json に以下を追加
# "editor.insertSpaces": true,
# "editor.tabSize": 4
```

---

## 6. TypeError: 型の不一致

### エラーメッセージ
```
TypeError: can only concatenate str (not "int") to str
```

### 原因
- 異なる型同士で演算を行っている
- 型変換が必要

### 解決方法
```python
# 文字列と数値の結合
age = 25
message = "Age: " + str(age)  # str()で変換

# 数値計算
num_str = "123"
result = int(num_str) + 100  # int()で変換

# 型を確認
print(type(variable))
```

---

## 7. ImportError: circular import

### エラーメッセージ
```
ImportError: cannot import name 'function' from partially initialized module
```

### 原因
- 循環インポート（AがBをインポート、BがAをインポート）

### 解決方法
```python
# 関数内でインポート
def my_function():
    from module_b import function_b
    return function_b()

# インポート構造を見直す
# 共通部分を別モジュールに分離
```

---

## 8. PermissionError

### エラーメッセージ
```
PermissionError: [Errno 13] Permission denied: 'file.txt'
```

### 原因
- ファイルが他のプログラムで開かれている
- 書き込み権限がない
- ディレクトリをファイルとして開こうとしている

### 解決方法
```python
# ファイルを閉じてから操作
with open('file.txt', 'r') as f:
    content = f.read()
# with文を抜けると自動的に閉じられる

# 管理者権限で実行（macOS）
# sudo python3 script.py

# ファイル権限を確認・変更（macOS）
# ls -l file.txt
# chmod 644 file.txt  # 読み書き権限付与

# ファイルがディレクトリでないか確認
import os
if os.path.isfile(path):
    # ファイル処理
    pass
```

---

## 9. pandas - SettingWithCopyWarning

### エラーメッセージ
```
SettingWithCopyWarning: A value is trying to be set on a copy of a slice
```

### 原因
- DataFrameのビュー（参照）に対して値を設定しようとしている

### 解決方法
```python
import pandas as pd

# .loc[]を使用（推奨）
df.loc[df['age'] > 20, 'category'] = 'adult'

# copy()を使用
df_copy = df[df['age'] > 20].copy()
df_copy['category'] = 'adult'

# 警告を抑制（非推奨）
pd.options.mode.chained_assignment = None
```

---

## 10. JSONDecodeError

### エラーメッセージ
```
json.decoder.JSONDecodeError: Expecting value: line 1 column 1 (char 0)
```

### 原因
- JSONファイルが空
- JSONフォーマットが不正
- ファイルがJSONでない

### 解決方法
```python
import json

# エラーハンドリング
try:
    with open('data.json', 'r', encoding='utf-8') as f:
        data = json.load(f)
except json.JSONDecodeError as e:
    print(f"JSON解析エラー: {e}")
    # デフォルト値を使用
    data = {}

# ファイルが空でないか確認
import os
if os.path.getsize('data.json') > 0:
    with open('data.json', 'r') as f:
        data = json.load(f)
```

---

## 11. 相対インポートエラー

### エラーメッセージ
```
ImportError: attempted relative import with no known parent package
```

### 原因
- スクリプトをモジュールとして実行していない

### 解決方法
```powershell
# モジュールとして実行
python -m package.module

# または絶対インポートを使用
from package.subpackage import module
```

---

## 12. メモリエラー

### エラーメッセージ
```
MemoryError
```

### 原因
- 大量のデータを一度に読み込んでいる
- メモリ不足

### 解決方法
```python
import pandas as pd

# チャンク読み込み
chunk_size = 10000
for chunk in pd.read_csv('large_file.csv', chunksize=chunk_size):
    # チャンクごとに処理
    process(chunk)

# 必要な列だけ読み込む
df = pd.read_csv('file.csv', usecols=['col1', 'col2'])

# データ型を指定してメモリを節約
df = pd.read_csv('file.csv', dtype={'id': 'int32'})
```

---

## トラブルシューティングの基本手順

1. **エラーメッセージを最後まで読む**
   - エラーの種類と発生場所を確認

2. **スタックトレースを確認**
   - どのファイルの何行目でエラーが発生したか

3. **変数の内容を確認**
   ```python
   print(type(variable))
   print(variable)
   ```

4. **段階的にデバッグ**
   - print文を追加して動作を確認
   - デバッガーを使用

5. **公式ドキュメントを確認**
   - https://docs.python.org/ja/3/

---

**最終更新日**: 2025年11月13日
**バージョン**: 1.0
