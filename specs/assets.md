# Assets

Rediance SNSで使用する全アセット一覧。外部素材のURL、絵文字ルール、アイコン方針。

## 画像（Unsplash）

すべてのプロフィール写真・イベント写真は **Unsplash** を使用。MITライセンス相当で商用利用可。

### プロフィール写真（12ユーザー分・`?w=800&h=450&fit=crop&crop=faces&facepad=3`）

| ユーザー | URL末尾 | 定義場所 |
|---|---|---|
| 自分（taisho_k） | `photo-1507003211169-0a1dd7228f2d` | app.html:6139, 6293, 8320 |
| rina_s | `photo-1539571696357-5a69c17a67c6` | app.html:8078 |
| yu_tanaka | `photo-1494790108377-be9c29b29330` | app.html:8109 |
| kenta_s | `photo-1580489944761-15a19d654956` | app.html:8135 |
| misaki_t | `photo-1500648767791-00dcc994a43e` | app.html:8153 |
| daisuke_ito | `photo-1472099645785-5658abf4ff4e` | app.html:8176 |
| kobayashi_r | `photo-1519085360753-af0119f7cbe7` | app.html:8194 |
| watanabe_a | `photo-1438761681033-6461ffad8d80` | app.html:8212 |
| ayaka_y | `photo-1463453091185-61582044d556` | app.html:8230 |
| sho_n | `photo-1534528741775-53994a69daeb` | app.html:8248 |
| sarah_ai | `photo-1500648767791-00dcc994a43e` | app.html:8266 |
| kenji_founder | `photo-1573496359142-b8d87734a5a2` | app.html:8284 |

**使用方法**: `https://images.unsplash.com/{ID}?w=800&h=450&fit=crop&crop=faces&facepad=3`

### イベントカバー写真（プリセット6種）

`app.html` 12044〜12051行 `EV_COVER_PHOTOS` 配列に定義。

```js
const EV_COVER_PHOTOS = [
  'https://images.unsplash.com/photo-1530103862676-de8c9debad1d?w=800&q=80', // party lights
  'https://images.unsplash.com/photo-1414235077428-338989a2e8c0?w=800&q=80', // dinner
  'https://images.unsplash.com/photo-1507525428034-b723cf961d3e?w=800&q=80', // beach
  'https://images.unsplash.com/photo-1492684223066-81342ee5ff30?w=800&q=80', // concert night
  'https://images.unsplash.com/photo-1528605248644-14dd04022da1?w=800&q=80', // mountains
  'https://images.unsplash.com/photo-1519671482749-fd09be7ccebf?w=800&q=80', // bbq
];
```

### 投稿サンプル画像

| 用途 | URL | 定義場所 |
|---|---|---|
| feed投稿1 | `photo-1540575467063-178a50c2df87?w=600&q=80` | app.html:10856 |
| feed投稿2 | `photo-1525625293386-3f8f99389edd?w=600&q=80` | app.html:10899 |
| feed投稿3 | `photo-1512820790803-83ca734da794?w=600&q=80` | app.html:10942 |

## アイコン

**アイコン戦略は二層**：

1. **ナビゲーションタブ**: インラインSVG（Heroicons風、stroke線画）
2. **UI内の機能表示**: Unicode絵文字（🔥💎⭐🌌🤝 等）

外部アイコンライブラリ（Font Awesome等）は未使用。

### ナビゲーションタブ（4タブ + 中央投稿ボタン）

実装は `app.html:7488-7506`。すべてインラインSVG（24x24、stroke-width 1.5、`currentColor`でテーマ追従）。

| タブ | アイコンSVG出典 | onclick | ID |
|---|---|---|---|
| ホーム | Heroicons home（家型） | `switchTab('home')` | `tab-home` |
| トーク | Heroicons chat-bubble | `switchTab('talk')` | `tab-talk` |
| 投稿（中央） | Heroicons plus（＋） | `switchTab('post')` | なし（`class="tab-post"`） |
| 検索 | Heroicons magnifying-glass | `switchTab('search')` | `tab-search` |
| プロフィール | Heroicons user | `switchTab('my')` | `tab-my` |

**注意**: 通知（notif）と サークル（circle）、プロフィール他者（profile）はタブバーには出ない。別画面として独立し、特定の呼び出し関数（showNotif(), openCircleSettings(), openUserProfile(id)）からのみ到達する。

### ホームヘッダー内のボタン

`app.html:5847-5852`：

| 位置 | 内容 | 実装 |
|---|---|---|
| 左 | ＋（投稿） | テキスト記号、`onclick="switchTab('post')"` |
| 中央 | HYPERAYロゴ | `<img src="hyperay-logo-white.png" height:28px>` |
| 右 | ♡（通知） | テキスト記号、`onclick="showNotif()"` |

### UI内の絵文字（代表例）

| 用途 | 絵文字 | 備考 |
|---|---|---|
| Core（サークル） | 🔥 | |
| Orbit（サークル） | 💎 | |
| Halo（サークル） | ⭐ | |
| Galaxy（サークル） | 🌌 | |
| Connect（サークル） | 🤝 | |
| 通知ヘッダー | 🔔 通知 | |
| 場所 | 📍 | |
| スコープオープン | 🌐 | イベント公開範囲 |
| スコープクローズド | 🔒 | イベント公開範囲 |
| RSVPタグ | ✅🤔❌ | 行く/仮/スキップ |
| アバター代替 | 🧑👩🧔👨🙍 | ユーザー写真がない時 |
| 設定メニュー | ☰ | My Pageヘッダー右 |

絶対的な一覧表ではなく、代表例。実装時は `app.html` の該当セクションを直接読むこと。

### アバター絵文字（ユーザー用）

`userProfiles[x].avatarEmoji` に格納。12ユーザー分の絵文字は `app.html` 8070〜8290行を参照。

## イベント背景グラデ（8種）

`EV_BG_GRADIENTS`（app.html:11952〜11961）。

| # | 名称 | 値 |
|---|---|---|
| 0 | ダークナイト | `linear-gradient(180deg, #0A0A0F 0%, #1F1B2E 60%, #2A2340 100%)` |
| 1 | ネオン紫 | `linear-gradient(180deg, #E879F9, #A855F7, #6D28D9, #4C1D95)` |
| 2 | サンセット | `linear-gradient(180deg, #FB923C, #EF4444, #BE123C, #7F1D1D)` |
| 3 | オーシャン | `linear-gradient(180deg, #22D3EE, #3B82F6, #1D4ED8, #1E3A8A)` |
| 4 | ミント | `linear-gradient(180deg, #34D399, #10B981, #047857, #064E3B)` |
| 5 | スポット紫ピンク | `radial-gradient(ellipse at 50% 25%, #F9A8D4, #E879F9, #9333EA, #4C1D95)` |
| 6 | スポットゴールド | `radial-gradient(ellipse at 50% 25%, #FDE68A, #FB923C, #EA580C, #7C2D12)` |
| 7 | ローズ | `linear-gradient(180deg, #F9A8D4, #EC4899, #BE185D, #831843)` |

## イベント背景パターン（11種）

`EV_BG_PATTERNS`（app.html:11967〜）。**Lea Verou's CSS3 Patterns Gallery**（MITライセンス）から移植した純CSS gradient。

| key | 日本語 | 英語表示名 |
|---|---|---|
| `black` | 黒 | Solid Black |
| `polkadot` | ドット | Polka Dot |
| `checker` | チェッカー | Checker |
| `stripe` | 斜めストライプ | Diagonal Stripes |
| `waves` | 波 | Waves |
| `hearts` | ハート | Hearts |
| `hstripe` | 横ストライプ | Horizontal Stripes |
| `argyle` | アーガイル | Argyle |
| `honeycomb` | 蜂の巣 | Honeycomb |
| `tartan` | タータン | Tartan |
| `seigaiha` | 青海波（和） | Seigaiha |

**重要**: `black` は `apply: null` で、選択時は単色 `#000` を塗る特殊ケース（applyEventPageBg関数 12025行目参照）。

## フォント

**デフォルト**はシステムフォント：

```
-apple-system, BlinkMacSystemFont, 'SF Pro Display', system-ui,
'Hiragino Sans', 'Hiragino Kaku Gothic ProN', 'Noto Sans JP', sans-serif
```

**Google Fonts**（10書体）を `app.html:9` で読み込み済み。主に **投稿コンポーザーのテキストレイヤーのフォント選択**（app.html:5941〜5946）に使用：

| data-font | 表示 | 用途 |
|---|---|---|
| `'Noto Sans JP', sans-serif` | 標準ゴシック | デフォルト |
| `'M PLUS 1p', sans-serif` weight:700 | 太ゴシック | 強調 |
| `'Noto Serif JP', serif` | 明朝 | 落ち着き |
| `'Shippori Mincho', serif` | エレガント | 格調高 |
| `'Yusei Magic', cursive` | 手書き | 親しみ |
| `'Klee One', cursive` | 学校風 | カジュアル |
| `'Zen Kaku Gothic New'` / `'Kaisei Decol'` / `'Stick'` / `'Reggae One'` | 予備 | 拡張用 |

UIチャート・本文等の他領域ではシステムフォントを使用する。

## ロゴ

**現行ブランドは HYPERAY で固定**。ホームヘッダーで使用。

| ファイル | 使用箇所 | 用途 |
|---|---|---|
| `hyperay-logo-white.png` | `app.html:5850`（ホームヘッダー中央、28px高） | 現行ロゴ（ダーク色ヘッダー上で白抜き表示） |

**注意**: `index.html`（Circle組閣ウィザード）のタイトル・バッジもHYPERAY表記で統一済み。新規ロゴが必要な場合は HYPERAY 名義で作成し、旧ブランド（Elsius／Radiance）名の資産は追加しないこと。

## 素材選定ルール

新しい画像が必要な場合：

1. **Unsplash** (`unsplash.com`) から選ぶ
2. URL末尾は必ず `?w=<幅>&q=80` または `?w=<幅>&h=<高>&fit=crop&crop=faces&facepad=3`（顔写真）
3. 生のURLをコードにベタ書き（APIは使わない）
4. ライセンス確認：Unsplashは商用利用可・クレジット不要

## 禁止事項

- 外部アイコンライブラリ（Font Awesome等）の追加は不可。タブはHeroicons風インラインSVG、UIは絵文字という二層戦略で統一
- 新規Google Fontsの追加は不可（既存10書体で賄う）。別CDNの外部フォントも不可
- `<script>` タグを含むSVGのコピペ不可（XSSリスク）
