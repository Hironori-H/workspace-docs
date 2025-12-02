# Python コーディング規約

## PEP 8 に基づくコーディング規約

このドキュメントは、Python公式のスタイルガイド **PEP 8** の推奨事項を簡潔にまとめたものです。

参考: https://pep8-ja.readthedocs.io/ja/latest/

---

## インデント・スペース

### 基本ルール
```python
# インデントはスペース4つ
def function():
    if True:
        print("インデント4スペース")

# タブは使用しない（スペースのみ）

# 行の長さは79文字以内を推奨
# コメント・ドキュメント文字列は72文字以内
```

### 長い行の折り返し
```python
# カッコを使った暗黙の行継続（推奨）
result = function(
    arg1, arg2,
    arg3, arg4
)

# バックスラッシュによる明示的な行継続
total = first_variable + \
        second_variable

# リスト・辞書の折り返し
my_list = [
    1, 2, 3,
    4, 5, 6,
]
```

---

## 命名規則

### 変数名・関数名
```python
# 小文字 + アンダースコア（snake_case）
variable_name = 10
def function_name():
    pass

# 定数は大文字 + アンダースコア
MAX_VALUE = 100
DEFAULT_TIMEOUT = 30
```

### クラス名
```python
# CapWords（PascalCase）
class MyClass:
    pass

class HttpRequest:
    pass
```

### プライベート変数
```python
class MyClass:
    def __init__(self):
        self.public_var = 1      # パブリック
        self._protected_var = 2   # プロテクテッド（慣例的）
        self.__private_var = 3    # プライベート（名前修飾）
```

### モジュール名
```python
# 短い小文字の名前、アンダースコア可
# module_name.py
# mymodule.py
```

---

## 空白行

```python
# トップレベルの関数・クラス定義の前後は2行空ける
def function1():
    pass


def function2():
    pass


class MyClass:
    pass


# クラス内のメソッド間は1行空ける
class MyClass:
    def method1(self):
        pass

    def method2(self):
        pass


# 関数内の論理的なまとまりで1行空けてもよい
def function():
    x = 1
    y = 2

    result = x + y
    return result
```

---

## インポート

### インポートの順序
```python
# 1. 標準ライブラリ
import os
import sys

# 2. サードパーティライブラリ
import numpy as np
import pandas as pd

# 3. 自作モジュール
from mymodule import myfunction
```

### インポートの書き方
```python
# 推奨：1行に1つずつ
import os
import sys

# 非推奨
import os, sys

# fromインポートは複数可
from subprocess import Popen, PIPE

# ワイルドカードインポートは避ける
from module import *  # 避ける
```

---

## スペースの使い方

### 演算子の前後
```python
# Yes
x = 1
y = 2
z = x + y

# No
x=1
y=2
z=x+y

# Yes：優先順位が明確な場合はスペースなし
i = i + 1
submitted += 1
x = x*2 - 1
hypot2 = x*x + y*y
c = (a+b) * (a-b)
```

### カンマの後
```python
# Yes
my_list = [1, 2, 3]
my_dict = {'key': 'value'}

# No
my_list = [1,2,3]
my_dict = {'key':'value'}
```

### カッコの内側
```python
# Yes
function(arg1, arg2)
my_list[0]
my_dict['key']

# No
function( arg1, arg2 )
my_list[ 0 ]
my_dict[ 'key' ]
```

### キーワード引数
```python
# Yes
def function(arg1, arg2=None):
    pass

function(1, arg2=2)

# No
def function(arg1, arg2 = None):
    pass

function(1, arg2 = 2)
```

---

## コメント

### コメントの書き方
```python
# コメントは完全な文で書く
# 最初の文字は大文字
# 文末にピリオドを付ける

# 変数の説明
count = 0  # コメントは少なくとも2スペース空ける

# 複数行コメント
# 複数行にわたる場合は、
# 各行の先頭に # を付ける
```

### ブロックコメント
```python
# ブロックコメントは、そのコードと同じレベルでインデント
def function():
    # この関数は重要な処理を行う
    # 複数の処理ステップがある
    step1()
    step2()
```

---

## ドキュメント文字列（docstring）

### 関数・メソッド
```python
def function(arg1, arg2):
    """
    関数の簡潔な説明（1行）

    詳細な説明がある場合は、
    空行を1行入れてから記載する

    Args:
        arg1 (int): 引数1の説明
        arg2 (str): 引数2の説明

    Returns:
        bool: 戻り値の説明

    Raises:
        ValueError: エラーの説明
    """
    return True
```

### クラス
```python
class MyClass:
    """
    クラスの簡潔な説明

    詳細な説明

    Attributes:
        attr1 (int): 属性1の説明
        attr2 (str): 属性2の説明
    """

    def __init__(self, attr1, attr2):
        self.attr1 = attr1
        self.attr2 = attr2
```

### モジュール
```python
"""
モジュールの説明

このモジュールは〇〇を行うためのものです。

Example:
    使用例をここに記載

    $ python module.py
"""
```

---

## 比較演算

```python
# Yes：is を使う（None、True、False との比較）
if x is None:
    pass

if x is not None:
    pass

# No
if x == None:
    pass

# Yes：bool値の確認
if greeting:
    pass

# No
if greeting == True:
    pass
if greeting is True:
    pass

# Yes：空のシーケンス確認
if not seq:
    pass

# No
if len(seq) == 0:
    pass
```

---

## 例外処理

```python
# Yes：具体的な例外を指定
try:
    value = int(text)
except ValueError:
    print("数値に変換できません")

# Yes：複数の例外
try:
    value = data[key]
except (KeyError, IndexError):
    print("キーまたはインデックスが存在しません")

# No：bare except は避ける
try:
    process()
except:  # すべての例外を捕捉（避ける）
    pass

# Yes：必要な場合は Exception を指定
try:
    process()
except Exception as e:
    logging.exception("エラー発生")
```

---

## 関数の戻り値

```python
# Yes：一貫性のある戻り値
def get_value(key):
    if key in data:
        return data[key]
    else:
        return None

# No：戻り値の型が不一致
def get_value(key):
    if key in data:
        return data[key]
    # Noneを返さない（暗黙的にNone）
```

---

## with文の使用

```python
# Yes：ファイルやリソースは with を使う
with open('file.txt', 'r') as f:
    content = f.read()

# No
f = open('file.txt', 'r')
content = f.read()
f.close()

# 複数のコンテキストマネージャ
with open('input.txt', 'r') as f_in, \
     open('output.txt', 'w') as f_out:
    f_out.write(f_in.read())
```

---

## リスト内包表記 vs ループ

```python
# Yes：シンプルな場合はリスト内包表記
squares = [x**2 for x in range(10)]

# Yes：複雑な場合は通常のループ
result = []
for item in items:
    if complex_condition(item):
        transformed = complex_transformation(item)
        if another_condition(transformed):
            result.append(transformed)
```

---

## 型ヒント（Python 3.5+）

```python
# 関数の引数と戻り値に型ヒントを追加
def greeting(name: str) -> str:
    return f"Hello, {name}"

# 変数の型ヒント
count: int = 0
names: list[str] = []

# Optional型
from typing import Optional

def get_value(key: str) -> Optional[int]:
    return data.get(key)

# 複数の型
from typing import Union

def process(value: Union[int, str]) -> str:
    return str(value)
```

---

## チェックツール

### 自動フォーマッター
```bash
# black（厳格なフォーマッター）
pip install black
black script.py

# autopep8（PEP 8準拠）
pip install autopep8
autopep8 --in-place --aggressive script.py
```

### Linter
```bash
# flake8（スタイルチェック）
pip install flake8
flake8 script.py

# pylint（詳細なチェック）
pip install pylint
pylint script.py
```

### 型チェック
```bash
# mypy（型チェック）
pip install mypy
mypy script.py
```

---

## まとめ

### 優先順位
1. **読みやすさを最優先**
2. **一貫性を保つ**（既存コードのスタイルに合わせる）
3. **PEP 8に従う**
4. **必要に応じてルールを破ってもよい**（合理的な理由がある場合）

### 参考資料
- PEP 8: https://pep8-ja.readthedocs.io/ja/latest/
- PEP 257（docstring）: https://peps.python.org/pep-0257/
- Google Pythonスタイルガイド: https://google.github.io/styleguide/pyguide.html

---

**最終更新日**: 2025年11月13日
**バージョン**: 1.0
