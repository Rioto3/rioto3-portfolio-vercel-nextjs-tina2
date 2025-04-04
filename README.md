# TinaCMS GitHub ブランチ戦略ガイド

## ブランチ構造
```
main (ベースコード/開発基点)
 │
 ├── production (公開サイト用ブランチ - mainからFast-forward専用)
 │
 ├── drafts (コンテンツ編集専用ブランチ)
 │
 └── feature/* (機能開発用ブランチ)
      └── 完成後 → main にマージ
```

## ブランチの役割と権限
| ブランチ | 目的 | 操作可能なパス | 権限 | マージ方法 |
|---------|------|--------------|------|----------|
| `main` | 開発基点・コードベース | すべて | 開発チーム (PR経由) | スカッシュマージ |
| `production` | 公開サイト用 | 直接編集不可 | 開発チーム | Fast-forward only |
| `drafts` | コンテンツ編集用 | `/content/` のみ | コンテンツ編集者 | PR経由でmainへ |
| `feature/*` | 機能開発用 | すべて | 開発チーム | PR経由でmainへ |

## ブランチ保護ルール
### main
- 作成・削除禁止
- PRレビュー承認必須（1名）
- ステータスチェック必須
- スカッシュマージのみ許可
- 強制プッシュ禁止

### production
- 作成・削除禁止
- 直接更新禁止（mainからのみ更新可）
- Fast-forward マージのみ許可
- 強制プッシュ禁止

### drafts
- 作成・削除禁止
- `/content/` 以外の更新禁止
- 強制プッシュ禁止

## ワークフロー
### コンテンツ更新フロー
1. `drafts` ブランチでコンテンツ編集
2. プレビュー環境で確認
3. `main` へのPRを作成（`content-published` ラベル付与）
4. レビュー後、コンテンツをmainへマージ
5. コンテンツバージョンタグを付与（例: `content-v1.2`）
6. `main` から `production` へFast-forwardマージで公開

### 機能開発フロー
1. `main` から `feature/xxx` ブランチを作成
2. 開発・テスト
3. `main` へのPRを作成・レビュー
4. スカッシュマージで `main` へマージ
5. 必要に応じて `main` から `production` へFast-forwardマージ

## コンテンツバージョン管理
- コンテンツの特定バージョンはGitタグで管理（例: `content-v1.0`, `content-v1.1`）
- コンテンツPRには必ず `content-published` ラベルを付与
- 重要なコンテンツ更新後はタグを付与してバージョンを記録
