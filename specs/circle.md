# Circle Settings — `#screen-circle`

## Preview

```
app.html#screen-circle
```

## Purpose

サークル（人間関係のレイヤー）管理画面。5階層（Core / Orbit / Halo / Galaxy / Connect）に人を振り分け、各階層には上限人数がある。Rediance SNSの思想を体現する中核UI。

## Source Ranges

- **HTML**: `app.html:6981-7142`
- **CSS**: `.circle-screen-header`, `.circle-section`, `.circle-section-header`, `.circle-section-title`, `.circle-capacity`, `.circle-chevron`, `.circle-members-scroll`, `.circle-member`, `.circle-member-avatar`, `.avatar-inner`, `.avatar-trusted`, `.avatar-close`, `.avatar-connected`, `.add-slot`, `.add-slot-wrap`, `.add-slot-label`, `.ambient-count`
- **JS**:
  - `toggleSection(id)` — セクション開閉
  - `showMemberAction(name, tier)` — メンバータップ時のアクションシート
  - `openAddMemberSheet(tierId)` — メンバー追加シート
  - `openCircleSettings()` — タブ外（例: MyPageから）からこの画面を直接開くラッパー

## Screen Structure

```
1. header
   └─ "Circle Settings"
2. circle-screen-header "Your Circle"
3. 5階層のセクション（上から順）

   【Core】sec-inner（open）
   ├─ 🔥 アイコン
   ├─ capacity "2/10人"
   └─ members-scroll（横スクロール）
       ├─ 田中 悠（avatar-inner）
       ├─ 鈴木 里奈（avatar-inner）
       └─ ＋追加スロット

   【Orbit】sec-trusted（open）
   ├─ 💎 / "5/15人"
   └─ 佐藤健太, 伊藤大輔, 高橋美咲, 小林亮, 渡辺彩 + ＋

   【Halo】sec-close（閉じ）
   ├─ ⭐ / "12/50人"
   └─ 山田彩香, 中島誠, 岡田真由, 森拓也 + ＋

   【Galaxy】sec-connected（閉じ）
   ├─ 🌌 / "45/150人"
   └─ 中村翔, 松本葵, 藤田悠斗 + "他42人..."

   【Connect】sec-ambient（閉じ、mb:100px）
   ├─ 🤝 / "280/500人"
   └─ ambient-count: "Connect済み（上位階層未配置）: 280人 / Connect上限 500人"
```

## Tier Definition

| Tier | ID | Icon | 上限 | 関係性 |
|---|---|---|---|---|
| Core | sec-inner | 🔥 | 10 | 最も近い仲間 |
| Orbit | sec-trusted | 💎 | 15 | 信頼する同志 |
| Halo | sec-close | ⭐ | 50 | 親しい友人 |
| Galaxy | sec-connected | 🌌 | 150 | つながり（ダンバー数） |
| Connect | sec-ambient | 🤝 | 500 | 知り合い（未配置） |

**思想**: 人間の脳が親密な関係を維持できる人数（ダンバー数150）に基づく階層制限。

## Avatar Color by Tier

- `avatar-inner` — Coreの暖色系
- `avatar-trusted` — Orbitの紫/青系
- `avatar-close` — Haloの黄/オレンジ系
- `avatar-connected` — Galaxyの青系

## Interactions

| 要素 | アクション |
|---|---|
| セクションヘッダー | toggleSection — 開閉 |
| メンバーカード | showMemberAction — 詳細／降格／削除シート |
| ＋追加スロット | openAddMemberSheet — メンバー候補シート |
| Connectセクション | ambient-countのみ表示（リストなし） |

## Theme

ライトテーマ。セクションは白背景＋薄ボーダー。

## Navigation

- この画面への入り口：
  - myProfile / profile の "Circle Settings" リンク
  - ☰メニュー内の "Circle Settings"
- 他画面へは下部タブバーで移動

## Notes

- Connect（最下層）は「上位4階層に配置されていない繋がり」という位置づけ
- Connect上限500人 = Rediance SNSの1ユーザーの関係性上限
