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

実形状（`userProfiles` グローバル、app.html:8070〜）：

```typescript
interface UserProfile {
  id: string;
  name: string;
  handle: string;                 // '@yu_tanaka' など
  avatarEmoji: string;            // アバター絵文字
  avatarClass: string;            // 'avatar-inner' | 'avatar-trusted' | 'avatar-close' | 'avatar-connected'
  bannerGradient: string;         // CSS linear-gradient文字列
  profilePhoto: string;           // Unsplash URL
  bio: string;                    // 改行保持
  location: string;
  joinDate: string;               // '2024年X月から利用'
  score: number;                  // 影響力スコア（R Score）
  stats: {
    trust: number;
    prestige: number;
    insights: number;
    stature: number;
  };
  innerCount?: number;            // Circleに入れている人数（Core）
  trustedCount?: number;          // Orbit
  closeCount?: number;            // Halo
  posts: Post[];                  // 本人の投稿
  identity: IdentityCategory[];   // Identityカテゴリ配列
}

interface Post {
  id: string;
  body: string;
  time: string;
  likes: number;
  shares: number;
  grad: string;                   // CSS gradient背景
}

interface IdentityCategory {
  name: string;                   // 絵文字＋名前、例 "🤖 AIエージェント"
  updated: string;                // '8分前'
  posts: { body: string; grad: string }[];
}
```

**称号（tier）はデータ上のフィールドではない**。`getTitle(score)` 関数（app.html:7813）がスコアから動的に導出する。10段階：Human / Leader / Icon / Rising / Star / Nova / Supernova / Gravity / Singularity / Universe。

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
