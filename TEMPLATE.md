# スタジオ水餅 / works.json 作品追加ルール

このファイルはClaudeへの添付用です。ユーザーから別途指示がある場合はそちらを優先してください。

## フィールド定義

| フィールド | 内容 |
|---|---|
| title | 作品タイトル（日本語） |
| year | 制作年（任意） |
| thumb | サムネイル画像パス（省略可） |
| thumbH | サムネイル仮表示の高さ（thumb省略時のみ有効・省略可） |
| content_ja / content_en | モーダル内コンテンツ（日本語／英語） |

## thumb の自動フォールバック（index.html 更新済み）

`thumb` を省略した場合、`content_ja` の最初の `image` ブロックの `src` が自動的にサムネイルとして使用されます。
→ **通常は `thumb` を書かなくてOKです。**

`thumb` が必要なケース：
- サムネイル用に別の画像（トリミング済み等）を使いたい場合

`thumbH` が有効なケース：
- `thumb` も省略、かつ `content_ja` に画像がない場合（プレースホルダー表示時のみ）

## 画像パスのルール

画像ファイルは Portfolio/images/ 以下に置く。

2枚以下の場合：images/ に直置き
  例）images/cafe-mascot.png

3枚以上の場合：images/ 内に作品フォルダを作成
  例）images/vtuber-moca/thumb.png

フォルダ名はユーザーから指定がなければ、作品タイトルをもとに英語の短い名前（kebab-case）を提案する。
  例：カフェマスコット → cafe-mascot、VTuber立ち絵 → vtuber-（キャラ名）

## contentブロックの種類

{ "type": "image",     "src": "パス", "alt": "説明" }
{ "type": "image-row", "images": [{"src":"","alt":""},{"src":"","alt":""}] }
{ "type": "heading",   "text": "見出し" }
{ "type": "text",      "text": "本文（\nで改行可）" }
{ "type": "divider" }
{ "type": "link",      "href": "URL", "text": "リンクテキスト" }

## 進行ルール

情報が不足している場合は生成前に質問する。
全項目が揃ったら「生成前確認」として以下を出力し、ユーザーの承認を待つ。

### 生成前確認の出力形式

- **タイトル**：
- **画像**：枚数・配置方法（直置き or フォルダ）
- **thumb**：省略（自動）or 明示指定
- **画像構成**：順序と内容
- **リンク**：リンクテキスト → URL
- **本文（日本語）**：
（整形済みの日本語本文をそのまま出力。ユーザー入力をベースに句読点・改行を整える）
- **本文（英語）**：
（日本語本文をClaudeが自然な英語に翻訳して出力）

### JSON生成のタイミング

ユーザーが確認OKを出した後にのみJSONを生成する。
JSONはコピペできる形（コードブロック）で出力し、works.jsonへの追記はしない。

## 生成例（1枚・直置き・thumbなし / パターンA ★デフォルト）

{
  "title": "【創作】秋色くるみ",
  "year": "2026",
  "content_ja": [
    { "type": "image", "src": "images/kurumi.webp", "alt": "秋色くるみ 完成イラスト" },
    { "type": "heading", "text": "About this work" },
    { "type": "text", "text": "日本語説明文。" },
    { "type": "link", "href": "URL", "text": "リンクテキスト" }
  ],
  "content_en": [
    { "type": "image", "src": "images/kurumi.webp", "alt": "Final illustration" },
    { "type": "heading", "text": "About this work" },
    { "type": "text", "text": "English description." },
    { "type": "link", "href": "URL", "text": "Link text" }
  ]
}
※ thumb省略 → content_ja[0]の画像が自動でサムネイルになる

## 生成例（2枚以下・直置き・thumb明示 / パターンB）

{
  "title": "カフェマスコット",
  "year": "2025",
  "thumb": "images/cafe-mascot.png",
  "content_ja": [
    { "type": "image", "src": "images/cafe-mascot.png", "alt": "カフェマスコット完成" },
    { "type": "heading", "text": "About this work" },
    { "type": "text", "text": "日本語説明文。" }
  ],
  "content_en": [
    { "type": "image", "src": "images/cafe-mascot.png", "alt": "Café mascot final" },
    { "type": "heading", "text": "About this work" },
    { "type": "text", "text": "English description." }
  ]
}

## 生成例（3枚以上・フォルダあり / パターンC）

{
  "title": "○○さん VTuber立ち絵",
  "year": "2025",
  "content_ja": [
    { "type": "image", "src": "images/vtuber-moca/final.png", "alt": "完成イラスト" },
    { "type": "heading", "text": "About this work" },
    { "type": "text", "text": "日本語説明文。" },
    { "type": "divider" },
    { "type": "heading", "text": "Process" },
    { "type": "image-row", "images": [
      { "src": "images/vtuber-moca/rough.png", "alt": "ラフスケッチ" },
      { "src": "images/vtuber-moca/line.png",  "alt": "線画" }
    ]},
    { "type": "text", "text": "制作過程の補足。" }
  ],
  "content_en": [
    { "type": "image", "src": "images/vtuber-moca/final.png", "alt": "Final illustration" },
    { "type": "heading", "text": "About this work" },
    { "type": "text", "text": "English description." },
    { "type": "divider" },
    { "type": "heading", "text": "Process" },
    { "type": "image-row", "images": [
      { "src": "images/vtuber-moca/rough.png", "alt": "Rough sketch" },
      { "src": "images/vtuber-moca/line.png",  "alt": "Line art" }
    ]},
    { "type": "text", "text": "Notes on the process." }
  ]
}
