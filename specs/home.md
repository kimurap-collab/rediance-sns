# Home Feed — `#screen-home`

## Preview

```
app.html#screen-home
```

## Purpose

ホームフィード画面。タイムライン（X風の縦スクロール）とストーリーズ横スクロールを表示する。アプリ起動直後のデフォルト画面。

## Source Ranges

- **HTML**: `app.html:5846-5859`
- **CSS**: `app.html` 内のhome関連セレクタ（`.home-header`, `.home-stories-row`, 投稿カード系）
- **JS関数**:
  - `buildHomeFeed()` — フィード描画（app.html:10993）
  - `buildHomeStories()` — ストーリーズバー描画（app.html:10662）
  - `switchTab('home')` — 画面切替（app.html:8579〜）

## Screen Structure

```
1. home-header（56px）
   ├─ 左: ＋ボタン → switchTab('post')
   ├─ 中央: HYPERAYロゴ（hyperay-logo-white.png, 28px）
   └─ 右: ♡ボタン → showNotif()
2. home-stories-row（横スクロール）
   └─ ストーリーアイコン（丸型、ユーザー別、未読リング付き）
3. homeFeed（縦スクロール、padding-bottom: 100px）
   └─ 投稿カード（feedPosts配列から生成）
```

## Data Requirements

```typescript
interface FeedPost {
  id: string;                 // 'f1', 'f2', ...
  userId: string;
  name: string;
  avatar: string;             // 絵文字
  avatarClass: string;        // 'avatar-radiance' | 'avatar-inner' | ...（tier色）
  score: number;              // 影響力スコア（この値からtierが動的導出される）
  time: string;               // '1時間前', '8分前' 形式（相対日本語）
  body: string;               // 改行保持
  grad: string;               // CSS gradient背景（img未指定時のカード背景）
  img?: string;               // Unsplash URL（投稿に写真がある場合）
  likes: number;
  shares: number;
  isSun: boolean;             // トップ階層の"太陽"表示フラグ
}
```

データは `feedPosts` グローバル配列（app.html:10831〜）。tier（Star/Nova等）は保存されず、`getTitle(score)`（app.html:7813）が score から動的に導出する。

## Interactions

| 要素 | アクション |
|---|---|
| タップ | ＋ボタン → 投稿コンポーザー起動 |
| タップ | ♡ボタン → 通知画面 |
| タップ | ストーリー → ストーリービューア（未実装） |
| タップ | 投稿カード本体 → 投稿詳細（openPostDetail） |
| タップ | 投稿内ユーザー名 → openUserProfile |
| タップ | ❤️いいねボタン → トグル＋カウンター更新 |
| タップ | 🔄シェア → showToast |

## Navigation (from this screen)

- `switchTab('post')` — 投稿作成
- `showNotif()` / `switchTab('notif')` — 通知
- `openUserProfile(userId)` — 他者プロフィール
- `openPostDetail(postId)` — 投稿詳細

## Key CSS

- `.home-header` — position:sticky, top:0, z-index, var(--bg)背景、var(--border)下線
- 投稿カード — white背景、1px var(--border)、8px角丸、padding 16px、縦20pxギャップ

## Notes

- overflow-y:auto はscreen要素自体に付与（スクロールはこの階層）
- タブバーは画面下部固定（別要素、app.html:7488〜）
- ストーリーリングのアニメーションは `.stories-seg-fill.active` で実装
