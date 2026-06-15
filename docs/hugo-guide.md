# Hugo コンテンツ作成マニュアル（初学者向け）

このサイト（Hugo + `hermit-v2` テーマ）で**ページや記事を作る方法**を、
プログラミング初心者でも順番にたどれるようにまとめたガイドです。

---

## 0. まず全体像をつかむ

Hugo は「**Markdown で書いた文章を、自動で HTML（ウェブページ）に変換する道具**」です。

```
content/ に Markdown を置く  →  hugo がビルド  →  public/ に完成HTMLが出る  →  公開
   （あなたが書く）              （自動）            （見られない/触らない）
```

ポイントは3つだけ：

1. **書く場所は `content/` フォルダだけ**。ここに `.md` ファイルを置く。
2. `public/` は Hugo が自動生成する完成品。**自分で編集しない**（このリポジトリでは Cloudflare が自動生成します）。
3. 設定は `hugo.toml`（サイト名・メニュー・SNSリンクなど）にまとめる。

---

## 1. 準備（最初の1回だけ）

```bash
# テーマを取得（このPCで初めて作業するとき）
git submodule update --init --recursive

# Hugo が入っていなければインストール
brew install hugo
```

確認：

```bash
hugo version
# hugo v0.16x.x+extended ... と出ればOK（"extended" が必要）
```

---

## 2. プレビュー（書きながら確認する）

```bash
hugo server -D
```

- ブラウザで **http://localhost:1313** を開くと、今のサイトが見られます。
- `-D` は「下書き（draft）も表示する」オプション。
- ファイルを保存すると**自動で画面が更新**されます。
- 止めるときは `Ctrl + C`。

> まずこれを起動しっぱなしにして、以下の作業をすると変化がすぐ見えて分かりやすいです。

---

## 3. 記事（ブログ投稿）を作る

### 3-1. コマンドで雛形を作る

```bash
hugo new posts/my-first-post.md
```

- `content/posts/my-first-post.md` が自動で作られます。
- ファイル名（`my-first-post`）が URL の一部になります。**英数字とハイフン**で付けるのが安全（日本語ファイル名は避ける）。

### 3-2. 中身を見てみる

作られたファイルの先頭はこうなっています（**フロントマター**と呼ぶ設定部分）：

```markdown
---
title: "My First Post"
date: 2026-06-16T10:00:00+09:00
draft: true
toc: false
images:
tags:
  - untagged
---

ここに本文を Markdown で書きます。
```

| 項目 | 意味 |
|---|---|
| `title` | 記事のタイトル（日本語OK）。例：`"はじめての投稿"` |
| `date` | 公開日時 |
| `draft` | `true` = 下書き（本番に出ない）。**公開するときは `false` に変える** |
| `toc` | `true` で目次を自動表示 |
| `tags` | タグ（複数可）。`- 日記` のように書く |

### 3-3. 本文を書く

`---` の下に Markdown で書きます。よく使う書き方：

```markdown
## 見出し（大）
### 見出し（小）

普通の文章。**太字**、*斜体*、`コード`。

- 箇条書き1
- 箇条書き2

1. 番号付き1
2. 番号付き2

[リンク文字](https://example.com)

![画像の説明](/images/photo.jpg)

> 引用文

​```python
print("コードブロックはコピーボタン付きで表示されます")
​```
```

### 3-4. 公開する

書き終えたら、フロントマターの `draft: true` を **`draft: false`** に変更します。
これで次のビルドからサイトに表示されます。

---

## 4. トップページ・プロフィールを整える（`hugo.toml`）

サイト名やプロフィール、メニュー、SNSリンクは `hugo.toml` に書きます。
今は最小限の設定なので、下記をコピーして自分用に書き換えると見栄えが整います。

```toml
baseURL = 'https://myhomepage.pages.dev/'
languageCode = 'ja-jp'
title = 'あなたのサイト名'
theme = 'hermit-v2'
enableRobotsTXT = true
enableEmoji = true

[taxonomies]
  tag = "tags"

[params]
  homeSubtitle = "トップページに出る一言（肩書きや紹介）"
  footerCopyright = "© 2026 あなたの名前"
  readTime = true              # 記事の読了時間を表示
  code_copy_button = true      # コードのコピーボタン
  scrollToTop = true           # トップへ戻るボタン

  [params.author]
    name = "あなたの名前"
    about = "自己紹介の短い文"

  # トップに並ぶSNSアイコン（不要な行は消す）
  [[params.socialLinks]]
    name = "github"
    url = "https://github.com/kigasudayooo"
  [[params.socialLinks]]
    name = "x"
    url = "https://x.com/あなたのID"

# 画面下のメニュー
[menu]
  [[menu.main]]
    name = "記事"
    url = "posts/"
    weight = 10
  [[menu.main]]
    name = "About"
    url = "about/"
    weight = 20
```

> `[[params.socialLinks]]` の `name` に使えるアイコン例：`github` `x` `mastodon` `linkedin` `email` など。

---

## 5. About（自己紹介）ページを作る

```bash
hugo new about.md
```

`content/about.md` ができるので、`draft: false` にして自己紹介を書きます。
上の `[menu]` で `url = "about/"` と設定したので、メニューの「About」から開けます。

---

## 6. 画像を載せる

1. `static/images/` フォルダを作り、画像（例 `photo.jpg`）を入れる。
2. 記事内で次のように書く：

```markdown
![説明文](/images/photo.jpg)
```

> `static/` の中身はビルド時にサイト直下へコピーされます。
> なので `static/images/photo.jpg` は URL では `/images/photo.jpg` になります。

---

## 7. 公開する（このサイトの場合）

このサイトは **GitHub に push すると Cloudflare Pages が自動でビルド・公開**します。

```bash
git add .
git commit -m "記事を追加"
git push
```

数十秒〜数分で `https://<あなたのプロジェクト>.pages.dev` に反映されます。
（Cloudflare との初回接続だけは別途ダッシュボードで設定が必要。詳細は `README.md` 参照）

---

## 8. よくあるつまずき

| 症状 | 原因と対処 |
|---|---|
| 記事が表示されない | `draft: true` のまま。`false` に変える。プレビューは `hugo server -D` |
| `hugo server` でエラー | テーマ未取得。`git submodule update --init --recursive` を実行 |
| 画像が出ない | パスを確認。`static/images/x.jpg` → 記事では `/images/x.jpg` |
| 見た目が崩れる | `extended` 版の Hugo が必要。`hugo version` に `+extended` があるか確認 |
| 日付順がおかしい | `date` の値を確認。未来の日付は公開されないことがある |

---

## 9. もっと詳しく

- Hugo 公式（日本語あり）: https://gohugo.io/documentation/
- テーマ `hermit-v2` のデモ兼ドキュメント: https://1bl4z3r.github.io/hermit-V2
- 設定項目の解説: https://1bl4z3r.github.io/hermit-V2/en/posts/explaining-configs/

---

### チートシート（最小手順）

```bash
hugo server -D                      # 1. プレビュー起動
hugo new posts/hello.md             # 2. 記事の雛形作成
#   → ファイルを編集し draft: false に
git add . && git commit -m "post"   # 3. 保存
git push                            # 4. 公開（自動デプロイ）
```
