# Notifications — `#screen-notif`

## Preview

```
app.html#screen-notif
```

## Purpose

通知画面。6件の固定サンプル通知を時系列（新→旧）で表示。現時点ではハードコード、動的生成ロジックなし。

## Source Ranges

- **HTML**: `app.html:6082-6133`
- **CSS**: `.notif-item`, `.notif-avatar`, `.notif-body`, `.notif-text`, `.notif-time`, `.notif-dot`
- **JS**: `showNotif()` → `switchTab('notif')`（app.html:8611）

## Screen Structure

```
1. header
   └─ header-logo: "🔔 通知"
2. 通知リスト（6件、縦並び）
   通知1件あたり：
   ├─ notif-avatar（絵文字）
   ├─ notif-body
   │  ├─ notif-text（HTML、<strong>でユーザー名強調）
   │  └─ notif-time（相対時刻表示）
   └─ notif-dot（未読マーカー、既読のものは非表示）
```

## Sample Data（固定）

| Avatar | Text | Time | 未読 |
|---|---|---|---|
| 🔥 | 田中 悠さんがあなたの投稿に⚡反応しました | 2分前 | ● |
| 💎 | 佐藤 健太さんがあなたをOrbitに追加しました | 18分前 | ● |
| ✨ | Sarah Chenの投稿があなたに届きました 💎 DiamondStar | 45分前 | ● |
| ⭐ | 山田 彩香さんがストーリーズに返信しました | 1時間前 |   |
| 🤝 | 中村 翔さんがあなたをGalaxyに追加しました | 3時間前 |   |
| 📈 | あなたの影響力スコアが+127増加しました ↑4.7% | 今日 6:00 |   |

## Interactions

現状は静的表示のみ。タップアクションなし。

## Navigation

- 他画面からはタブバーで到達できない。`showNotif()`はホームヘッダー♡ボタン経由でのみ呼ばれる
- 戻るボタンは明示的になく、下部タブで他画面へ移動

## Future Work (not implemented)

- 通知タップ → 該当投稿/プロフィールへ遷移
- 未読マーカーの自動クリア
- 通知の動的生成（feedPosts/ユーザーアクションから）
