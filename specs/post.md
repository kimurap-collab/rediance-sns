# Post Composer — `#screen-post`

## Preview

```
app.html#screen-post
```

## Purpose

投稿コンポーザー。Instagram Stories風のフルスクリーンキャンバスで、背景（グラデ/写真）＋テキストレイヤー＋URLカードを組み合わせて投稿を作成する。

## Source Ranges

- **HTML**: `app.html:5863-6081`
- **CSS**: `.cmp-*` プレフィックスの全セレクタ（app.html:4259〜5000行周辺）
- **JS**:
  - `cmpPost()` — 投稿確定
  - `cmpClose()` — キャンセル
  - `cmpCreateTextLayer()` — テキストレイヤー追加
  - `cmpSelectGradFromPalette()` — グラデ選択
  - `cmpLoadPhoto()` — 写真ロード
  - `cmpCycleFilter()` — フィルター切替
  - `cmpToggleToolPanel('font')` — フォント/カラーパネル開閉
  - `cmpOpenUrlModal()` — URL添付モーダル

## Screen Structure

```
1. cmp-canvas（フルスクリーン）
   ├─ cmp-bg — 背景（グラデor写真）
   ├─ cmp-photo-wrap — 写真表示＋変形ハンドル（4角＋回転）
   ├─ cmp-photo-filter — フィルターオーバーレイ
   ├─ cmp-text-layers — テキストレイヤー（複数可）
   ├─ cmp-center-guides — ドラッグ中のセンターガイド
   ├─ cmp-trash-zone — ゴミ箱（ドラッグ中のみ表示）
   └─ cmp-float-controls（右下）
       ├─ cmp-float-palette — グラデ10色ドット（横並び、展開可）
       ├─ cmp-float-grad-btn — 現在グラデ表示
       ├─ cmp-float-btn Aa — フォント/カラーパネル
       ├─ cmp-float-btn 写真 — ファイル選択
       ├─ cmp-float-btn フィルター — 次フィルター
       ├─ cmp-float-btn URL — URLモーダル
       └─ cmp-float-post-btn — 投稿ボタン
2. cmp-header（上部）
   └─ cmp-close-btn ✕
```

## Data / State

```typescript
// グローバル状態（app.html:9252〜）
let cmpBgMode: 'grad' | 'photo' | 'gif';
let cmpSelectedGradIdx: number;      // 0〜9
let cmpLayers: TextLayer[];          // テキストレイヤー配列
let cmpSelectedLayerId: string | null;
let cmpPhotoSelected: boolean;
let cmpCurrentFilterIdx: number;     // 現在フィルターのインデックス（cmpFilters配列の位置）

interface TextLayer {
  id: string;                        // 'tl-1', 'tl-2', ...
  text: string;
  fontFamily: string;
  fontWeight: string | number;
  color: string;
  bgType: 'none' | 'solid' | 'outline';
  x: number;                         // キャンバス中心からのオフセット
  y: number;
  scale: number;
  rotate: number;                    // ラジアンではなくdegree
  fontSize: number;                  // デフォルト32
}
```

## Interactions

| 要素 | アクション |
|---|---|
| グラデドット | タップでグラデ切替 |
| グラデボタン長押し | 10色パレット展開 |
| 写真ボタン | ファイル選択 → 写真モードに切替 |
| フィルターボタン | 次フィルターにサイクル |
| テキストレイヤー | ドラッグで移動、ピンチで拡縮回転、ダブルタップで編集 |
| 写真 | ドラッグで移動、4角で拡縮、上部ハンドルで回転 |
| ゴミ箱 | 写真/レイヤーをドラッグしてドロップで削除 |
| ✕ボタン | `cmpClose()` → switchTab('home') |
| 投稿ボタン | `cmpPost()` → feedPosts追加 → switchTab('home') |

## Constraints

- タブバーは非表示（`.phone:has(#screen-post.active) .tab-bar { display: none; }`、182行目）
- キャンバス空白タップ → レイヤー選択解除（`cmpExitEditMode` + `cmpDeselectAll`）
- 初回投稿画面表示時、レイヤーが空なら自動でテキストレイヤー作成（8604行目）

## Navigation (from this screen)

- `cmpClose()` → ホーム
- `cmpPost()` → 投稿追加＋ホーム
