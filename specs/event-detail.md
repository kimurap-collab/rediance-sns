# Event Detail — `#screen-event-detail`

## Preview

```
app.html#screen-event-detail
```

(eventsData[0]で開く)

## Purpose

イベント詳細画面。カバー写真、タイトル、日時、場所、ホスト、参加者リスト、Q&A的な詳細、RSVP（行く/仮/スキップ）を1ページに収める。ダークテーマ・カスタム背景（グラデ＋模様）適用。

## Source Ranges

- **HTML骨格**: `app.html:6950-6953`（動的生成なので実体は少ない）
  ```html
  <div class="screen" id="screen-event-detail">
    <div class="ev-detail-body" id="evDetailBody"></div>
    <div class="ev-detail-cta-bar" id="evDetailCtaBar"></div>
  </div>
  ```
- **CSS**: `.ev-detail-*`, `.ev-rsvp-*`, `.ev-answer-*`, `.ev-cta-pill`, `.ev-detail-scope-badge`
- **JS**:
  - `openEventDetail(eventId, returnTo)` — 開く（app.html:12690）
  - `closeEventDetail()` — 閉じて遷移元へ戻る（app.html:12700）
  - `renderEventDetail(ev)` — 画面内容をinnerHTMLで生成（app.html:12723）
  - `setEventResponse(status)` — RSVP更新（app.html:12858 onclick内）
  - `setInviteResponseFromFeed(eventId, status)` — ホームフィードから即時RSVP（app.html:12714）
  - `applyEventPageBg(screen, gradIdx, patternKey)` — 背景塗り（app.html:12025）

## Rendered Structure（動的HTML）

```
1. ev-detail-body（スクロール）
   ├─ ev-detail-cover（カバー写真、背景画像＋position）
   │   └─ scope-badge（🌐オープン / 🔒クローズド）
   ├─ ev-detail-content
   │   ├─ ev-detail-title
   │   ├─ ev-detail-date "3月15日(土) 19:00"
   │   ├─ ev-detail-place "📍 〇〇"
   │   ├─ ev-detail-host（主催者アバター＋名前）
   │   ├─ ev-detail-message（詳細文、改行保持）
   │   ├─ 参加者グリッド "行く: N名"
   │   │   └─ ev-detail-participant（12ユーザー表示、超過時 "+N"）
   │   └─ 主催者のみ：編集ボタン → openEventEdit(id)
2. ev-detail-cta-bar（画面下部固定、RSVPボタン）
   ├─ 参加状況サマリ
   └─ ev-rsvp-primary / ev-rsvp-secondary（行く/仮/スキップ）
```

## RSVP State

```typescript
interface EventResponses {
  [userId: string]: 'going' | 'maybe' | 'skip';
}

// ev.responses に格納。buildHomeFeed との連携あり（ホーム画面から直接RSVP可）
```

## Interactions

| 要素 | アクション |
|---|---|
| カバー写真 | なし（装飾のみ） |
| 参加者アバター | openUserProfile |
| 編集ボタン（ホストのみ） | openEventEdit(id) |
| 🟢行く/🟡仮/🔴スキップ | setEventResponse → トースト＋UI更新 |
| 戻る | closeEventDetail → returnTo(talk/home) |

## CTA Bar（ev-detail-cta-bar）

状態別の色分け：
- **going (active)**: rgba(34,197,94,0.3)背景、#22C55Eボーダー、#86EFACテキスト
- **maybe (active)**: rgba(234,179,8,0.3)背景、#EAB308ボーダー、#FDE68Aテキスト
- **skip (active)**: rgba(239,68,68,0.3)背景、#EF4444ボーダー、#FCA5A5テキスト
- **inactive**: rgba(255,255,255,0.1)背景、white透過テキスト

(app.html:3056〜3195参照)

## Theme

- **ダークテーマ**（作成画面のグラデ＋模様が適用される）
- `#screen-event-detail.active { display: flex; }` で縦フレックスレイアウト（app.html:2867）

## Navigation

- `openEventDetail(id, returnTo)` の第2引数で戻り先を指定：
  - `'talk'`: closeEventDetail→ switchTalkTab('events')
  - `'home'`: closeEventDetail → screen-home → buildHomeFeed()
- 編集 → `openEventEdit(id)` → screen-event-create

## Home Feed連携

ホームフィードにもイベント招待カードが表示され、そこから直接RSVP可能（`setInviteResponseFromFeed`）。詳細画面を開かずにワンタップで回答できる導線。
