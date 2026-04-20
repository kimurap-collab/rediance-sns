# Talk (Messages / Circle / Events) — `#screen-talk`

## Preview

```
app.html#screen-talk
```

## Purpose

メッセージ・連絡先・イベントの統合画面。3つのセグメントタブで切り替える。DMスレッド一覧、Circle/Groupメンバー管理、開催イベントリストを一箇所でまとめる。

## Source Ranges

- **HTML**: `app.html:6430-6855`
- **CSS**: `.talk-screen-header`, `.talk-seg-bar`, `.talk-seg-tab`, `.talk-tab-content`, `.tl-*`, `.contacts-*`
- **JS**:
  - `switchTalkTab('dm' | 'contacts' | 'events')` — タブ切替
  - `openDmChat(userId, name, avatar)` — DMチャット画面
  - `openNewMessage()` — 新規メッセージ作成
  - `openDirectThread(members)` — 直接送信スレッド作成
  - `openNewMsgPreset(members)` — 宛先プリセット済み新規
  - `toggleContactsTier(id)` — ティア開閉
  - `contactsFilterMembers(q)` — 名前検索
  - `openCircleSettings()` — Circle設定モーダル
  - `openEventCreate()` — 新規イベント作成
  - `openEventDetail(eventId)` — イベント詳細

## Screen Structure

```
1. talk-screen-header
   ├─ "Talk"
   └─ ✏️ → openNewMessage()
2. talk-seg-bar（3タブ）
   ├─ Messages（active）
   ├─ Circle & Groups
   └─ Events
3. タブコンテンツ（display切替）

  【tab: dm】talkContent-dm
  tl-list（縦並び）
    tl-row（6件、サンプル）
      ├─ tl-avatar（絵文字）
      ├─ tl-top: name(unread付き) + time
      └─ tl-bottom: preview + tl-badge（未読件数）

  【tab: contacts】talkContent-contacts
  contacts-wrap
    ├─ 検索バー（名前フィルタ）
    ├─ Your Circle セクション
    │   └─ Core / Orbit / Halo ティア（折畳式）
    │       └─ メンバーカード（タップでプロフィール）
    ├─ Groups セクション
    │   └─ グループカード（タップで詳細）
    └─ Contacts セクション

  【tab: events】talkContent-events
  イベントリスト（eventsDataから生成）
    └─ イベントカード → openEventDetail(id)
```

## Data

```typescript
interface DmThread {
  userId: string;
  name: string;
  avatar: string;  // 絵文字
  lastMessage: string;
  time: string;
  unreadCount: number;
}

interface ContactsTier {
  id: 'core' | 'orbit' | 'halo';
  icon: string;
  label: string;
  members: { id: string; name: string; avatar: string }[];
}

interface Event {
  id: string;
  title: string;
  date: string;
  time: string;
  place: string;
  hostId: string;
  coverImage?: string;
  coverPos?: { x: number; y: number };
  // ...（event-create.md参照）
}
```

## Interactions

| 要素 | アクション |
|---|---|
| tl-row | openDmChat — DM画面 |
| ✏️ | openNewMessage — 新規DM |
| セグメントタブ | switchTalkTab で表示切替 |
| ティア行タップ | 開閉トグル |
| Send to All ボタン | その階層全員に送信 |
| Edit & Send ボタン | 宛先プリセットで新規作成 |
| ＋（Circle見出し） | openCircleSettings |
| 検索入力 | contactsFilterMembers(q) — フィルタ結果表示 |
| イベントカード | openEventDetail(id) |

## Notes

- タブ間は `talk-tab-content.active { display: block; }` で切替（app.html:2334）
- サンプルDMは6件固定（田中悠・鈴木里奈が未読）
- 現状DM送信は未実装、UI表示のみ

## Navigation

- DMチャット → `openDmChat` — 新画面に遷移
- イベント詳細 → `openEventDetail`
- プロフィール → `openUserProfile`
