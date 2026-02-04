# Zenn Publish Skill 📝

Zenn.devに記事・本を執筆・公開するためのスキル。GitHubリポジトリ連携方式。

## 前提条件

- ZennアカウントとGitHubリポジトリが連携済み
- リポジトリ: `github.com/<github_account>/articles`（または任意の名前）
- `articles/` ディレクトリに記事、`books/` ディレクトリに本を配置

## トリガー

- 「Zennに記事を書いて」「Zenn記事作成」
- 「Zennに投稿」「Zenn publish」
- 「技術記事を書いて」（type: tech）
- 「アイデア記事を書いて」（type: idea）

## 記事の書き方

### ディレクトリ構成

```
<repo>/
├── articles/
│   ├── my-first-article.md
│   └── another-article.md
└── books/
    └── my-book/
        ├── config.yaml
        ├── cover.png
        └── chapter1.md
```

### 記事のFront Matter

```markdown
---
title: "記事のタイトル"
emoji: "🚀"
type: "tech"           # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "react", "nextjs"]  # タグ（最大5つ）
published: true        # false で下書き
published_at: 2026-02-04 09:00  # 公開予約（オプション）
---

ここから本文...
```

### Front Matter 各項目

| 項目 | 必須 | 説明 |
|------|------|------|
| `title` | ✅ | 記事タイトル |
| `emoji` | ✅ | アイキャッチ絵文字（1文字） |
| `type` | ✅ | `tech`（技術）or `idea`（アイデア） |
| `topics` | ✅ | タグ配列、最大5つ |
| `published` | ✅ | `true`で公開、`false`で下書き |
| `published_at` | - | 公開日時指定（YYYY-MM-DD HH:MM） |

### slug（ファイル名）のルール

- ファイル名がそのままURLになる
- 例: `my-article.md` → `zenn.dev/<user>/articles/my-article`
- 使える文字: `a-z`, `0-9`, `-`, `_`（1〜50文字）
- 日付プレフィックス推奨: `20260204_article-title.md`

## 公開フロー

```bash
# 1. 記事を作成・編集
vim articles/my-new-article.md

# 2. プレビュー（オプション）
npx zenn preview

# 3. コミット＆プッシュで自動デプロイ
git add articles/my-new-article.md
git commit -m "Add new article: タイトル"
git push origin main
```

**Note:** コミットメッセージに `[ci skip]` または `[skip ci]` を含めるとデプロイがスキップされます。

## 本（Book）の書き方

### ディレクトリ構成

```
books/
└── my-awesome-book/
    ├── config.yaml      # 本の設定（必須）
    ├── cover.png        # カバー画像（500x700px推奨）
    ├── intro.md         # チャプター1
    ├── chapter1.md      # チャプター2
    └── conclusion.md    # チャプター3
```

### config.yaml

```yaml
title: "本のタイトル"
summary: "本の紹介文"
topics: ["topic1", "topic2"]  # 最大5つ
published: true               # false で下書き
price: 0                      # 有料なら 200〜5000（100円単位）
toc_depth: 2                  # 目次の深さ（0〜3）
chapters:
  - intro
  - chapter1
  - conclusion
```

### チャプターのFront Matter

```markdown
---
title: "チャプターのタイトル"
free: true  # 有料本で無料公開する場合
---

チャプター本文...
```

## 画像の挿入方法

1. **Zenn画像アップローダー**: https://zenn.dev/dashboard/uploader
2. **リポジトリ内配置**: `/images/` ディレクトリに配置
3. **外部サービス**: Gyazo等のURL

```markdown
![alt text](https://storage.googleapis.com/zenn-user-upload/...)
![alt text](/images/my-image.png)
```

## Zenn CLI コマンド

```bash
# インストール
npm init --yes
npm install zenn-cli

# 記事作成
npx zenn new:article
npx zenn new:article --slug my-slug --title "タイトル" --type tech --emoji 🚀

# 本作成
npx zenn new:book
npx zenn new:book --slug my-book

# プレビュー
npx zenn preview  # http://localhost:8000
```

## 記事テンプレート

### 技術記事（tech）

```markdown
---
title: "【TypeScript】○○の実装方法"
emoji: "🔧"
type: "tech"
topics: ["typescript", "programming"]
published: false
---

## はじめに

この記事では○○について解説します。

## 環境

- Node.js v20
- TypeScript 5.x

## 実装

### ステップ1

\`\`\`typescript
// コード例
\`\`\`

## まとめ

- ポイント1
- ポイント2

## 参考リンク

- [公式ドキュメント](https://example.com)
```

### アイデア記事（idea）

```markdown
---
title: "○○について考えてみた"
emoji: "💡"
type: "idea"
topics: ["ポエム", "考察"]
published: false
---

## 背景

最近○○について考える機会がありました。

## 本題

...

## おわりに

...
```

## ベストプラクティス

1. **slugは変えない**: 一度公開したら変更不可（URLが変わる）
2. **下書きで確認**: `published: false` でまずプッシュ
3. **絵文字は慎重に**: 記事の顔になる
4. **topicsは検索性重視**: 人気タグを使う
5. **日付プレフィックス**: ファイル管理が楽になる

## 削除について

- 記事・本の削除は **Zennダッシュボード** から行う
- GitHubからファイルを削除してもZenn上では消えない（安全設計）

## 参考リンク

- [Zenn CLIガイド](https://zenn.dev/zenn/articles/zenn-cli-guide)
- [Zenn CLI導入](https://zenn.dev/zenn/articles/install-zenn-cli)
- [slugとは](https://zenn.dev/zenn/articles/what-is-slug)
- [画像のアップロード](https://zenn.dev/zenn/articles/deploy-github-images)
