# My Profile — `#screen-my`

## Preview

```
app.html#screen-my
```

## Purpose

自分自身のプロフィール画面。`screen-profile`とほぼ同じ構造だが、ヘッダーに☰メニュー、アクションボタン（Edit Profile / Circle Settings / Group Settings）、Identity/Postsタブ切替が追加される。

## Source Ranges

- **HTML**: `app.html:6283-6427`
- **CSS**: `.my-profile-tab-bar`, `.my-profile-tab`, `.post-grid`, `.pokemon-profile-card`（共通）
- **JS**:
  - `switchMyTab('identity' | 'posts')` — タブ切替
  - `openSettingsMenu()` — ☰メニュー（右サイドパネル）
  - `openCircleSettings()` — サークル設定モーダル
  - `openGroupCreate()` — グループ作成モーダル
  - `showStatInfo(key)` / `toggleIdentitySection(id)` / `openFsPost(posts, idx, 'my')` — profile.md参照

## Screen Structure

```
1. home-header
   ├─ 左: 空white space（32px）
   ├─ 中央: "@taisho_k"
   └─ 右: ☰ → openSettingsMenu()
2. pokemon-profile-card（profile.mdと同構造）
3. アクションボタン3つ（横並び、各flex:1）
   ├─ Edit Profile（coming soon）
   ├─ Circle Settings → openCircleSettings()
   └─ Group Settings → openGroupCreate()
4. my-profile-tab-bar
   ├─ Identity タブ（デフォルトactive）
   └─ Posts タブ
5. myTabContent-identity（初期表示）
   └─ identity-section（4カテゴリ：AI / シンガポール / 経営 / 投資）
6. myTabContent-posts（display:none → タブでtoggle）
   └─ post-grid（#myPostsGrid、JSで生成）
```

## Data

- 固定値の自分情報（木村 誠司、@taisho_k、Star tier、50.9K等）
- `myIdentityPosts` — Identityカテゴリ別投稿（app.html内で定義）
- `myPosts` — Postsタブのグリッド表示用

## Interactions

| 要素 | アクション |
|---|---|
| ☰ | 右サイドメニュー（`#settingsMenuOverlay`、app.html:7510〜）展開 |
| Edit Profile | 未実装（トースト） |
| Circle Settings | サークル設定モーダル |
| Group Settings | グループ作成モーダル |
| Identity/Posts タブ | switchMyTab() でコンテンツ切替 |
| カテゴリヘッダー | 開閉トグル |
| 投稿カード | openFsPost() フルスクリーン |

## Settings Menu（☰で開く）

`#settingsMenuOverlay`（app.html:7510〜7545）、右からスライドイン（80%幅、max 320px）。

内容：
- 🔗 Share Profile / ⚙️ Circle Settings / 👤 Account / 🔔 Notifications / 🔒 Privacy / 🛡️ Security
- 📦 Archive / 📊 Activity / 📱 QR Code
- 🔖 Saved / ⭐ Favorites
- ❓ Help / 🚪 Log Out（danger色）

## Difference from screen-profile

| 項目 | screen-my | screen-profile |
|---|---|---|
| ヘッダー | ☰あり | なし |
| アクションボタン | 3つ | なし |
| タブ切替 | あり | なし |
| circle-settings-link | なし | あり |
| profile-bio-card | なし | あり |

## Navigation

- 他画面へはタブバーで移動
- ☰メニュー内から各設定画面へ（多くは未実装のトースト）
