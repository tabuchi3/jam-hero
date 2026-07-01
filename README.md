# JAM Hero — 公開手順

背景がフェードで切り替わり、Appleライクにテキストがスクロールで立ち上がるページです。
写真と文章は Supabase で管理し、Vercel で公開します。

## 構成

- `index.html` … 公開ページ（訪問者が見る／Supabaseから読むだけ）
- `admin.html` … 管理画面（ログインして写真・文章を編集）
- バックエンド … Supabase プロジェクト `jam-hero`（作成済み・東京リージョン）
  - テーブル `hero`（設定・文章・写真URL）
  - Storage バケット `hero`（写真ファイル・公開読み取り）
  - RLS 設定済み：読み取りは公開、書き込みはログイン必須

接続情報は両HTMLに埋め込み済みです（公開してよい publishable キーなので、そのままGitHubに上げて問題ありません）。

## 手順1：管理者アカウントを作る（最初に1回だけ）

編集できる人を登録します。Supabase の管理画面から行います。

1. https://supabase.com/dashboard → プロジェクト `jam-hero` を開く
2. 左メニュー **Authentication** → **Users** → **Add user** → **Create new user**
3. あなたのメールアドレスと、任意のパスワードを設定して作成
   （「Auto Confirm User」を有効にすると、確認メールなしですぐ使えます）

このメール／パスワードが admin.html のログインに使うものになります。

## 手順2：GitHubに上げる

1. GitHub（`tabuchi3`）で新規リポジトリを作成（例：`jam-hero`、Public/Privateどちらでも可）
2. このフォルダの `index.html` と `admin.html` をアップロード（ドラッグ&ドロップでもOK）

## 手順3：Vercelで公開

1. https://vercel.com にGitHubアカウントでログイン
2. **Add New → Project** → 先ほどのリポジトリを **Import**
3. 設定は変更不要（静的HTMLなのでそのまま **Deploy**）
4. 数十秒で `https://jam-hero-xxxx.vercel.app` のようなURLが発行されます

公開ページ … `https://（発行URL）/`
管理画面 …… `https://（発行URL）/admin.html`

独自ドメインを割り当てたい場合は、Vercelの **Settings → Domains** から追加できます。

## 使い方

1. `/admin.html` を開いてログイン
2. 右下の ⚙ で編集パネルを開く
3. 写真を追加（自動で軽量化＆アップロード）、文章を編集
4. 変更は自動保存され、公開ページ `/` に反映されます

## 補足

- 写真を削除すると Storage からも削除されます
- 公開ページはSupabaseから読み込むため、初回表示にわずかな待ちがあります
- 管理画面URL（/admin.html）は、むやみに人に共有しないでください（ログインは必要ですが、URL自体は伏せておくのが無難です）
