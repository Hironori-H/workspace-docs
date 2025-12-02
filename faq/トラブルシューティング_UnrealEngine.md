# Unreal Engine トラブルシューティング

## よくあるエラーと解決方法

---

## 1. プロジェクトが開けない

### エラーメッセージ
```
The following modules are missing or built with a different engine version
```

### 原因
- エンジンのバージョン不一致
- プロジェクトファイルが破損
- モジュールのコンパイルが必要

### 解決方法
1. **.uproject ファイルを右クリック**
   - "Switch Unreal Engine version" で正しいバージョンを選択

2. **プロジェクトを再ビルド**
   - .uproject を右クリック > Generate Visual Studio project files
   - .sln ファイルを開いてビルド

3. **Intermediate、Saved、Binariesフォルダを削除**
   - プロジェクトフォルダから以下を削除:
     - `Intermediate/`
     - `Saved/`
     - `Binaries/`
   - .uproject をダブルクリックして再生成

---

## 2. ブループリントコンパイルエラー

### エラーメッセージ
```
Error: Blueprint could not be compiled
```

### 原因
- ブループリントのロジックエラー
- 参照が壊れている
- 変数の型が一致していない

### 解決方法
1. **エラーログを確認**
   - Output Log パネルで詳細を確認
   - コンパイラタブでエラー箇所を特定

2. **参照を修正**
   - 赤いノードは参照が壊れている
   - 削除して再接続

3. **変数の型を確認**
   - ピンの色が一致しているか確認
   - 必要に応じて型変換ノードを追加

4. **親クラスを確認**
   - Class Settings で親クラスが正しいか確認

---

## 3. パッケージ化エラー

### エラーメッセージ
```
UATHelper: Packaging (Windows): BUILD FAILED
```

### 原因
- コンテンツに問題がある
- プラグインの問題
- ディスク容量不足

### 解決方法
1. **Output Log を確認**
   - Window > Developer Tools > Output Log
   - エラーの詳細を確認

2. **プロジェクトをクリーン**
   - Intermediate、Saved、Binariesフォルダを削除
   - DerivedDataCacheフォルダも削除を検討

3. **Development ビルドで試す**
   - Shipping ではなく Development でパッケージ化
   - エラーメッセージがより詳細になる

4. **不要なプラグインを無効化**
   - Edit > Plugins
   - 使用していないプラグインを無効化

---

## 4. クラッシュ・フリーズ

### 原因
- メモリ不足
- 無限ループ
- グラフィックドライバの問題

### 解決方法
1. **クラッシュログを確認**
   - `Saved/Logs/` フォルダ内のログファイル
   - `Saved/Crashes/` フォルダのクラッシュレポート

2. **セーフモードで起動**
   - コマンドライン引数: `-dx11` または `-dx12`
   - OpenGL モード: `-opengl`

3. **シェーダーキャッシュを削除**
   - `Saved/ShaderCache/` フォルダを削除

4. **グラフィックドライバを更新**
   - NVIDIA/AMD の最新ドライバをインストール

5. **メモリ使用量を確認**
   - Stat Memory コマンドで確認
   - アセットの最適化を検討

---

## 5. ライティングビルドエラー

### エラーメッセージ
```
Lighting build failed. Swarm failed to kick off.
```

### 原因
- Swarm Agent が起動していない
- ライトマップUVが正しくない
- メモリ不足

### 解決方法
1. **Swarm Agent を起動**
   - `Engine/Binaries/DotNET/SwarmAgent.exe` を実行
   - Allow This Machine to Distribute Jobs を有効化

2. **ライトマップ解像度を下げる**
   - スタティックメッシュのライトマップ解像度を64または128に設定

3. **ライトマップUVを確認**
   - スタティックメッシュエディタでUV Channel 1を確認
   - 重複や逆転がないか確認

4. **ライティング品質を下げる**
   - Build > Lighting Quality > Preview
   - Production品質は時間がかかる

---

## 6. アセットのインポートエラー

### エラーメッセージ
```
Failed to import [asset name]
```

### 原因
- ファイル形式が対応していない
- ファイルが破損している
- パスに特殊文字が含まれている

### 解決方法
1. **対応形式を確認**
   - FBX: 推奨形式
   - OBJ: 基本的なメッシュのみ
   - テクスチャ: TGA, PNG, JPEG

2. **FBXエクスポート設定**
   - FBX バージョン: 2014/2015 以降
   - Triangulate を有効化
   - Smoothing Groups を有効化

3. **ファイルパスを確認**
   - 日本語や特殊文字を含まないパスに配置

4. **DCC ツールから再エクスポート**
   - Maya, Blender, 3ds Max などから再度エクスポート

---

## 7. ブループリント実行時エラー

### エラーメッセージ
```
Accessed None trying to read property
```

### 原因
- Nullオブジェクトを参照している
- オブジェクトが存在しない

### 解決方法
1. **Null チェックを追加**
   - "Is Valid" ノードを使用
   - Branch で分岐処理

2. **初期化を確認**
   - BeginPlay で変数を初期化
   - Get All Actors of Class で取得

3. **デバッグ**
   - Print String でデバッグ出力
   - ブレークポイントを設置

---

## 8. パフォーマンス問題

### 原因
- ポリゴン数が多すぎる
- ドローコールが多い
- ライティングが複雑

### 解決方法
1. **Stat コマンドで確認**
   ```
   stat fps          # フレームレート
   stat unit         # フレーム時間
   stat game         # ゲームスレッド
   stat gpu          # GPU負荷
   stat memory       # メモリ使用量
   ```

2. **LOD を設定**
   - スタティックメッシュにLODを追加
   - 距離に応じてポリゴン数を削減

3. **インスタンシングを使用**
   - Instanced Static Mesh Component
   - Hierarchical Instanced Static Mesh

4. **ライティングを最適化**
   - Stationary/Static Light を使用
   - Lightmass Importance Volume を設定

---

## 9. ソース管理の競合

### エラーメッセージ
```
Asset is checked out by another user
```

### 原因
- 他のユーザーがアセットをロックしている
- ソース管理の状態が不整合

### 解決方法
1. **ソース管理の状態を確認**
   - Content Browser で右クリック > Source Control > Refresh
   - 最新の状態を取得

2. **強制的にチェックアウト**
   - 右クリック > Source Control > Check Out
   - 管理者に連絡してロックを解除

3. **ローカルで作業**
   - ソース管理から外して作業
   - 後で統合

---

## 10. シェーダーコンパイルが遅い

### 原因
- 大量のマテリアルやシェーダー
- DDC (Derived Data Cache) が空

### 解決方法
1. **DDC を共有**
   - チームで共有DDCサーバーを使用
   - `BaseEngine.ini` で設定

2. **エンジンを最新に保つ**
   - シェーダーコンパイラの最適化

3. **開発中はシンプルなマテリアル**
   - 複雑なマテリアルは最後に適用

---

## 11. プラグインのコンパイルエラー

### エラーメッセージ
```
Plugin could not be loaded because module could not be loaded
```

### 原因
- プラグインのソースコードに問題
- エンジンバージョンの不一致

### 解決方法
1. **プラグインをビルド**
   - Visual Studio でプラグインのモジュールをビルド

2. **プラグインのバージョンを確認**
   - .uplugin ファイルで EngineVersion を確認

3. **プラグインを無効化**
   - プロジェクト設定でプラグインを無効化して起動

---

## 12. C++ コンパイルエラー

### エラーメッセージ
```
Error C2065: undeclared identifier
```

### 原因
- ヘッダーファイルのインクルード忘れ
- 名前空間の問題
- モジュールの依存関係

### 解決方法
1. **ヘッダーをインクルード**
   ```cpp
   #include "GameFramework/Actor.h"
   #include "Components/StaticMeshComponent.h"
   ```

2. **モジュール依存関係を追加**
   - `[ProjectName].Build.cs` ファイルを編集
   ```csharp
   PublicDependencyModuleNames.AddRange(new string[] {
       "Core", "CoreUObject", "Engine", "InputCore"
   });
   ```

3. **プロジェクトファイルを再生成**
   - .uproject を右クリック > Generate Visual Studio project files

---

## トラブルシューティングの基本手順

1. **ログを確認**
   - Output Log パネル（Window > Developer Tools > Output Log）
   - `Saved/Logs/` フォルダのログファイル

2. **クリーンビルド**
   ```
   Intermediate/、Saved/、Binaries/ フォルダを削除
   .uproject を右クリック > Generate Visual Studio project files
   ビルド
   ```

3. **エンジンのバージョンを確認**
   - Help > About Unreal Editor
   - プロジェクトと一致しているか確認

4. **ドキュメントを参照**
   - https://docs.unrealengine.com/

5. **コミュニティで質問**
   - Unreal Engine フォーラム
   - Answer Hub

---

## 便利なコンソールコマンド

```
stat fps              # FPS表示
stat unit             # パフォーマンス詳細
stat game             # ゲームスレッド情報
stat gpu              # GPU負荷
r.ScreenPercentage 50 # 解像度スケール（パフォーマンステスト用）
showdebug              # デバッグ情報
viewmode wireframe    # ワイヤーフレーム表示
```

---

**最終更新日**: 2025年11月13日
**バージョン**: 1.0
