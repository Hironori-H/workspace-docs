# PowerShell トラブルシューティング

## よくあるエラーと解決方法

---

## 1. 実行ポリシーエラー

### エラーメッセージ
```
.\script.ps1 : このシステムではスクリプトの実行が無効になっているため、ファイル script.ps1 を読み込むことができません。
```

### 原因
- PowerShellの実行ポリシーが制限されている

### 解決方法
```powershell
# 現在の実行ポリシーを確認
Get-ExecutionPolicy

# 実行ポリシーを変更（管理者権限が必要）
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser

# 一時的にバイパス
powershell -ExecutionPolicy Bypass -File script.ps1
```

---

## 2. パスにスペースが含まれるエラー

### エラーメッセージ
```
用語 '/Users/username/My' は、コマンドレット、関数、スクリプト ファイル、または操作可能なプログラムの名前として認識されません。
```

### 原因
- パスにスペースが含まれているのに引用符で囲んでいない

### 解決方法
```powershell
# ダブルクォーテーションで囲む
cd "/Users/username/My Documents"

# シングルクォーテーションでもOK
cd '/Users/username/My Documents'

# コマンド実行時も同様
& "/Applications/AppName.app/Contents/MacOS/program"
```

---

## 3. 文字化け

### エラーメッセージ
```
???????????
```

### 原因
- 文字エンコーディングが一致していない

### 解決方法
```powershell
# ファイルをUTF-8で読み込む
Get-Content -Path file.txt -Encoding UTF8

# 出力をUTF-8で保存
"テキスト" | Out-File -FilePath output.txt -Encoding UTF8

# コンソール出力の文字コードを変更
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8

# chcp でコードページを変更
chcp 65001  # UTF-8
```

---

## 4. コマンドが認識されない

### エラーメッセージ
```
'python' は、内部コマンドまたは外部コマンド、操作可能なプログラムまたはバッチ ファイルとして認識されていません。
```

### 原因
- 環境変数PATHにプログラムのパスが含まれていない
- プログラムがインストールされていない

### 解決方法
```powershell
# 環境変数PATHを確認
$env:Path

# 一時的にPATHを追加（macOSではコロン区切り）
$env:Path += ":/usr/local/bin"

# フルパスで実行
& "/usr/local/bin/python3" script.py

# プログラムの場所を探す
Get-Command python3
```

---

## 5. アクセス拒否エラー

### エラーメッセージ
```
アクセスが拒否されました。
```

### 原因
- 管理者権限が必要な操作を実行しようとしている
- ファイル/フォルダの権限不足

### 解決方法
```powershell
# macOSではsudoを使用して管理者権限で実行
# sudo pwsh
# または特定のコマンドのみ:
# sudo pwsh -File script.ps1

# ファイルの権限を確認
Get-Acl -Path file.txt | Format-List

# macOSのターミナルでファイル権限を確認（bash/zsh）
# ls -l file.txt
# chmod +x script.sh  # 実行権限付与
```

---

## 6. 変数が展開されない

### 問題
```powershell
$name = "World"
echo 'Hello $name'  # 出力: Hello $name
```

### 原因
- シングルクォーテーションを使用している

### 解決方法
```powershell
# ダブルクォーテーションを使用
$name = "World"
echo "Hello $name"  # 出力: Hello World

# 式を展開
echo "Result: $(2 + 2)"  # 出力: Result: 4
```

---

## 7. ファイルが使用中

### エラーメッセージ
```
プロセスはファイル 'file.txt' にアクセスできません。別のプロセスが使用中です。
```

### 原因
- ファイルが他のプログラムで開かれている

### 解決方法
```powershell
# ファイルを使用しているプロセスを探す
$path = "/path/to/file.txt"
Get-Process | Where-Object {$_.Modules.FileName -eq $path}

# macOSのターミナルでファイルを使用しているプロセスを探す
# lsof /path/to/file.txt

# プロセスを終了
Stop-Process -Name "TextEdit" -Force
```

---

## 8. 配列・コレクション関連エラー

### エラーメッセージ
```
インデックスが配列の境界外です。
```

### 原因
- 存在しないインデックスにアクセスしている

### 解決方法
```powershell
# 配列の長さを確認
$array = @(1, 2, 3)
$array.Count

# 存在確認してからアクセス
if ($array.Count -gt 2) {
    $array[2]
}

# 安全に取得
$value = $array | Select-Object -Index 2 -ErrorAction SilentlyContinue
```

---

## 9. JSONパースエラー

### エラーメッセージ
```
ConvertFrom-Json : 予期しないトークンが見つかりました
```

### 原因
- JSON形式が不正
- エスケープ処理が必要

### 解決方法
```powershell
# ファイルから読み込む
$json = Get-Content -Path data.json -Raw | ConvertFrom-Json

# エラーハンドリング
try {
    $json = Get-Content -Path data.json -Raw | ConvertFrom-Json
} catch {
    Write-Host "JSON解析エラー: $_"
}

# 単一行のJSON文字列
$jsonString = '{"name":"test","value":123}'
$object = $jsonString | ConvertFrom-Json
```

---

## 10. リモート実行エラー

### エラーメッセージ
```
WinRM クライアントは要求を処理できません。
```

### 原因
- リモート実行が許可されていない

### 解決方法（Windows向け - WinRM）
```powershell
# WinRMサービスを起動
Start-Service WinRM

# リモート実行を有効化
Enable-PSRemoting -Force

# リモート接続をテスト
Test-WSMan -ComputerName localhost
```

### macOS/Linuxでのリモート実行
```powershell
# macOSではSSHベースのリモーティングを使用
# SSHリモーティングを有効化
# sudo pwsh
# Install-Module -Name Microsoft.PowerShell.RemotingTools

# SSH経由でリモート実行
Enter-PSSession -HostName hostname -UserName username
```

---

## 11. パイプライン関連エラー

### 問題
```powershell
# 期待と異なる結果
Get-ChildItem | Where-Object Name -eq "test.txt"
```

### 原因
- パイプラインの使い方が不適切

### 解決方法
```powershell
# 正しい構文
Get-ChildItem | Where-Object {$_.Name -eq "test.txt"}

# 簡易構文
Get-ChildItem | Where-Object Name -eq "test.txt"

# Select-Objectで絞り込み
Get-Process | Select-Object Name, CPU | Where-Object {$_.CPU -gt 100}
```

---

## 12. 日付・時刻の扱い

### 問題
```powershell
# 文字列として扱われる
$date = "2025-11-13"
$date.AddDays(1)  # エラー
```

### 解決方法
```powershell
# DateTime型に変換
$date = [DateTime]"2025-11-13"
$date.AddDays(1)

# Get-Dateを使用
$today = Get-Date
$yesterday = $today.AddDays(-1)

# フォーマット指定
$today.ToString("yyyyMMdd")  # 20251113
```

---

## 13. Null参照エラー

### エラーメッセージ
```
null 値の式ではメソッドを呼び出せません。
```

### 原因
- 変数がnullなのにメソッドやプロパティを参照している

### 解決方法
```powershell
# Null確認
if ($null -ne $variable) {
    $variable.Property
}

# Null合体演算子（PowerShell 7以降）
$result = $variable ?? "default"

# デフォルト値を設定
$value = if ($variable) { $variable } else { "default" }
```

---

## 14. ファイルパスの区切り文字

### 問題
```powershell
# macOSではスラッシュ（/）を使用
# バックスラッシュはエスケープ文字として解釈される場合がある
$path = "/Users/username/workspace"
```

### 解決方法
```powershell
# macOSではスラッシュを使用
$path = "/Users/username/workspace"

# ダブルクォーテーション内でも問題なし
$path = "/Users/username/My Documents/workspace"

# Join-Pathを使用（推奨・クロスプラットフォーム対応）
$path = Join-Path -Path "/Users/username" -ChildPath "workspace"

# [System.IO.Path]を使用（クロスプラットフォーム対応）
$path = [System.IO.Path]::Combine("/Users/username", "workspace")
```

---

## トラブルシューティングの基本手順

1. **エラーメッセージ全体を確認**
   ```powershell
   # 詳細なエラー情報
   $Error[0] | Format-List -Force
   ```

2. **コマンドのヘルプを確認**
   ```powershell
   Get-Help Get-ChildItem -Full
   Get-Help Get-ChildItem -Examples
   ```

3. **変数の内容を確認**
   ```powershell
   $variable | Get-Member
   $variable.GetType()
   ```

4. **デバッグモードで実行**
   ```powershell
   Set-PSDebug -Trace 1
   # スクリプト実行
   Set-PSDebug -Off
   ```

5. **エラーアクションを制御**
   ```powershell
   # エラーを停止
   $ErrorActionPreference = "Stop"

   # エラーを無視
   Get-Item "notexist.txt" -ErrorAction SilentlyContinue
   ```

---

**最終更新日**: 2025年11月13日
**バージョン**: 1.0
