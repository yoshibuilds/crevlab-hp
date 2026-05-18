# Clarix HP — GitHub Pages 公開手順

## 事前準備

- GitHubアカウントが必要です（未作成の場合は https://github.com で無料作成）
- Git がインストールされていること（`git --version` で確認）

---

## Step 1：GitHubリポジトリを作成する

1. https://github.com/new を開く
2. Repository name: `clarix-hp`（または `clarix.github.io` にすると専用ドメインになる）
3. Public を選択
4. "Create repository" をクリック

---

## Step 2：ファイルをアップロードする

### ターミナル（PowerShell）でコマンドを実行：

```powershell
# clarix-hpフォルダへ移動
cd C:\Users\naoki\ai-company\projects\apps\clarix-hp

# Gitを初期化
git init
git add index.html
git commit -m "Initial HP release"

# GitHubに接続（YOUR_USERNAMEをあなたのGitHubユーザー名に置き換える）
git remote add origin https://github.com/YOUR_USERNAME/clarix-hp.git
git branch -M main
git push -u origin main
```

### 生活費シミュレーターも同梱する場合：

```powershell
# life-budgetフォルダごとコピーしてからadd
xcopy /E /I ..\life-budget life-budget
git add .
git commit -m "Add life-budget tool"
git push
```

---

## Step 3：GitHub Pages を有効にする

1. リポジトリページの **Settings** タブを開く
2. 左メニューの **Pages** をクリック
3. **Source** → "Deploy from a branch" を選択
4. **Branch** → `main` / `/ (root)` を選択
5. **Save** をクリック

数分後に以下のURLで公開されます：
```
https://YOUR_USERNAME.github.io/clarix-hp/
```

---

## Step 4：（オプション）カスタムドメインを設定する

独自ドメイン（例：clarix.com）を持っている場合：

1. Pages設定の **Custom domain** に `clarix.com` を入力して Save
2. ドメインのDNS設定で以下のAレコードを追加：
   ```
   185.199.108.153
   185.199.109.153
   185.199.110.153
   185.199.111.153
   ```
3. "Enforce HTTPS" にチェックを入れる

---

## 更新方法（コンテンツ変更後）

```powershell
cd C:\Users\naoki\ai-company\projects\apps\clarix-hp
git add index.html
git commit -m "Update: ○○を追加"
git push
```

プッシュ後、数分でサイトに反映されます。

---

## トラブルシューティング

| 問題 | 解決策 |
|------|--------|
| pushでエラーになる | `git config --global user.email "your@email.com"` を先に実行 |
| Pages設定が見つからない | リポジトリがPublicになっているか確認 |
| サイトが表示されない | 5〜10分待ってからリロード |
| life-budgetのリンクが切れる | `../life-budget/index.html` → `./life-budget/index.html` に修正 |
