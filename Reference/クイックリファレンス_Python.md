# Python クイックリファレンス

## よく使う構文・ライブラリ一覧

---

## 基本構文

### 変数・データ型
```python
# 変数の定義
name = "John"
age = 25
height = 175.5
is_active = True

# 型の確認
type(name)           # <class 'str'>
isinstance(age, int) # True

# 型変換
str(123)             # "123"
int("123")           # 123
float("3.14")        # 3.14
```

### リスト
```python
# リストの作成
fruits = ["apple", "banana", "cherry"]
numbers = [1, 2, 3, 4, 5]
mixed = [1, "two", 3.0, True]

# 要素へのアクセス
fruits[0]            # "apple"
fruits[-1]           # "cherry"（最後の要素）
fruits[1:3]          # ["banana", "cherry"]（スライス）

# 追加・削除
fruits.append("orange")
fruits.insert(1, "grape")
fruits.remove("banana")
del fruits[0]
popped = fruits.pop()

# リスト操作
len(fruits)          # 長さ
"apple" in fruits    # 存在確認
fruits.sort()        # ソート
fruits.reverse()     # 反転
```

### 辞書
```python
# 辞書の作成
person = {"name": "John", "age": 25}

# 要素へのアクセス
person["name"]       # "John"
person.get("age")    # 25
person.get("email", "N/A")  # キーがない場合のデフォルト値

# 追加・更新・削除
person["email"] = "john@example.com"
person.update({"phone": "123-4567"})
del person["age"]
popped = person.pop("email")

# 辞書操作
person.keys()        # キー一覧
person.values()      # 値一覧
person.items()       # (キー, 値)のペア
```

---

## 制御構文

### 条件分岐
```python
# if-elif-else
if age < 18:
    print("未成年")
elif age < 65:
    print("成人")
else:
    print("シニア")

# 三項演算子
status = "adult" if age >= 18 else "minor"

# 複数条件
if age >= 18 and age < 65:
    print("成人")

if name == "John" or name == "Jane":
    print("該当者")
```

### ループ
```python
# for ループ
for fruit in fruits:
    print(fruit)

for i in range(5):      # 0-4
    print(i)

for i in range(1, 10, 2):  # 1, 3, 5, 7, 9
    print(i)

# while ループ
count = 0
while count < 5:
    print(count)
    count += 1

# break, continue
for i in range(10):
    if i == 5:
        break           # ループを抜ける
    if i % 2 == 0:
        continue        # 次のイテレーションへ
    print(i)
```

---

## ファイル操作

### 読み込み
```python
# 全体を読み込み
with open("file.txt", "r", encoding="utf-8") as f:
    content = f.read()

# 1行ずつ読み込み
with open("file.txt", "r", encoding="utf-8") as f:
    for line in f:
        print(line.strip())

# 全行をリストとして読み込み
with open("file.txt", "r", encoding="utf-8") as f:
    lines = f.readlines()
```

### 書き込み
```python
# 上書き
with open("file.txt", "w", encoding="utf-8") as f:
    f.write("Hello World\n")

# 追記
with open("file.txt", "a", encoding="utf-8") as f:
    f.write("New Line\n")

# 複数行書き込み
lines = ["Line 1\n", "Line 2\n", "Line 3\n"]
with open("file.txt", "w", encoding="utf-8") as f:
    f.writelines(lines)
```

---

## pathlib でのファイル操作

```python
from pathlib import Path

# パスの作成
file_path = Path("C:/Users/CLPC-63/workspace/file.txt")
dir_path = Path(__file__).parent

# パス操作
file_path.name          # "file.txt"
file_path.stem          # "file"
file_path.suffix        # ".txt"
file_path.parent        # 親ディレクトリ
file_path.absolute()    # 絶対パス

# 存在確認
file_path.exists()
file_path.is_file()
file_path.is_dir()

# ファイル一覧
list(dir_path.glob("*.txt"))
list(dir_path.rglob("*.txt"))  # 再帰的に検索

# ファイル作成・削除
file_path.touch()       # ファイル作成
file_path.unlink()      # ファイル削除
dir_path.mkdir(parents=True, exist_ok=True)  # ディレクトリ作成

# ファイル読み書き
content = file_path.read_text(encoding="utf-8")
file_path.write_text("Hello", encoding="utf-8")
```

---

## 文字列操作

```python
text = "  Hello World  "

# 基本操作
text.lower()         # "  hello world  "
text.upper()         # "  HELLO WORLD  "
text.strip()         # "Hello World"
text.replace("World", "Python")  # "  Hello Python  "

# 分割・結合
text.split()         # ["Hello", "World"]
",".join(["a", "b", "c"])  # "a,b,c"

# 検索
text.find("World")   # 8（見つからない場合は -1）
text.index("World")  # 8（見つからない場合はエラー）
"Hello" in text      # True

# フォーマット
name = "John"
age = 25
f"My name is {name} and I am {age} years old"
"My name is {} and I am {} years old".format(name, age)
```

---

## リスト内包表記

```python
# 基本
numbers = [1, 2, 3, 4, 5]
squares = [x**2 for x in numbers]  # [1, 4, 9, 16, 25]

# 条件付き
evens = [x for x in numbers if x % 2 == 0]  # [2, 4]

# 辞書内包表記
squared_dict = {x: x**2 for x in numbers}  # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# ネスト
matrix = [[i*j for j in range(1, 4)] for i in range(1, 4)]
```

---

## 例外処理

```python
# 基本的な try-except
try:
    result = 10 / 0
except ZeroDivisionError as e:
    print(f"エラー: {e}")

# 複数の例外
try:
    file = open("nonexistent.txt", "r")
except FileNotFoundError:
    print("ファイルが見つかりません")
except PermissionError:
    print("権限がありません")

# finally
try:
    f = open("file.txt", "r")
    content = f.read()
except Exception as e:
    print(f"エラー: {e}")
finally:
    f.close()  # 必ず実行される

# else（例外が発生しなかった場合）
try:
    result = 10 / 2
except ZeroDivisionError:
    print("ゼロ除算")
else:
    print(f"結果: {result}")
```

---

## 日付・時刻

```python
from datetime import datetime, timedelta

# 現在日時
now = datetime.now()
today = datetime.today()

# フォーマット
now.strftime("%Y-%m-%d")           # "2025-11-13"
now.strftime("%Y%m%d_%H%M%S")      # "20251113_143052"
now.strftime("%Y年%m月%d日")        # "2025年11月13日"

# 文字列から日付へ
date_str = "2025-11-13"
date = datetime.strptime(date_str, "%Y-%m-%d")

# 日付計算
tomorrow = now + timedelta(days=1)
yesterday = now - timedelta(days=1)
next_week = now + timedelta(weeks=1)
```

---

## JSON操作

```python
import json

# JSONファイルの読み込み
with open("data.json", "r", encoding="utf-8") as f:
    data = json.load(f)

# JSONファイルへの書き込み
data = {"name": "John", "age": 25}
with open("data.json", "w", encoding="utf-8") as f:
    json.dump(data, f, ensure_ascii=False, indent=2)

# JSON文字列への変換
json_str = json.dumps(data, ensure_ascii=False)

# JSON文字列からの変換
data = json.loads(json_str)
```

---

## CSV操作

```python
import csv

# CSV読み込み
with open("data.csv", "r", encoding="utf-8") as f:
    reader = csv.reader(f)
    for row in reader:
        print(row)

# 辞書として読み込み
with open("data.csv", "r", encoding="utf-8") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row["name"], row["age"])

# CSV書き込み
data = [["name", "age"], ["John", "25"], ["Jane", "30"]]
with open("output.csv", "w", encoding="utf-8", newline="") as f:
    writer = csv.writer(f)
    writer.writerows(data)

# 辞書から書き込み
data = [
    {"name": "John", "age": 25},
    {"name": "Jane", "age": 30}
]
with open("output.csv", "w", encoding="utf-8", newline="") as f:
    fieldnames = ["name", "age"]
    writer = csv.DictWriter(f, fieldnames=fieldnames)
    writer.writeheader()
    writer.writerows(data)
```

---

## pandas 基本操作

```python
import pandas as pd

# CSVの読み込み
df = pd.read_csv("data.csv", encoding="utf-8")

# Excelの読み込み
df = pd.read_excel("data.xlsx", sheet_name="Sheet1")

# 基本情報
df.head()            # 最初の5行
df.tail()            # 最後の5行
df.info()            # データ型と欠損値
df.describe()        # 統計情報
df.shape             # (行数, 列数)
df.columns           # 列名一覧

# データ選択
df["column_name"]    # 列の選択
df[["col1", "col2"]] # 複数列の選択
df.loc[0]            # 行の選択（ラベル）
df.iloc[0]           # 行の選択（インデックス）
df.loc[df["age"] > 25]  # 条件で抽出

# データ操作
df["new_column"] = df["age"] * 2
df.drop("column_name", axis=1, inplace=True)  # 列削除
df.dropna()          # 欠損値を含む行を削除
df.fillna(0)         # 欠損値を0で埋める
df.sort_values("age", ascending=False)  # ソート

# 集計
df.groupby("category").mean()
df.groupby("category")["value"].sum()

# CSV出力
df.to_csv("output.csv", index=False, encoding="utf-8")
```

---

## os モジュール

```python
import os

# カレントディレクトリ
os.getcwd()

# ディレクトリ変更
os.chdir("path/to/directory")

# ディレクトリ作成
os.mkdir("new_folder")
os.makedirs("parent/child", exist_ok=True)

# ファイル一覧
os.listdir(".")
os.listdir("path/to/directory")

# パス操作
os.path.join("folder", "file.txt")
os.path.exists("file.txt")
os.path.isfile("file.txt")
os.path.isdir("folder")
os.path.basename("/path/to/file.txt")  # "file.txt"
os.path.dirname("/path/to/file.txt")   # "/path/to"

# ファイル削除
os.remove("file.txt")
os.rmdir("empty_folder")

# 環境変数
os.environ.get("PATH")
os.environ["MY_VAR"] = "value"
```

---

## コマンドライン引数

```python
import argparse

parser = argparse.ArgumentParser(description="スクリプトの説明")

# 引数の定義
parser.add_argument("-i", "--input", required=True, help="入力ファイル")
parser.add_argument("-o", "--output", required=True, help="出力ファイル")
parser.add_argument("-v", "--verbose", action="store_true", help="詳細表示")
parser.add_argument("--count", type=int, default=10, help="実行回数")

args = parser.parse_args()

# 引数の使用
print(args.input)
print(args.output)
if args.verbose:
    print("詳細モード")
```

---

## ロギング

```python
import logging

# 基本設定
logging.basicConfig(
    level=logging.INFO,
    format='[%(asctime)s] %(levelname)s - %(message)s',
    datefmt='%Y-%m-%d %H:%M:%S'
)

# ログ出力
logging.debug("デバッグ情報")
logging.info("情報")
logging.warning("警告")
logging.error("エラー")
logging.critical("重大なエラー")

# ファイルに出力
logging.basicConfig(
    filename='app.log',
    level=logging.INFO,
    format='[%(asctime)s] %(levelname)s - %(message)s'
)
```

---

**最終更新日**: 2025年11月13日
**バージョン**: 1.0
