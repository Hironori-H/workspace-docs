# PowerShell コーディング規約

## PowerShell公式ベストプラクティスに基づくコーディング規約

このドキュメントは、PowerShellの公式スタイルガイドとベストプラクティスを簡潔にまとめたものです。

参考:
- https://learn.microsoft.com/ja-jp/powershell/scripting/developer/cmdlet/
- https://github.com/PoshCode/PowerShellPracticeAndStyle

---

## 命名規則

### コマンドレット・関数名
```powershell
# 動詞-名詞 の形式（PascalCase）
Get-Process
Set-Location
New-Item
Remove-Item

# 承認された動詞を使用
Get-Verb  # 承認された動詞一覧を表示

# 推奨される動詞
# Get, Set, New, Remove, Add, Clear, Copy, Move, など
```

### 変数名
```powershell
# PascalCase（大文字始まり）
$FirstName = "John"
$ItemCount = 10
$IsActive = $true

# ループカウンタなどは小文字も可
for ($i = 0; $i -lt 10; $i++) {
    # 処理
}

# ブール値は Is/Has で始める
$IsValid = $true
$HasError = $false
```

### パラメータ名
```powershell
function Get-UserData {
    param(
        [string]$UserName,      # PascalCase
        [int]$UserId,
        [switch]$IncludeDetails
    )
}
```

---

## インデント・フォーマット

### インデント
```powershell
# スペース4つを推奨
function Get-Data {
    if ($condition) {
        Write-Host "処理中"
        Get-Item "file.txt"
    }
}

# 開き括弧は同じ行
if ($x -eq 1) {
    # 処理
}

# 閉じ括弧は独立した行
```

### 行の長さ
```powershell
# 115文字以内を推奨
# 長い場合は改行とバッククォート
$result = Get-ChildItem -Path "/very/long/path/to/directory" `
    -Filter "*.txt" `
    -Recurse

# パイプラインで改行（推奨）
Get-ChildItem -Path "/path/to/directory" |
    Where-Object {$_.Length -gt 1MB} |
    Sort-Object Length -Descending |
    Select-Object -First 10
```

---

## コメント

### 単一行コメント
```powershell
# コメントは処理の前に記載
# 完全な文で書く

# ファイル一覧を取得
$files = Get-ChildItem

$count = 0  # 行末コメント（最小限に）
```

### ブロックコメント
```powershell
<#
複数行のコメント
長い説明が必要な場合に使用
#>

<#
.SYNOPSIS
    関数の簡潔な説明

.DESCRIPTION
    詳細な説明

.PARAMETER ParameterName
    パラメータの説明

.EXAMPLE
    Get-UserData -UserName "John"
    使用例の説明

.NOTES
    Author: Your Name
    Date: 2025-11-13
#>
function Get-UserData {
    # 関数本体
}
```

---

## 変数の宣言

### 型の明示
```powershell
# 型を明示的に指定（推奨）
[string]$Name = "John"
[int]$Age = 25
[bool]$IsActive = $true
[DateTime]$Date = Get-Date

# 配列
[string[]]$Names = @("John", "Jane", "Bob")
[int[]]$Numbers = @(1, 2, 3, 4, 5)

# ハッシュテーブル
[hashtable]$User = @{
    Name = "John"
    Age = 25
}
```

### スコープ
```powershell
# グローバル
$global:GlobalVar = "value"

# スクリプト
$script:ScriptVar = "value"

# ローカル（デフォルト）
$local:LocalVar = "value"
$LocalVar = "value"  # 同じ

# 環境変数
$env:PATH
```

---

## 関数定義

### 基本的な関数
```powershell
function Get-UserData {
    <#
    .SYNOPSIS
        ユーザーデータを取得

    .PARAMETER UserName
        ユーザー名

    .EXAMPLE
        Get-UserData -UserName "John"
    #>

    param(
        [Parameter(Mandatory=$true)]
        [string]$UserName,

        [Parameter(Mandatory=$false)]
        [switch]$IncludeDetails
    )

    # 処理
    Write-Output "User: $UserName"
}
```

### 高度な関数（Advanced Function）
```powershell
function Get-UserData {
    [CmdletBinding()]
    param(
        [Parameter(
            Mandatory=$true,
            ValueFromPipeline=$true,
            ValueFromPipelineByPropertyName=$true
        )]
        [ValidateNotNullOrEmpty()]
        [string]$UserName,

        [Parameter(Mandatory=$false)]
        [ValidateRange(1, 100)]
        [int]$Count = 10
    )

    begin {
        Write-Verbose "処理開始"
    }

    process {
        # メイン処理
        Write-Output "User: $UserName"
    }

    end {
        Write-Verbose "処理終了"
    }
}
```

---

## パラメータのバリデーション

```powershell
function Test-Validation {
    param(
        # 必須パラメータ
        [Parameter(Mandatory=$true)]
        [string]$Name,

        # Null/空チェック
        [ValidateNotNullOrEmpty()]
        [string]$Email,

        # 範囲チェック
        [ValidateRange(1, 100)]
        [int]$Age,

        # 長さチェック
        [ValidateLength(1, 50)]
        [string]$Username,

        # パターンチェック
        [ValidatePattern("^\d{3}-\d{4}$")]
        [string]$PhoneNumber,

        # セットチェック
        [ValidateSet("Red", "Green", "Blue")]
        [string]$Color,

        # スクリプトチェック
        [ValidateScript({Test-Path $_})]
        [string]$FilePath
    )
}
```

---

## エラーハンドリング

### Try-Catch-Finally
```powershell
# 基本的なエラーハンドリング
try {
    Get-Content -Path "nonexistent.txt" -ErrorAction Stop
}
catch {
    Write-Error "ファイルの読み込みに失敗: $_"
}
finally {
    Write-Verbose "クリーンアップ処理"
}

# 特定のエラー型を捕捉
try {
    $content = Get-Content -Path $FilePath -ErrorAction Stop
}
catch [System.IO.FileNotFoundException] {
    Write-Error "ファイルが見つかりません: $FilePath"
}
catch [System.UnauthorizedAccessException] {
    Write-Error "アクセス権限がありません: $FilePath"
}
catch {
    Write-Error "予期しないエラー: $_"
}
```

### エラーアクションの設定
```powershell
# エラー時に停止
$ErrorActionPreference = "Stop"

# エラーを表示して継続
$ErrorActionPreference = "Continue"

# エラーを無視
$ErrorActionPreference = "SilentlyContinue"

# コマンド単位でエラーアクションを指定
Get-Item "file.txt" -ErrorAction SilentlyContinue
```

---

## 出力

### Write系コマンドレット
```powershell
# 標準出力（パイプライン経由）
Write-Output "結果"

# ホスト出力（画面のみ）
Write-Host "メッセージ" -ForegroundColor Green

# エラー
Write-Error "エラーメッセージ"

# 警告
Write-Warning "警告メッセージ"

# 詳細情報（-Verbose で表示）
Write-Verbose "詳細情報"

# デバッグ情報（-Debug で表示）
Write-Debug "デバッグ情報"

# 情報（-InformationAction で制御）
Write-Information "情報メッセージ"
```

---

## 比較演算子

```powershell
# 等価比較
$x -eq $y    # 等しい
$x -ne $y    # 等しくない

# 大小比較
$x -gt $y    # より大きい
$x -ge $y    # 以上
$x -lt $y    # より小さい
$x -le $y    # 以下

# パターンマッチ
$text -like "*pattern*"      # ワイルドカード
$text -match "regex"         # 正規表現

# 大文字小文字を区別する比較
$x -ceq $y   # 等しい（大文字小文字を区別）
$x -cne $y   # 等しくない（大文字小文字を区別）

# 論理演算子
($x -eq 1) -and ($y -eq 2)
($x -eq 1) -or ($y -eq 2)
-not ($x -eq 1)
!($x -eq 1)
```

---

## ループ

### ForEach
```powershell
# ForEach-Object（パイプライン）
Get-ChildItem | ForEach-Object {
    Write-Host $_.Name
}

# foreach文
foreach ($file in Get-ChildItem) {
    Write-Host $file.Name
}

# For文
for ($i = 0; $i -lt 10; $i++) {
    Write-Host $i
}

# While文
$i = 0
while ($i -lt 10) {
    Write-Host $i
    $i++
}
```

---

## パイプライン

```powershell
# 推奨：各パイプラインを改行
Get-ChildItem -Path "/path/to/directory" |
    Where-Object {$_.Length -gt 1MB} |
    Sort-Object Length -Descending |
    Select-Object Name, Length |
    Format-Table -AutoSize

# フィルタリング
Get-Process |
    Where-Object {$_.CPU -gt 100} |
    Sort-Object CPU -Descending

# 変換
Get-ChildItem |
    ForEach-Object {
        [PSCustomObject]@{
            Name = $_.Name
            Size = $_.Length
            SizeMB = [math]::Round($_.Length / 1MB, 2)
        }
    }
```

---

## ファイル操作

### パスの扱い
```powershell
# Join-Path を使用（推奨）
$path = Join-Path -Path "/Users/username" -ChildPath "Documents"

# [System.IO.Path] も可
$path = [System.IO.Path]::Combine("/Users/username", "Documents")

# リテラルパス（ワイルドカード無効）
Get-Item -LiteralPath "/path/to/[brackets].txt"
```

---

## ベストプラクティス

### 承認された動詞を使用
```powershell
# Yes
Get-Process
Set-Variable
New-Item

# No
Fetch-Process   # 未承認の動詞
Retrieve-Data
```

### エイリアスを避ける（スクリプト内）
```powershell
# Yes（スクリプト内）
Get-ChildItem
Where-Object

# No（スクリプト内）
dir    # エイリアス
ls     # エイリアス
?      # エイリアス

# コマンドラインでは短縮形OK
```

### Splatting を使用
```powershell
# パラメータが多い場合
$params = @{
    Path = "/path/to/directory"
    Filter = "*.txt"
    Recurse = $true
    ErrorAction = "Stop"
}
Get-ChildItem @params
```

### 戻り値を明示
```powershell
function Get-Result {
    # 暗黙的な戻り値を避ける
    $result = "value"

    # Write-Output または return を明示
    return $result
    # または
    Write-Output $result
}
```

---

## テスト

### Pester（テストフレームワーク）
```powershell
# テストの例
Describe "Get-UserData" {
    It "ユーザー名を返す" {
        $result = Get-UserData -UserName "John"
        $result | Should -Be "User: John"
    }

    It "必須パラメータのチェック" {
        {Get-UserData} | Should -Throw
    }
}
```

---

## まとめ

### 優先順位
1. **一貫性を保つ**
2. **読みやすさを優先**
3. **承認された動詞・名詞を使用**
4. **エラーハンドリングを適切に**
5. **コメント・ドキュメントを記載**

### チェックツール
```powershell
# PSScriptAnalyzer（静的解析ツール）
Install-Module -Name PSScriptAnalyzer
Invoke-ScriptAnalyzer -Path .\script.ps1
```

---

**最終更新日**: 2025年11月13日
**バージョン**: 1.0
