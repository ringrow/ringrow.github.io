# DMARC アナライザー — Outlook アドイン

## ファイル構成

```
dmarc-addin/
├── manifest.xml      ← Outlookに登録するマニフェスト
├── taskpane.html     ← アドインのUI本体
└── README.md
```

---

## デプロイ手順

### 1. taskpane.html を HTTPS でホスティング

Office アドインは **HTTPS** が必須です。以下のいずれかにアップロードしてください。

| サービス | 方法 |
|---------|------|
| Azure Static Web Apps | 無料枠あり、おすすめ |
| GitHub Pages | リポジトリを public にすれば無料 |
| 社内 Web サーバー | IIS / nginx で HTTPS を有効化 |

例）GitHub Pages の場合:
```
https://your-username.github.io/dmarc-addin/taskpane.html
```

### 2. manifest.xml を編集

`YOUR-DOMAIN` を実際のURLに一括置換します。

```xml
<!-- 変更前 -->
<SourceLocation DefaultValue="https://YOUR-DOMAIN/taskpane.html"/>

<!-- 変更後（例） -->
<SourceLocation DefaultValue="https://your-username.github.io/dmarc-addin/taskpane.html"/>
```

アイコン画像（16×16, 32×32, 80×80 px の PNG）も同じ場所に置いてURLを更新してください。

### 3. Outlookへのインストール

#### 方法A: 個人インストール（自分だけ）
1. Outlook on the web (outlook.com / Microsoft 365) を開く
2. 歯車アイコン → **アドインを管理**
3. 「**カスタムアドイン**」→「**ファイルから追加**」
4. `manifest.xml` をアップロード

#### 方法B: 組織展開（管理者）
1. Microsoft 365 管理センター → **設定** → **統合アプリ**
2. 「**アプリをアップロード**」→ manifest.xml を選択
3. 展開対象ユーザーを選択して完了

#### 方法C: ローカルテスト（開発者向け）
```bash
# Node.js と office-addin-debugging をインストール
npm install -g office-addin-debugging

# ローカルサーバーを起動（http-server等）
npx http-server . --ssl --cert ./localhost.crt --key ./localhost.key

# Outlook デスクトップでサイドロード
# ファイル → オプション → トラストセンター → アドイン → マニフェストフォルダを指定
```

---

## 使い方

1. Outlookのリボンに「**DMARC 分析**」ボタンが表示される
2. クリックすると右側にタスクペインが開く
3. DMARCレポートのXMLファイルをドロップまたはファイル選択
4. SPF / DKIM / DMARC の認証結果が一覧表示される

---

## 動作環境

| クライアント | 対応 |
|-------------|------|
| Outlook on the web | ✅ |
| Outlook 2016 以降（Windows） | ✅ |
| Outlook 2016 以降（Mac） | ✅ |
| Outlook Mobile（iOS/Android） | ⚠️ 基本機能のみ |
| Outlook 2013 | ❌ |
