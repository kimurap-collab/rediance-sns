# User Profile (other users) — `#screen-profile`

## Preview

```
app.html#screen-profile
```

デフォルトでは自分のデータで表示されるが、`openUserProfile(userId)` 呼び出し時に動的に差し替わる。

## Purpose

他者（またはホームからタップされたユーザー）のプロフィール画面。称号バッジ付きカード＋Identityセクション（投稿のカテゴリ別表示）＋bioカードで構成。

## Source Ranges

- **HTML**: `app.html:6136-6280`
- **CSS**: `.pokemon-profile-card`, `.pokemon-card-*`, `.identity-category`, `.identity-*`, `.profile-bio-*`, `.circle-settings-link`
- **JS**:
  - `openUserProfile(userId)` — プロフィール差し替え＋オーバーレイで表示
  - `toggleIdentitySection(id)` — Identityカテゴリの開閉
  - `showStatInfo(key)` — Trust/Prestige/Insights/Statureの説明モーダル
  - `openFsPost(posts, idx, 'my')` — 投稿フルスクリーン表示

## Screen Structure

```
1. pokemon-profile-card（450px高、背景写真）
   ├─ pokemon-card-topbar
   │  └─ tier-badge（例: "⭐ Star (50.9K)"）
   └─ pokemon-card-overlay（下部テキスト）
      ├─ name / handle / bio
      └─ stats（4つ、横並び）
         ├─ 💖 Trust
         ├─ 💎 Prestige
         ├─ 📝 Insights
         └─ 🏛️ Stature
2. circle-settings-link → openCircleSettings()
3. identity-section
   └─ 4カテゴリ（例: 🤖AI・テック / 🇸🇬シンガポール / 💼経営 / 📈投資）
      ├─ identity-cat-header（開閉トリガー、▾）
      └─ identity-cat-body
         └─ identity-posts-grid（グラデ背景の投稿カード）
4. profile-bio-card
   ├─ 📍 ロケーション
   ├─ タグ群
   └─ 📅 利用開始日
```

## Data Requirements

```typescript
interface UserProfile {
  id: string;
  name: string;
  handle: string;
  bio: string;
  profilePhoto: string;         // Unsplash URL
  tier: 'radiance' | 'diamond' | 'star' | 'rising' | 'new';
  score: number;
  stats: {
    trust: number;
    prestige: number;
    insights: number;
    stature: number;
  };
  identities: {
    [category: string]: {
      icon: string;
      name: string;
      updated: string;
      posts: Post[];
    };
  };
  location: string;
  tags: string[];
  joinedAt: string;
}
```

データ元: `userProfiles` グローバル（app.html:8070〜）

## Interactions

| 要素 | アクション |
|---|---|
| tier-badge | 装飾のみ（タップ無反応） |
| stat-item | showStatInfo() でモーダル表示 |
| identity-cat-header | toggleIdentitySection() で開閉 |
| identity-post-item | openFsPost() でフルスクリーン表示 |
| circle-settings-link | openCircleSettings() モーダル |

## Navigation

- 戻る: タブバーから他画面へ
- `openUserProfile` は内部的にオーバーレイ（`userProfileOverlay`）を使う可能性あり。タブ切替時に強制的に閉じる（8592〜）

## Notes

- 初期状態では4カテゴリのうち AI・シンガポール が `open` クラス付与で展開済み、経営/投資は閉じている
- 背景写真は `?w=800&h=450&fit=crop&crop=faces&facepad=3` で顔を中心にクロップ
