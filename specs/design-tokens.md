# Design Tokens

Rediance SNSのデザイントークン一覧。CSS変数として `app.html` 冒頭の `:root {}`（11〜41行目）で定義されている。新画面を実装する際は必ずこのトークンを使用し、生の16進カラーコードは原則禁止。

## カラー（ブランド）

| Token | 値 | 用途 |
|---|---|---|
| `--primary` | `#1E3A5F` | メインカラー（ネイビー）。CTA、タイトル、アイコン強調 |
| `--primary-light` | `#2E5EAA` | ホバー、選択状態 |
| `--primary-dark` | `#0F2040` | プレス状態、深色背景 |
| `--primary-bg` | `#EEF3FB` | 薄い背景（セクションエリア） |
| `--primary-bg-light` | `#F5F8FE` | さらに薄い背景 |

## カラー（称号）

| Token | 値 | 用途 |
|---|---|---|
| `--gold` | `#F59E0B` | ゴールド称号、重要アクセント |
| `--gold-light` | `#FCD34D` | ゴールドグラデ |
| `--amber` | `#D97706` | 琥珀系アクセント |
| `--silver` | `#94A3B8` | シルバー称号 |

## カラー（中立・テキスト）

| Token | 値 | 用途 |
|---|---|---|
| `--bg` | `#FFFFFF` | ページ背景 |
| `--surface` | `#F5F5F5` | カード・パネル背景 |
| `--border` | `#E8E8E8` | 区切り線、カード境界 |
| `--text-main` | `#000000` | 本文 |
| `--text-sub` | `#555555` | 補助情報 |
| `--text-light` | `#999999` | メタ情報（時刻等） |
| `--inner-bg` | `#F5F5F5` | 入れ子パネル背景 |
| `--trusted-bg` | `#FAFAFA` | 信頼系UIの背景 |

## カラー（意味）

| Token | 値 | 用途 |
|---|---|---|
| `--success` | `#10B981` | 成功、行くRSVP |
| `--danger` | `#EF4444` | 危険、スキップRSVP |

## RSVP専用カラー（イベント画面）

各状態（行く/仮/スキップ）ごとに active/inactive トーンが異なる。`app.html` 3056〜3195行目参照。

- **going**: `#22C55E`（ボーダー）, `#86EFAC`（テキスト）, `rgba(34,197,94,0.25〜0.3)`（背景）
- **maybe**: `#EAB308`（ボーダー）, `#FDE68A`（テキスト）, `rgba(234,179,8,0.25〜0.3)`（背景）
- **skip**:  `#EF4444`（ボーダー）, `#FCA5A5`（テキスト）, `rgba(239,68,68,0.25〜0.3)`（背景）

## 角丸

| Token | 値 | 用途 |
|---|---|---|
| `--radius-sm` | `6px` | 小さなバッジ、タグ |
| `--radius-md` | `8px` | カード、入力欄 |
| `--radius-lg` | `50px` | 丸ピル（ボタン、RSVP） |
| `--radius-xl` | `50px` | 丸ピル（同） |

## シャドウ

| Token | 値 | 用途 |
|---|---|---|
| `--shadow-sm` | `0 1px 3px rgba(0,0,0,0.06)` | フラット要素の僅かな浮き |
| `--shadow-md` | `0 4px 12px rgba(0,0,0,0.08)` | 通常のカード |
| `--shadow-lg` | `0 8px 24px rgba(0,0,0,0.10)` | モーダル、フローティング |

## レイアウト定数

| Token | 値 | 用途 |
|---|---|---|
| `--tab-height` | `68px` | 下部タブバー |
| `--header-height` | `56px` | 上部ヘッダー |
| `--status-height` | `44px` | iOSステータスバー模倣 |

## グローバル

- **フォント**: `-apple-system, BlinkMacSystemFont, 'SF Pro Display', system-ui, 'Hiragino Sans', 'Hiragino Kaku Gothic ProN', 'Noto Sans JP', sans-serif`
- **レタースペーシング**: `-0.14px`
- **フォント機能**: `font-feature-settings: "kern" 1`
- **ボディ背景**: `#F0F0F0`（iPhone枠の外側）
- **フォーカスリング**: `outline: 2px dashed #1E3A5F; outline-offset: 2px`
- **Phone最大幅**: `430px`
- **Phoneシャドウ**: `0 0 40px rgba(0,0,0,0.15)`

## ステータスバーのグラデ（固定）

`linear-gradient(135deg, #020408 0%, #1E3A5F 35%, #7C3AED 70%, #EC4899 100%)`

## 余白スケール（推奨）

明示的なトークンはないが、コードベース全体で慣習化されている：

- `4px` / `6px` / `8px` / `12px` / `16px` / `20px` / `24px` / `32px`
- padding, margin, gap はこのスケールから選ぶ

## アニメーション

- **トースト表示時間**: 2500ms（`app.html` 8575行目）
- **タブ切替のiconスケール**: `scale(1.1)`（active時）
- **タブ切替のiconプレス**: `scale(0.95)` + 即時復帰

## 重要な制約

1. **ダークテーマとライトテーマの混在あり**：
   - メイン画面（home, my, profile等）は **ライトテーマ**（白背景、黒文字）
   - イベント詳細画面（event-detail）は **ダークテーマ**（背景グラデ/柄）
   - イベント作成画面（event-create）は **ライトテーマ**（入力しやすさ優先）
2. **RSVPピル**は常にダーク調で統一（イベント画面内で使用）
3. 新画面を追加する際は既存画面のテーマに合わせる
