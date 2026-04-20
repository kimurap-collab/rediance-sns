# Search — `#screen-search`

## Preview

```
app.html#screen-search
```

## Purpose

検索画面。ユーザー＋投稿を横断検索。入力なしの状態は「Featured（おすすめユーザー）」＋「Trending（#タグ）」を表示する発見型UI。

## Source Ranges

- **HTML**: `app.html:6956-6978`
- **CSS**: `.search-trend-item`, `.search-post-card`, `.search-post-top`, `.search-post-avatar`, `.search-post-name`
- **JS**:
  - `handleSearch()` — 入力検知→フィルタリング（app.html:8666）
  - `getAllSearchablePosts()` — feedPosts＋各ユーザーのpostsを結合（app.html:8616）
  - `buildSearchPostCard(p)` — 投稿カード描画（app.html:8634）
  - `openSearchPostFs(encodedJson)` — 投稿フルスクリーン展開（app.html:11131）
  - `initSearchRecommended()` — おすすめユーザーグリッド（app.html:8657）

## Screen Structure

```
1. home-header
   └─ 検索input（🔍アイコン付き、oninput=handleSearch）
2. 検索結果エリア（searchResults、デフォルト非表示）
   └─ 入力時のみ表示：ユーザー一覧＋投稿一覧
3. デフォルト表示（searchDefault）
   ├─ "Featured" 見出し
   ├─ searchRecommended（おすすめユーザーグリッド）
   ├─ "Trending" 見出し
   └─ search-trend-item（#タグ5つ、タップで検索実行）
      ├─ #AIエージェント
      ├─ #スタートアップ
      ├─ #シンガポール
      ├─ #投資
      └─ #デザイン
```

## Search Behavior

1. inputのvalueが空 → `#searchResults` 非表示、`#searchDefault` 表示
2. inputが1文字以上 → 両エリア反転
3. 検索対象：
   - `feedPosts`（home feed）
   - `userProfiles[x].posts`（重複除去）
4. ユーザー名・投稿本文・handleに部分一致

## Data

```typescript
interface SearchablePost {
  userId: string;
  name: string;
  avatar: string;
  body: string;
  grad?: string;
  img?: string;
  time: string;
  likes: number;
  shares: number;
  score: number;
}
```

## Interactions

| 要素 | アクション |
|---|---|
| searchInput | handleSearch() — リアルタイム検索 |
| トレンドタグ | 検索欄にセット＋handleSearch |
| 検索結果カード | openSearchPostFs |
| おすすめユーザー | openUserProfile |

## Theme

ライトテーマ。入力欄は `var(--surface)` 背景、`var(--border)` ボーダー。

## Navigation

他画面へはタブバーで移動。検索画面内からはユーザー詳細・投稿フルスクリーンへ遷移。
