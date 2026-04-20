# Rediance SNS — Specs for Programmers (AI-friendly)

このディレクトリは **プログラマー（人間・AI両方）が Rediance SNS の画面を忠実に再現するための技術仕様書**。各specはシンプルなMarkdownで、LLMが機械的に読めるよう構造化されている。

---

## 使い方（Quickstart）

### 1. プレビューで画面を確認する

```
app.html#<screen-id>
```

例：`app.html#screen-event-create` をブラウザで開くと、イベント作成画面が直接表示される。これで「完成後どう見えるか」がすぐ分かる。

### 2. 実装に必要な情報を集める

1. `design-tokens.md` を読んで色・影・角丸・余白を把握
2. `assets.md` を読んで画像URL・絵文字・パターンの一覧を把握
3. 対象画面の spec（例: `event-create.md`）を読んで構造・インタラクション・データ要件を把握
4. spec内の **Source Ranges** で `app.html` の該当行を参照

### 3. 一次ソースは常に app.html

各specは「入り口」であって完全な実装仕様ではない。詳細は必ず **`app.html` の行番号範囲を読んで確認** する。

---

## ファイル構成

```
specs/
├── README.md              ← このファイル
├── design-tokens.md       ← CSS変数（色・影・角丸・余白）
├── assets.md              ← 画像URL・絵文字・パターン一覧
│
├── home.md                ← #screen-home         ホームフィード
├── post.md                ← #screen-post         投稿コンポーザー
├── notif.md               ← #screen-notif        通知
├── profile.md             ← #screen-profile      他者プロフィール
├── my.md                  ← #screen-my           自分プロフィール
├── talk.md                ← #screen-talk         メッセージ/Circle/イベント
├── event-create.md        ← #screen-event-create イベント作成/編集
├── event-detail.md        ← #screen-event-detail イベント詳細
├── search.md              ← #screen-search       検索
└── circle.md              ← #screen-circle       サークル設定
```

---

## Spec の共通フォーマット

各画面specは以下のセクションを持つ：

| セクション | 内容 |
|---|---|
| **Preview** | `app.html#<id>` — ブラウザで直接開けるURL |
| **Purpose** | この画面が何のためにあるか |
| **Source Ranges** | app.html内のHTML/CSS/JS行番号範囲 |
| **Screen Structure** | 画面の階層構造（ツリー図） |
| **Data Requirements** | TypeScript interface形式のデータ定義 |
| **Interactions** | 要素×アクションの表 |
| **Theme** | ライト/ダークどちらか |
| **Navigation** | この画面から他画面への遷移先 |
| **Notes** | その他の注意点 |

---

## 全画面一覧（Screen ID → Tab Map）

| Hash URL | 画面名 | タブ | 用途 |
|---|---|---|---|
| `#screen-home` | ホーム | home | フィード＋ストーリーズ |
| `#screen-post` | 投稿作成 | post | Instagram Stories風コンポーザー |
| `#screen-notif` | 通知 | notif | 通知一覧（ハードコード） |
| `#screen-profile` | 他者プロフィール | profile | Pokémon Cardスタイルの他者情報 |
| `#screen-my` | 自分プロフィール | my | 自分情報＋Identity/Posts切替 |
| `#screen-talk` | メッセージ | talk | DM＋Circle＋イベント統合 |
| `#screen-event-create` | イベント作成/編集 | — | 独立画面、タブからは直接来ない |
| `#screen-event-detail` | イベント詳細 | — | 独立画面、ダークテーマ |
| `#screen-search` | 検索 | search | ユーザー＋投稿＋トレンド |
| `#screen-circle` | サークル設定 | circle | 人間関係5階層管理 |

---

## 技術スタック（固定）

- **単一HTMLファイル**: すべて `app.html` 1ファイルに詰め込む
- **Vanilla JS**: フレームワーク不使用（React/Vue等なし）
- **CSS**: 生のCSS、プリプロセッサなし、`:root {}` 変数のみ
- **外部依存**: 絵文字（Unicode）＋Unsplash画像のみ。JS/CSSライブラリは一切読まない
- **状態管理**: グローバル変数（`feedPosts`, `eventsData`, `userProfiles`, `cmp*`, `ev*`等）
- **ルーティング**: ハッシュベース（`#screen-xxx` → JavaScript で `routeByHash()`）
- **ターゲット**: モバイル（幅430px前提の `.phone` コンテナ）

---

## 実装上の原則

### 1. プロ素材を先に探す（自作禁止）

デザイン要素（SVGパターン、アイコン、イラスト）は **必ず既存のプロ素材を借用** する。自作SVG・自作パスは原則禁止。詳細は `~/.claude/skills/web-design/SKILL.md` を参照。

現状の実績：
- **イベント背景パターン**: Lea Verou's CSS3 Patterns Gallery（MITライセンス）から移植
- **画像**: すべてUnsplash

### 2. クリック回数最小化

ユーザー目線で「このアクションは何クリックで達成できるか」を常に意識。多すぎる場合は「本当に必要な階層か？」を再検討する。

### 3. 配色の統一

- メイン画面は **ライトテーマ**（白背景・黒文字）
- イベント詳細・投稿コンポーザーは **ダークテーマ**（ドラマティック表現）
- 両者を混在させない
- 必ず `design-tokens.md` のCSS変数を使い、生の16進は避ける

### 4. ダミーデータはハードコード

データベース接続は現時点ではなし。サンプルデータは `app.html` 内のJSグローバルに格納。

---

## 既知の未実装 / TODO

- DM送信（UI表示のみ）
- 通知の動的生成
- Edit Profile（トーストのみ）
- Archive / Activity / QR Code（メニュー項目のみ）
- 投稿のコメント機能
- ストーリービューア

---

## 更新方針

機能追加・バグ修正で画面構造が変わった場合は、**対応するspecを更新する**（特に Source Ranges の行番号とScreen Structureのツリー）。

spec と実装がズレたらspecを優先で直し、実装をspec通りに戻すか、specを現実に合わせる判断をする。
