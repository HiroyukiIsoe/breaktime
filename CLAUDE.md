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
- レスポンシブデザイン
- テキストボックスとコピーボタンの横並び配置
- ホバー効果とアニメーション
- 上寄せレイアウト

## 技術仕様

### 使用技術
- **HTML5**: セマンティックなマークアップ
- **CSS3**: Flexbox、トランジション、ホバー効果
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

## 対応ブラウザ
- モダンブラウザ（Chrome, Firefox, Safari, Edge）
- Clipboard API 対応ブラウザ

## デプロイメント
GitHub Pages での静的サイトホスティングに対応。`index.html` をリポジトリのルートまたは `docs/` フォルダに配置することでアクセス可能です。