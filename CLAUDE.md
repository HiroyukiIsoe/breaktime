# 休憩時間表示ページ

## プロジェクト概要

GitHub Pages にアクセスした際に、現在時刻を基準とした 1 時間の休憩時間を表示する静的なウェブページです。ページには「休憩入ります。（HH:MI〜HH:MI）」という形式のテキストが表示され、クリップボードにコピーする機能も実装されています。

## 実装済み機能

### 1. 休憩時間の表示
- ページアクセス時に現在時刻を取得
- 開始時刻: 現在時刻（HH:MI形式の24時間表記）
- 終了時刻: 開始時刻の1時間後
- ゼロパディング対応（例: 09:05）
- 日付跨ぎ対応（例: 23:30 → 00:30）

### 2. コピー機能
- テキストボックスに休憩時間を表示
- 「コピー」ボタンでクリップボードにコピー
- コピー成功時に「コピーしました」のフィードバック表示
- エラーハンドリング実装

### 3. Google Analytics 4 (GA4) アクセス解析
- **ページビュートラッキング**
  - gtag.jsによる自動ページビュー計測
  - 測定ID: `G-C01JZ7W0PX`
- **カスタムイベントトラッキング**
  - `copy_break_time`: コピーボタンクリック時に送信（成功時）
  - `copy_error`: コピー失敗時に送信
  - イベントカテゴリとラベルによる分類
- **データ収集項目**
  - ページビュー数、ユニークユーザー数、セッション数
  - デバイス・ブラウザ情報
  - コピーボタンのエンゲージメント率

### 4. UI/UXデザイン
- **レスポンシブデザイン**
  - デスクトップ（769px以上）: 縦並び配置、画面幅の33%（最小400px、最大800px）
  - タブレット・スマートフォン（768px以下）: 縦並び配置、全幅表示、フォントサイズ調整
  - 小型スマートフォン（480px以下）: より小さなフォントサイズと余白の最適化
- ホバー効果とアニメーション
- 上寄せレイアウト
- タップ領域の最適化（モバイルフレンドリー）
- テキストボックスとボタンの幅を統一

## 技術仕様

### 使用技術
- **HTML5**: セマンティックなマークアップ、viewport設定
- **CSS3**:
  - Flexbox による柔軟なレイアウト
  - メディアクエリ（`@media`）によるレスポンシブ対応（768px, 480pxブレークポイント）
  - box-sizing: border-box による一貫した幅計算
  - トランジション、ホバー効果
- **JavaScript (ES6+)**:
  - Date API による時刻処理
  - Clipboard API によるコピー機能
  - DOM操作とイベント処理
  - async/await によるエラーハンドリング
- **Google Analytics 4 (gtag.js)**:
  - ページビューの自動トラッキング
  - カスタムイベントトラッキング
  - ユーザー行動分析

### ファイル構成
```
breaktime/
├── index.html          # 単一ファイル実装（HTML + CSS + JavaScript）
├── docs/
│   └── design.md      # 要件定義書
└── CLAUDE.md          # プロジェクト説明書（このファイル）
```

## 主要な実装詳細

### 時刻フォーマット関数
```javascript
function formatTime(date) {
    const hours = String(date.getHours()).padStart(2, '0');
    const minutes = String(date.getMinutes()).padStart(2, '0');
    return `${hours}:${minutes}`;
}
```

### コピー機能とGA4イベントトラッキング
```javascript
async function copyToClipboard() {
    try {
        await navigator.clipboard.writeText(currentMessage);
        showFeedback('コピーしました');

        // Google Analytics イベント送信
        if (typeof gtag !== 'undefined') {
            gtag('event', 'copy_break_time', {
                'event_category': 'engagement',
                'event_label': 'copy_button_click'
            });
        }
    } catch (err) {
        console.error('コピーに失敗しました:', err);
        showFeedback('コピーに失敗しました');

        // Google Analytics エラーイベント送信
        if (typeof gtag !== 'undefined') {
            gtag('event', 'copy_error', {
                'event_category': 'error',
                'event_label': 'copy_failed'
            });
        }
    }
}
```

### レイアウト構造
- Flexboxによる中央寄せとコンポーネント配置
- `readonly` input要素による選択可能なテキスト表示
- CSS transition による滑らかなユーザーインタラクション

### レスポンシブ対応
```css
/* デスクトップ: 縦並び配置 */
.container {
    display: flex;
    flex-direction: column;
    width: 33%;
    min-width: 400px;
    max-width: 800px;
}

.break-input, .copy-button {
    width: 100%; /* コンテナ内で全幅 */
}

/* タブレット・スマートフォン（768px以下）*/
@media (max-width: 768px) {
    .container {
        width: 100%; /* 画面幅いっぱい */
        max-width: 100%;
    }
    .break-input {
        font-size: 1.2rem; /* フォントサイズ調整 */
    }
    .copy-button {
        padding: 1rem;
        font-size: 1.1rem;
    }
}

/* 小型スマートフォン（480px以下）*/
@media (max-width: 480px) {
    .break-input {
        font-size: 1rem; /* さらに小さく */
    }
    .copy-button {
        font-size: 1rem;
    }
}
```

## Google Analytics 4 セットアップ

### 測定ID設定
現在の測定ID: `G-C01JZ7W0PX`

### トラッキングコード
[index.html](index.html) の `<head>` セクションに以下のコードを実装済み:
```html
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-C01JZ7W0PX"></script>
<script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config', 'G-C01JZ7W0PX');
</script>
```

### トラッキングイベント一覧
| イベント名 | トリガー | カテゴリ | ラベル |
|-----------|---------|---------|--------|
| `copy_break_time` | コピー成功 | engagement | copy_button_click |
| `copy_error` | コピー失敗 | error | copy_failed |
| `page_view` | ページ表示 | (自動) | (自動) |

### Google Analytics管理画面での確認方法
1. https://analytics.google.com/ にアクセス
2. プロパティ「Break Time Page」を選択
3. **リアルタイム**レポートで即座にアクセスを確認可能
4. **イベント**レポートで`copy_break_time`と`copy_error`を確認
5. **エンゲージメント > ページとスクリーン**でページビューを確認

## 対応ブラウザ・デバイス
- **デスクトップ**: Chrome, Firefox, Safari, Edge（最新版）
- **モバイル**: iOS Safari, Chrome for Android
- **要件**: Clipboard API 対応ブラウザ、JavaScript有効

## デプロイメント
GitHub Pages での静的サイトホスティングに対応。`index.html` をリポジトリのルートまたは `docs/` フォルダに配置することでアクセス可能です。

**デプロイURL**: https://hiroyukiisoe.github.io/breaktime/