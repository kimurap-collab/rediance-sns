# Event Create / Edit — `#screen-event-create`

## Preview

```
app.html#screen-event-create
```

## Purpose

イベント作成＆編集画面（同一UIで両用途）。カバー写真は必須、ページ背景はグラデ＋模様の組み合わせで装飾、招待範囲（オープン/クローズド）を選択。

## Source Ranges

- **HTML**: `app.html:6856-6947`
- **CSS**: `.ev-create-header`, `.ev-create-body`, `.ev-cover-editor`, `.ev-cover-preview`, `.ev-cover-drag-hint`, `.ev-cover-section-title`, `.ev-cover-palette`, `.ev-cover-upload-btn`, `.ev-bg-editor-block`, `.ev-bg-editor-title`, `.ev-bg-editor-hint`, `.ev-bg-mini-preview`, `.ev-bg-mini-name`, `.ev-field`, `.ev-field-label`, `.ev-field-optional`, `.ev-field-row`, `.ev-input`, `.ev-input-lg`, `.ev-textarea`, `.ev-scope-toggle`, `.ev-scope-btn`, `.ev-scope-detail`, `.ev-invite-targets`, `.ev-invite-target`, `.ev-invite-summary`, `.ev-send-btn`
- **JS関数（主要）**:
  - `openEventCreate()` — 新規作成（約12170行〜）
  - `openEventEdit(eventId)` — 既存編集（約12205行〜）
  - `closeEventCreate()` — キャンセル
  - `submitEventCreate()` — 送信
  - `renderEvCoverEditor()` — カバー写真パレット描画
  - `renderEvBgEditor()` — グラデ＋模様パレット描画
  - `handleEvCoverUpload(event)` — 自分の写真アップロード
  - `attachCoverDragHandlers(el)` — カバー写真ドラッグ位置調整
  - `setEventScope('open' | 'closed')` — 公開範囲切替
  - `renderEventInviteTargets()` — 招待対象リスト描画
  - `applyEventPageBg(el, gradIdx, patternKey)` — 背景適用
- **データ定数**:
  - `EV_COVER_PHOTOS`（プリセット6種、`app.html:12044`）
  - `EV_BG_GRADIENTS`（8種、`app.html:11952`）
  - `EV_BG_PATTERNS`（11種、`app.html:11967`）

## Screen Structure

```
1. ev-create-header（固定）
   ├─ 戻る ← → closeEventCreate()
   ├─ タイトル: "イベント作成" or "イベント編集"（evCreateScreenTitle）
   └─ 右スペーサー（36px、センター寄せ用）

2. ev-create-body（スクロール）

   【カバー写真エディタ】ev-cover-editor
   ├─ ev-cover-preview（プレビュー、ドラッグで位置調整可）
   ├─ ev-cover-drag-hint "ドラッグで位置調整"（写真選択時のみ表示）
   ├─ "写真を選ぶ" 見出し
   ├─ ev-cover-palette（プリセット6枚、横スクロール）
   └─ "📷 自分の写真をアップロード" ボタン

   【ページ背景エディタ】ev-cover-editor ev-bg-editor-block
   ├─ "✨ ページ背景" 見出し
   ├─ "下のプレビューに即反映されます" ヒント
   ├─ ev-bg-mini-preview（中央に英語名ラベル表示）
   │   └─ ev-bg-mini-name "Solid Black" 等
   ├─ "グラデーション" 見出し
   ├─ ev-bg-grad-palette（グラデ8種、横スクロール）
   ├─ "模様" 見出し
   └─ ev-bg-pattern-palette（模様11種、横スクロール）

   【フィールド群】
   ├─ タイトル（text）
   ├─ 開始日時（date + time、横並び）
   ├─ 終了日時 任意（date + time）
   ├─ 場所（text）
   ├─ 詳細 任意（textarea rows=4）
   ├─ 公開範囲（2ボタントグル）
   │   ├─ 🌐 オープン：誰でも参加可能
   │   └─ 🔒 クローズド：指定した仲間のみ
   │       └─ ev-scope-detail（クローズド選択時のみdisplay）
   │           ├─ "招待する相手を選択"
   │           ├─ ev-invite-targets（招待対象カード群）
   │           └─ ev-invite-summary "合計N名"
   └─ 回答締切 任意（date）

   └─ ev-send-btn "イベントを作成" / "保存する"（編集時）
```

## Data Requirements

```typescript
interface Event {
  id: string;              // 'ev-xxx'
  title: string;           // 必須
  date: string;            // YYYY-MM-DD、必須
  time: string;            // HH:mm、必須
  endDate?: string;
  endTime?: string;
  place: string;           // 必須
  message?: string;
  deadline?: string;       // 回答締切
  scope: 'open' | 'closed';
  invitedIds?: string[];   // scope='closed'時のみ
  coverImage: string;      // URL、必須
  coverPos: { x: number; y: number };  // 0〜100（%）
  bgGradIdx: number;       // EV_BG_GRADIENTSのインデックス
  bgPattern: string;       // EV_BG_PATTERNSのキー
  hostId: string;          // 作成者ID
  rsvps: { [userId: string]: 'going' | 'maybe' | 'skip' };
}
```

## State変数（グローバル）

```js
let evEditingEventId = null;           // 編集中のID、新規はnull
let evCreateCoverImage = null;         // 選択中のカバー写真URL
let evCreateCoverPos = { x: 50, y: 50 };
let evCreateBgGradIdx = 0;
let evCreateBgPattern = 'black';       // デフォルト
let evCreateScope = 'open';
let evCreateInvitedIds = [];
```

## Interactions

| 要素 | アクション |
|---|---|
| プリセット写真タップ | カバー写真確定→プレビュー更新 |
| アップロードボタン | ファイル選択→プレビュー更新（base64） |
| プレビュー上ドラッグ | `evCreateCoverPos` 更新（mouse+touch対応） |
| グラデーションドット | `evCreateBgGradIdx` 更新→プレビュー即反映 |
| 模様ドット | `evCreateBgPattern` 更新→プレビュー即反映、英語名表示 |
| オープン/クローズド | `setEventScope()` で切替、クローズド時のみ招待UI展開 |
| 招待対象カード | トグル選択、`ev-invite-summary` 更新 |
| イベント作成ボタン | `submitEventCreate()` → バリデーション→作成→詳細画面へ |

## Validation（submitEventCreate）

```js
if (!evCreateCoverImage) { showToast('カバー写真を選択してください'); return; }
if (!title) { showToast('タイトルを入力してください'); return; }
if (!date) { showToast('開始日を入力してください'); return; }
// ...
```

## Scroll Behavior

- 画面を開く毎に `document.getElementById('screen-event-create').scrollTop = 0` で最上部にリセット（`openEventCreate` / `openEventEdit` の両方で）

## Theme

- **ライトテーマ**（白背景・黒文字）
- 入力しやすさ優先。ダーク配色は使わない
- ev-invite-target は白カード＋1.5px var(--border)

## Navigation (from this screen)

- 戻る → `closeEventCreate()` → 直前の画面
- 作成/保存 → `submitEventCreate()` → `openEventDetail(newId)`

## Edge Cases

- **black（Solid Black）パターン選択時**: `apply: null` なので `applyEventPageBg` が早期リターンで単色 `#000` を塗る
- **EV_BG_GRADIENTS[0] はダークナイト（グラデ）**: 他のグラデと同じくgradientなので、パターンがその上にレイヤーされる
