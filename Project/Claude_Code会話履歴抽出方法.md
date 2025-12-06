# Claude Code 会話履歴抽出方法

**作成日**: 2025-11-18
**目的**: Claude Codeのセッションが停止・固まった場合に、会話履歴を抽出してworklogに追記する方法

---

## 会話履歴の保存場所

Claude Codeは会話履歴をローカルに自動保存しています。

### 保存先ディレクトリ
```
C:\Users\CLPC-63\.claude\debug\
```

### ファイル形式
- **ファイル名**: UUID形式（例: `bb684fbf-75b1-48bf-8c94-70a7e448d485.txt`）
- **内容**: 各セッションの会話ログ（テキスト形式）
- **更新**: 会話が進むたびに自動更新

---

## 抽出手順

### ステップ1: 最新のデバッグファイルを特定

**コマンド**:
```powershell
powershell -Command "Get-ChildItem 'C:\Users\CLPC-63\.claude\debug' -File | Sort-Object LastWriteTime -Descending | Select-Object -First 10 | Format-Table Name, Length, LastWriteTime -AutoSize"
```

**出力例**:
```
Name                                     Length LastWriteTime
----                                     ------ -------------
bb684fbf-75b1-48bf-8c94-70a7e448d485.txt 233372 2025/11/18 16:42:42
053d5c3b-8d26-467c-b2e9-90c0b7110178.txt  99604 2025/11/18 16:42:56
```

**確認ポイント**:
- `LastWriteTime` が最新のもの
- `Length` が大きいもの（長い会話ほどファイルサイズが大きい）

---

### ステップ2: デバッグファイルを読み込む

#### 方法A: Claude Code (Worker) で読み込む
```markdown
Claude Code (Worker) に以下を指示:
「C:\Users\CLPC-63\.claude\debug\[ファイル名].txt を読み込んで、会話内容を抽出してください」
```

#### 方法B: PowerShellで直接読み込む
```powershell
powershell -Command "Get-Content 'C:\Users\CLPC-63\.claude\debug\[ファイル名].txt'"
```

#### 方法C: テキストエディタで開く
```
VS Code等のエディタで直接開いて確認
```

---

### ステップ3: worklogに整理して追記

抽出した会話履歴を以下の形式でworklogに追記:

```markdown
## Phase X: [作業内容]（Reviewerとのやり取り）

**対話開始**: YYYY-MM-DD HH:MM
**参加者**: User, Claude Code (Reviewer)
**セッションID**: [ファイル名のUUID部分]

### ユーザーからの質問・指示
[会話内容を整理して記載]

### Reviewerの回答・提案
[会話内容を整理して記載]

### 決定事項
- [決まったこと1]
- [決まったこと2]

### 未完了事項
- [残っているタスク]

### 参照ファイル
- 元の会話ログ: `C:\Users\CLPC-63\.claude\debug\[ファイル名].txt`
```

---

## その他の履歴ファイル

### history.jsonl
```
C:\Users\CLPC-63\.claude\history.jsonl
```
- セッション一覧
- 各セッションのメタデータ

### file-history
```
C:\Users\CLPC-63\.claude\file-history\
```
- ファイル編集履歴
- バージョン管理

---

## トラブルシューティング

### Q1: デバッグファイルが見つからない
**対処**:
- `.claude` フォルダが存在するか確認
- Claude Codeが正常に起動しているか確認
- 別のユーザープロファイルで実行していないか確認

### Q2: ファイルサイズが0バイト
**対処**:
- セッションがまだ開始されていない可能性
- 最新の更新を待つ

### Q3: どのファイルがどのセッションか分からない
**対処**:
- `LastWriteTime` で時系列を確認
- 複数のファイルを読んで内容から判断

---

## 注意事項

1. **プライバシー**: デバッグファイルには会話内容が含まれるため、取り扱いに注意
2. **定期的なクリーンアップ**: 古いデバッグファイルは定期的に削除推奨
3. **バックアップ**: 重要な会話履歴は別途バックアップ推奨

---

## 活用例

### ケース1: Reviewerセッションが固まった
1. 最新のデバッグファイルを特定
2. Workerセッションで読み込み
3. worklogに整理して追記

### ケース2: 過去の会話を振り返る
1. `LastWriteTime` から該当日時のファイルを特定
2. 内容を確認
3. 必要に応じてドキュメント化

### ケース3: 複数セッションの内容を統合
1. 複数のデバッグファイルを読み込み
2. 時系列で整理
3. 統合したworklogを作成

---

**最終更新**: 2025-11-18
**バージョン**: 1.0
