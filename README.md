# gkentei-flashcards — G検定フラッシュカードSPA

2026年5月9日（土）開催 **G検定（JDLA Deep Learning for GENERAL）** 受験対策用の、スマホ・スキマ時間学習向けフラッシュカード単一ページアプリ。

YouTube動画「[【G検定 試験対策 一問一答100分！】｜全8章問題集！#聞き流し](https://youtu.be/BO05K85SBag)」の章立て（JDLA G検定公式シラバス準拠 全8章）に沿って、各章の頻出用語をフラッシュカード化したもの。

## アプリ仕様

- **形式：** HTML + Tailwind CSS（CDN）+ Vanilla JavaScript の単一ページアプリ
- **ビルド工程：** なし。`index.html` を静的配信するだけで動く
- **データ：** `cards.json` を `fetch` で読み込み（CDN・APIなし）
- **永続化：** `sessionStorage` のみ（タブを閉じれば進捗もクリア）。`localStorage` は不使用

### 主要機能

- 章別フィルタ（第1〜8章）／全章まとめて学習
- モード切替：順番／ランダム／**苦手復習（不明マークだけ）**
- カードのフリップ（タップ・スペースキー・⤺ボタン）
- 既知／不明マーキング（自動で次カードへ）
- スマホで横スワイプ → 前後カード移動
- 試験日 2026-05-09 までの**残日数カウントダウン**（ヘッダー右）
- 動画の該当章へジャンプする「▶ 該当章を動画で見る」ボタン
- ライト／ダーク両モード対応（OS追従＋手動上書き）
- PCキーボード操作：`Space/Enter`=フリップ、`←/→`=前後、`1`=不明、`2`=既知

### データ構造

`cards.json`：

```jsonc
{
  "version": "1.0",
  "exam_date": "2026-05-09",
  "source": { "video_id": "BO05K85SBag", "video_url": "...", "note": "..." },
  "chapters": [
    { "id": 1, "title": "人工知能とは？", "start_sec": 70, "video_timestamp_url": "..." }
  ],
  "cards": [
    {
      "id": "ch1-001",
      "chapter": 1,
      "term": "人工知能（AI）",
      "definition": "人間の知的活動を模倣・代替するコンピュータシステム...",
      "explanation": "明確な統一定義はなく...",
      "video_timestamp_url": "https://youtu.be/BO05K85SBag?t=70"
    }
  ]
}
```

## 動作環境

- iOS Safari（最新-1）
- Android Chrome（最新-1）
- PC Chrome / Firefox / Safari（最新-1）

## ローカル起動

`fetch('cards.json')` のCORS制約のため `file://` 直開きでは動かない。簡易HTTPサーバ経由で起動する。

```bash
cd gkentei-flashcards
python3 -m http.server 8000
# ブラウザで http://localhost:8000 にアクセス
```

Node.js環境なら：

```bash
npx serve .
```

## カードを追加・修正する

`cards.json` の `cards` 配列に項目を追加するだけ。`id` は `chN-NNN`（章番号-連番）の規約で重複しないように。

最低限のフィールド：

| フィールド | 必須 | 内容 |
| --- | --- | --- |
| `id` | ✅ | 一意な文字列（`ch3-021` など） |
| `chapter` | ✅ | 1〜8 |
| `term` | ✅ | カード表面に出る用語名 |
| `definition` | ✅ | 1〜2行の定義 |
| `explanation` | 任意 | 1〜3行の補足解説 |
| `video_timestamp_url` | ✅ | `https://youtu.be/BO05K85SBag?t=<秒数>` |

## デプロイ手順テンプレ（後日指定）

リポジトリ URL とデプロイ先は **TBD（後日指定）**。代表的な選択肢を以下にまとめておく。

### A. GitHub Pages

1. リポジトリ作成（例：`https://github.com/<USER>/gkentei-flashcards` ←後日指定）
2. `main` ブランチに push
3. Settings → Pages → Source: `Deploy from a branch` → Branch: `main` / `/ (root)` を選択
4. `https://<USER>.github.io/gkentei-flashcards/` で公開

### B. Vercel

1. リポジトリ作成 → push
2. [vercel.com](https://vercel.com) にログインして「Add New」→「Project」→ 該当リポジトリをimport
3. Framework Preset: `Other`、Build/Output設定はデフォルト（静的配信のみ）
4. Deploy をクリック → 自動で公開URL発行

### C. Cloudflare Pages

Vercelとほぼ同じ流れ。Build commandは空、Output directoryは `/`。

## ファイル一覧

```
gkentei-flashcards/
├── index.html          # SPA本体
├── cards.json          # フラッシュカードデータ（8章 / 129枚）
├── README.md           # このファイル
├── LICENSE             # MIT
├── .gitignore          # macOS/Node等の除外設定
└── screenshots/        # 動作確認スクリーンショット
```

## 出典・注意事項

- 章立て：YouTube動画 `BO05K85SBag` の概要欄チャプターを使用（**動画＝JDLA公式シラバス準拠の全8章構成**）。
- 用語解説：動画の自動字幕は本リポジトリ作成時点で取得不可（YouTube側のPOトークン要件）だったため、**動画の章立て・概要欄に沿って JDLA G検定公式シラバスを基に作成**。
- 出典が動画本編の説明と一字一句一致するわけではないため、最終確認はJDLA公式テキスト・公式問題集を参照のこと。

## ライセンス

MIT License（`LICENSE` 参照）。
