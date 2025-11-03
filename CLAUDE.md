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

### 3. UI/UXデザイン
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

### コピー機能
```javascript
async function copyToClipboard() {
    try {
        await navigator.clipboard.writeText(currentMessage);
        showFeedback('コピーしました');
    } catch (err) {
        console.error('コピーに失敗しました:', err);
        showFeedback('コピーに失敗しました');
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

## 対応ブラウザ・デバイス
- **デスクトップ**: Chrome, Firefox, Safari, Edge（最新版）
- **モバイル**: iOS Safari, Chrome for Android
- **要件**: Clipboard API 対応ブラウザ

## デプロイメント
GitHub Pages での静的サイトホスティングに対応。`index.html` をリポジトリのルートまたは `docs/` フォルダに配置することでアクセス可能です。