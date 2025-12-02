# Workspace プロジェクト概要

**プロジェクト名**: After Effects / Unreal Engine 自動化システム
**開始日**: 2025年11月
**管理者**: ユーザー様 + Claude Code
**最終更新日**: 2025年11月17日

---

## 概要

このプロジェクトは、Adobe After Effects および Unreal Engine の作業を自動化し、デザイナーの作業効率を大幅に向上させることを目的としています。

### 主要コンポーネント

1. **After Effects 自動化システム (AE_Monitor)**
   - .aepファイルの自動処理
   - レイヤー情報のJSON出力
   - PostgreSQLデータベースへの自動格納
   - ダイアログ抑制による完全自動化

2. **Unreal Engine 自動化システム**
   - レンダリング自動化
   - パラメータバッチ変更
   - マテリアルインスタンス管理

3. **作業管理システム**
   - 日付ベースのフォルダ構造
   - 作業ログの詳細記録
   - レビュープロセスの標準化
   - アーカイブ管理

---

## プロジェクト目標

### 短期目標（1〜3ヶ月）
- After Effects 完全自動化の実現
- データベース設計の確立
- 作業プロセスの標準化

### 中期目標（3〜6ヶ月）
- Unreal Engine との連携強化
- デザイナー作業効率の定量的測定
- ユースケースの拡充

### 長期目標（6ヶ月〜）
- 完全自動化パイプラインの構築
- 他プロジェクトへの横展開
- AIアシスタント統合（Claude Code）の最適化

---

## Workspace構造

### ディレクトリ構成

```
workspace/
├── DOCS/                        # ドキュメント管理フォルダ
│   ├── README.md               # このファイル（プロジェクト概要）
│   ├── FAQ.md                  # よくある質問
│   ├── 用語集.md                # プロジェクト固有の用語
│   ├── 作業前提条件_Worker.md   # Worker（実装担当）用ガイド
│   ├── 作業前提条件_Reviewer.md # Reviewer（レビュー担当）用ガイド
│   └── 作業前提条件_Maintainer.md # Maintainer（管理担当）用ガイド
│
├── batch_file/                 # 共通スクリプト・バッチファイル
│   ├── templates/              # テンプレート集
│   └── *.bat, *.ps1            # 実行スクリプト
│
├── database_difinition/        # データベース定義
│   ├── AEA_DB設計書.md
│   └── AEA_DB_setup.sql
│
├── usecase_difinition/         # ユースケース定義
│   ├── AS-IS_TO-BE_業務可視化.md
│   └── デザイナー作業効率化アイディア集.txt
│
├── archive/                    # アーカイブ（過去の作業フォルダ）
│   └── YYYY/MM-MonthName/YYYYMMDD/
│
└── YYYYMMDD/                   # 日付別作業フォルダ
    ├── YYYYMMDD_worklog.md    # 作業ログ（必須）
    ├── reject_file/            # 却下されたファイル
    └── *.md, *.py, *.bat       # 作業成果物
```

### フォルダの役割

#### 固定フォルダ
- **DOCS/**: プロジェクト全体のドキュメント管理
- **batch_file/**: 共通で使用するスクリプト・バッチファイル
- **database_difinition/**: データベース設計・定義ファイル
- **usecase_difinition/**: ユースケース・業務分析ドキュメント
- **archive/**: 過去の作業を年・月別に整理

#### 日付フォルダ（YYYYMMDD形式）
- 毎日の作業はその日の日付フォルダで実施
- 作業ログ（YYYYMMDD_worklog.md）を必ず作成
- 完了後は定期的にアーカイブへ移動

---

## 役割分担

このプロジェクトでは、Claude Codeが3つの役割を担当します：

### 1. Worker（実装・作業担当）
- 機能の実装
- スクリプトの作成
- バグ修正
- ドキュメント作成

### 2. Reviewer（レビュー・評価担当）
- 成果物のレビュー
- 品質評価
- 問題点の指摘
- 修正指示書の作成

### 3. Maintainer（管理・整備担当）
- Workspace全体の管理
- アーカイブ管理
- ドキュメント体系の整備
- プロジェクト改善提案

詳細は各役割の作業前提条件ドキュメントを参照してください。

---

## 主要技術スタック

### 開発環境
- **OS**: Windows 11
- **エディタ**: Visual Studio Code
- **バージョン管理**: Git

### After Effects 自動化
- **ExtendScript**: SimpleTrigger.jsx
- **Node.js**: automation システム
- **データベース**: PostgreSQL
- **ログ**: Winston

### Unreal Engine 自動化
- **Python**: Unreal Editor Script
- **バージョン**: Unreal Engine 5.3

### スクリプト言語
- **Python**: 3.9+
- **PowerShell**: 7+
- **Batch**: Windows CMD

---

## 作業フロー

### 標準的な作業の流れ

1. **作業開始**
   - 本日の日付フォルダを作成（例: 20251117）
   - 作業ログ（YYYYMMDD_worklog.md）を作成

2. **実装作業（Worker）**
   - 機能の実装
   - スクリプトの作成
   - 作業ログへの記録

3. **レビュー（Reviewer）**
   - 成果物のレビュー
   - レビュー結果の記録
   - 修正指示書の作成（必要な場合）

4. **修正作業（Worker）**
   - 修正指示書に従って修正
   - 再度レビューを依頼

5. **完了**
   - 作業ログのステータスを「完了」に更新
   - 一定期間経過後、アーカイブへ移動

---

## 命名規則

### ファイル名
- **スクリプトファイル（.py, .bat, .ps1）**: 英数字のみ
  - 例: `setup_project_config.py`
- **ドキュメントファイル（.md, .txt）**: 日本語可
  - 例: `作業ログ_20251117.md`
- **バージョン表記**: `_vX.Y` 形式
  - 例: `script_v1.2.py`

### フォルダ名
- **日付フォルダ**: `YYYYMMDD` 形式（例: 20251117）
- **アーカイブ**: `YYYY/MM-MonthName/YYYYMMDD` 形式

---

## よくあるタスク

### アーカイブの実施
```powershell
# 11月のフォルダをアーカイブ
.\batch_file\archive_november.ps1
```

### After Effects 自動化のテスト
```batch
# テスト用バッチファイルを実行
.\20251117\launch_ae_automation_test.bat
```

### データベースのセットアップ
```bash
# PostgreSQLにDB設計を適用
psql -U postgres -d AEA_DB -f database_difinition/AEA_DB_setup.sql
```

---

## トラブルシューティング

一般的な問題の解決方法については、[FAQ.md](FAQ.md) を参照してください。

---

## プロジェクトの進捗状況

### 完了済み
- ✅ After Effects ダイアログ抑制機能（Phase 1完了）
- ✅ 作業前提条件ドキュメント整備（Worker/Reviewer/Maintainer）
- ✅ アーカイブ管理システムの構築
- ✅ 日付ベースのフォルダ構造の確立

### 進行中
- 🔄 After Effects JSON出力の改善
- 🔄 データベーススキーマの最適化

### 今後の予定
- 📅 Unreal Engine との連携強化
- 📅 デザイナー向けGUIツールの開発
- 📅 完全自動化パイプラインの構築

---

## リンク・参考資料

### 内部ドキュメント
- [作業前提条件_Worker.md](作業前提条件_Worker.md)
- [作業前提条件_Reviewer.md](作業前提条件_Reviewer.md)
- [作業前提条件_Maintainer.md](作業前提条件_Maintainer.md)
- [FAQ.md](FAQ.md)
- [用語集.md](用語集.md)

### 外部リンク
- [Adobe After Effects Scripting Guide](https://ae-scripting.docsforadobe.dev/)
- [Unreal Engine Python API](https://docs.unrealengine.com/5.3/en-US/PythonAPI/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)

---

## 連絡先・サポート

### プロジェクト管理者
- ユーザー様

### AIアシスタント
- Claude Code (Anthropic Claude Sonnet 4.5)

### 問題報告
- 作業ログ（YYYYMMDD_worklog.md）の「課題・問題点」セクションに記録
- 重大な問題はユーザー様に直接報告

---

## ライセンス・著作権

このプロジェクトは社内プロジェクトです。
外部への公開・配布は禁止されています。

---

**最終更新日**: 2025年11月17日
**バージョン**: 1.0
