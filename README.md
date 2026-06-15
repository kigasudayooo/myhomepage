# myhomepage

Hugo + [hermit-v2](https://github.com/1bl4z3r/hermit-V2) テーマで構築した個人サイト。
[Cloudflare Pages](https://pages.cloudflare.com/) で完全無料・セキュアにホスティングする。

## ローカル開発

```bash
# テーマ submodule を取得（初回のみ）
git submodule update --init --recursive

# Hugo 未インストールなら
brew install hugo

# 開発サーバー起動（http://localhost:1313）
hugo server -D

# 本番ビルド（public/ に出力）
hugo --gc --minify
```

## デプロイ（Cloudflare Pages）

GitHub に push すると自動でビルド・デプロイされる。初回のみ Cloudflare 側で設定する。

1. Cloudflare Dashboard → Workers & Pages → Create → Pages → **Connect to Git** →
   `kigasudayooo/myhomepage` を選択
2. ビルド設定:
   - Framework preset: **Hugo**
   - Build command: `hugo --gc --minify`
   - Build output directory: `public`
3. 環境変数: **`HUGO_VERSION`** = `0.163.1`（ローカル検証済みバージョン）※必須
4. Save and Deploy（テーマ submodule は自動取得される）
5. 発行された `https://<project>.pages.dev` を確認し、`hugo.toml` の `baseURL` を
   その URL に修正して再コミット → 自動再デプロイ

## セキュリティ

- `static/_headers` でセキュリティヘッダ（HSTS / CSP / X-Frame-Options 等）を配信。
  Hugo が `public/_headers` として出力し、Cloudflare Pages が読み取る。
- SSL・WAF・DDoS 防御は Cloudflare が無料で提供。
- 任意強化: ダッシュボードで Always Use HTTPS / Bot Fight Mode を有効化。
