# PowerShell クイックリファレンス

## macOS での PowerShell について

PowerShell CoreはmacOSでも利用可能です。

### インストール方法
```bash
brew install powershell
```

### 起動方法
```bash
pwsh
```

**注意**: このドキュメントはPowerShell Core（クロスプラットフォーム版）を前提としています。
Windows専用のコマンドレット（例: `Get-ComputerInfo`）は、macOSでは使用できない場合があります。

---

## よく使うコマンド一覧

---

## ファイル・ディレクトリ操作

### 一覧表示
```powershell
# ファイル一覧
Get-ChildItem
ls                    # エイリアス
dir                   # エイリアス

# 特定の拡張子のみ
Get-ChildItem -Filter "*.txt"
Get-ChildItem -Include "*.csv","*.json" -Recurse

# 隠しファイルも表示
Get-ChildItem -Force

# サブディレクトリも含む
Get-ChildItem -Recurse
```

### ディレクトリ移動
```powershell
# ディレクトリ移動
Set-Location "/path/to/directory"
cd "/path/to/directory"     # エイリアス

# 親ディレクトリに移動
cd ..

# ホームディレクトリに移動
cd ~

# 前のディレクトリに戻る
cd -
```

### ディレクトリ・ファイル作成
```powershell
# ディレクトリ作成
New-Item -ItemType Directory -Path "folder_name"
mkdir "folder_name"           # エイリアス

# ファイル作成
New-Item -ItemType File -Path "file.txt"

# 親ディレクトリも含めて作成
New-Item -ItemType Directory -Path "parent\child" -Force
```

### コピー・移動・削除
```powershell
# コピー
Copy-Item -Path "source.txt" -Destination "dest.txt"
Copy-Item -Path "folder" -Destination "new_folder" -Recurse

# 移動
Move-Item -Path "source.txt" -Destination "dest.txt"

# 削除
Remove-Item -Path "file.txt"
Remove-Item -Path "folder" -Recurse         # ディレクトリごと削除
Remove-Item -Path "*.tmp" -Force            # 強制削除
```

### ファイル名変更
```powershell
# 単一ファイル
Rename-Item -Path "old.txt" -NewName "new.txt"

# 一括リネーム
Get-ChildItem -Filter "*.txt" | Rename-Item -NewName {$_.Name -replace '.txt','.md'}
```

---

## ファイル内容の操作

### 読み込み
```powershell
# 全行読み込み
Get-Content -Path "file.txt"
cat "file.txt"                # エイリアス

# エンコーディング指定
Get-Content -Path "file.txt" -Encoding UTF8

# 最初の10行
Get-Content -Path "file.txt" -TotalCount 10

# 最後の10行
Get-Content -Path "file.txt" -Tail 10

# リアルタイム監視
Get-Content -Path "log.txt" -Wait
```

### 書き込み
```powershell
# 上書き
"Hello World" | Out-File -FilePath "file.txt"
"Hello World" > "file.txt"    # リダイレクト

# 追記
"New Line" | Add-Content -Path "file.txt"
"New Line" >> "file.txt"      # リダイレクト

# エンコーディング指定
"テキスト" | Out-File -FilePath "file.txt" -Encoding UTF8
```

### 検索・置換
```powershell
# ファイル内を検索
Select-String -Path "file.txt" -Pattern "keyword"
Select-String -Path "*.txt" -Pattern "keyword" -Recurse

# 正規表現で検索
Select-String -Path "file.txt" -Pattern "\d{3}-\d{4}"

# 大文字小文字を区別しない
Select-String -Path "file.txt" -Pattern "keyword" -CaseSensitive:$false

# 置換（ファイル内容を読み込んで置換後に保存）
(Get-Content -Path "file.txt") -replace 'old', 'new' | Set-Content -Path "file.txt"
```

---

## プロセス管理

### プロセス一覧
```powershell
# 全プロセス表示
Get-Process

# 特定のプロセス
Get-Process -Name "notepad"

# CPU使用率順
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10

# メモリ使用率順
Get-Process | Sort-Object WS -Descending | Select-Object -First 10
```

### プロセス操作
```powershell
# プロセス起動
Start-Process "notepad.exe"
Start-Process "notepad.exe" -ArgumentList "file.txt"

# プロセス停止
Stop-Process -Name "notepad"
Stop-Process -Id 1234
Stop-Process -Name "app" -Force
```

---

## 変数・環境変数

### 変数
```powershell
# 変数の定義
$name = "value"
$number = 123
$array = @(1, 2, 3, 4, 5)
$hash = @{key1="value1"; key2="value2"}

# 変数の表示
$name
Write-Host $name
echo $name

# 変数の型
$name.GetType()
```

### 環境変数
```powershell
# 環境変数の表示
$env:Path
$env:USER           # macOS
$env:USERNAME       # Windows
$env:HOSTNAME       # macOS
$env:COMPUTERNAME   # Windows

# 環境変数の設定（一時的）
$env:MYVAR = "value"

# 環境変数の追加（macOS - コロン区切り）
$env:Path += ":/usr/local/bin"
# 注: Windowsではセミコロン区切りを使用

# 全環境変数の表示
Get-ChildItem Env:
```

---

## ネットワーク

### 接続確認
```powershell
# Ping
Test-Connection -ComputerName "google.com" -Count 4

# ポート確認
Test-NetConnection -ComputerName "localhost" -Port 80

# HTTP リクエスト
Invoke-WebRequest -Uri "https://example.com"
Invoke-RestMethod -Uri "https://api.example.com/data"
```

### ダウンロード
```powershell
# ファイルダウンロード
Invoke-WebRequest -Uri "https://example.com/file.zip" -OutFile "file.zip"

# 短縮形
iwr "https://example.com/file.zip" -OutFile "file.zip"
```

---

## 日付・時刻

```powershell
# 現在の日時
Get-Date

# フォーマット指定
Get-Date -Format "yyyy-MM-dd"
Get-Date -Format "yyyyMMdd_HHmmss"
Get-Date -Format "yyyy年MM月dd日"

# 日付計算
(Get-Date).AddDays(1)     # 明日
(Get-Date).AddDays(-7)    # 1週間前
(Get-Date).AddMonths(1)   # 1ヶ月後
```

---

## 圧縮・解凍

```powershell
# ZIP圧縮
Compress-Archive -Path "folder" -DestinationPath "archive.zip"
Compress-Archive -Path "*.txt" -DestinationPath "files.zip"

# ZIP解凍
Expand-Archive -Path "archive.zip" -DestinationPath "destination"

# 上書き展開
Expand-Archive -Path "archive.zip" -DestinationPath "destination" -Force
```

---

## パイプライン・フィルタ

```powershell
# Where-Object でフィルタ
Get-ChildItem | Where-Object {$_.Length -gt 1MB}
Get-Process | Where-Object {$_.CPU -gt 100}

# Select-Object で列選択
Get-Process | Select-Object Name, CPU, Id

# Sort-Object でソート
Get-ChildItem | Sort-Object Length -Descending

# ForEach-Object でループ
Get-ChildItem | ForEach-Object {Write-Host $_.Name}

# Group-Object でグループ化
Get-ChildItem | Group-Object Extension

# Measure-Object で集計
Get-ChildItem | Measure-Object -Property Length -Sum -Average
```

---

## システム情報

```powershell
# OS情報
[System.Environment]::OSVersion
$PSVersionTable  # PowerShellバージョン

# ディスク容量
Get-PSDrive

# macOSでの追加情報
# システム情報は system_profiler コマンドを使用
# メモリ情報
sysctl hw.memsize  # bash/zshで実行

# CPU情報
sysctl machdep.cpu  # bash/zshで実行

# 注意: Get-ComputerInfo, Get-Volume, Get-CimInstance は
# Windows専用のコマンドレットでmacOSでは使用できません
```

---

## エラー処理

```powershell
# try-catch
try {
    Get-Content -Path "nonexistent.txt" -ErrorAction Stop
} catch {
    Write-Host "エラー: $_"
}

# エラーアクションの設定
$ErrorActionPreference = "Stop"      # エラーで停止
$ErrorActionPreference = "Continue"  # エラーを表示して継続
$ErrorActionPreference = "SilentlyContinue"  # エラーを無視

# 個別コマンドでエラーアクション指定
Get-Item "file.txt" -ErrorAction SilentlyContinue
```

---

## 便利なショートカット

```powershell
# コマンド履歴
Get-History
h                     # エイリアス

# 履歴から実行
Invoke-History 5      # 5番目のコマンドを実行
r 5                   # エイリアス

# クリップボード操作
Get-Clipboard
Set-Clipboard "text"
Get-Content file.txt | Set-Clipboard

# 実行時間計測
Measure-Command {Get-ChildItem -Recurse}
```

---

**最終更新日**: 2025年11月13日
**バージョン**: 1.0
