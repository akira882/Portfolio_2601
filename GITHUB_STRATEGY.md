# GitHub Portfolio Management Strategy

**採用担当者が評価するGitHubポートフォリオ構築戦略**

学習過程を含む全てのコードをGitHubで管理し、「学習能力」と「成長過程」を可視化して採用確率を最大化します。

---

## 📊 GitHubポートフォリオの重要性

ポートフォリオのあるジュニア開発者の書類選考通過率は**67.2%**、ポートフォリオなしは**8.3%**

React Native開発者に必須のスキル：
- Git/GitHubの基本理解
- コードの保守性
- 継続的な学習姿勢

---

## 🗂️ 最適なGitHubリポジトリ構造

### **3つのリポジトリ戦略**

```
【リポジトリ1】react-native-learning-journey
└─ 日々の学習記録（Week 1〜12）

【リポジトリ2】portfolio-cleartask
└─ メインポートフォリオアプリ

【リポジトリ3】portfolio-smarthire
└─ サブポートフォリオアプリ
```

---

## 📁 詳細構造

### **リポジトリ1: react-native-learning-journey**

**目的：学習過程の可視化 → 「継続的学習能力」のアピール**

```
react-native-learning-journey/
├── README.md                          # 学習ロードマップ全体像
├── PROGRESS.md                        # 週次進捗レポート
├── week-01-javascript-basics/
│   ├── README.md                      # Week 1の学習目標と成果
│   ├── day-01-variables/
│   │   ├── README.md                  # 今日学んだこと
│   │   ├── code-examples.ts           # 実装コード
│   │   └── notes.md                   # Notion保存用メモ
│   ├── day-02-functions/
│   │   ├── README.md
│   │   ├── arrow-functions.ts
│   │   └── notes.md
│   ├── day-03-arrays/
│   ├── day-04-objects/
│   └── day-05-mini-project/
│       ├── README.md
│       ├── todo-logic.ts
│       └── package.json
│
├── week-02-typescript-basics/
│   ├── README.md
│   ├── day-01-type-annotations/
│   ├── day-02-interfaces/
│   └── ...
│
├── week-03-react-basics/
│   └── ...
│
├── week-04-react-intermediate/
│   └── ...
│
├── week-05-react-native-setup/
│   └── ...
│
└── resources/
    ├── references.md                  # 参考資料リンク集
    └── troubleshooting.md             # よくあるエラーと解決方法
```

---

### **リポジトリ2: portfolio-cleartask**

**目的：実務レベルのアプリ開発力証明**

```
portfolio-cleartask/
├── README.md                          # プロジェクト概要（重要！）
├── DEVELOPMENT.md                     # 開発過程の記録
├── TECHNICAL_DECISIONS.md             # 技術選定理由
├── .gitignore
├── package.json
├── tsconfig.json
├── app.json
│
├── src/
│   ├── components/
│   │   ├── common/
│   │   │   ├── Button.tsx
│   │   │   ├── Input.tsx
│   │   │   └── ProgressBar.tsx
│   │   └── features/
│   │       ├── TaskCard.tsx
│   │       └── PraiseMessage.tsx
│   │
│   ├── screens/
│   │   ├── HomeScreen.tsx
│   │   ├── TaskDetailScreen.tsx
│   │   └── SettingsScreen.tsx
│   │
│   ├── navigation/
│   │   └── AppNavigator.tsx
│   │
│   ├── contexts/
│   │   └── TaskContext.tsx
│   │
│   ├── services/
│   │   ├── firebase.ts
│   │   └── taskService.ts
│   │
│   ├── hooks/
│   │   ├── useTasks.ts
│   │   └── useSpeechRecognition.ts
│   │
│   ├── types/
│   │   └── index.ts
│   │
│   └── utils/
│       └── formatDate.ts
│
├── assets/
│   ├── images/
│   └── fonts/
│
├── __tests__/
│   ├── components/
│   └── services/
│
└── docs/
    ├── SETUP.md                       # セットアップ手順
    ├── ARCHITECTURE.md                # アーキテクチャ説明
    └── SCREENSHOTS.md                 # スクリーンショット
```

---

## 📝 最重要：README.md の書き方

### **採用担当者が最初に見る README.md**

```markdown
# ClearTask - 認知負荷を軽減するタスク管理アプリ

[![React Native](https://img.shields.io/badge/React%20Native-0.74-blue.svg)](https://reactnative.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0-blue.svg)](https://www.typescriptlang.org/)
[![Firebase](https://img.shields.io/badge/Firebase-10.0-orange.svg)](https://firebase.google.com/)

> 高次脳機能障がい当事者の視点で設計された、認知負荷を最小化するタスク管理アプリ

![App Demo](./docs/demo.gif)

## 🎯 プロジェクト概要

### 解決する課題
- 一般的なタスク管理アプリは、複数のタスクを同時に表示するため認知負荷が高い
- ADHD・高次脳機能障がい当事者にとって、マルチタスク環境は使いづらい
- 既存アプリにはアクセシビリティ配慮が不足

### このアプリの特徴
1. **1タスク1画面表示** - 認知負荷を最小化
2. **視覚的進捗バー** - 達成感の可視化
3. **音声入力対応** - Expo Speech Recognition活用
4. **リアルタイム同期** - Firebaseでマルチデバイス対応

---

## 🛠️ 技術スタック

### フロントエンド
- **React Native (Expo)** - クロスプラットフォーム開発
- **TypeScript** - 型安全性の確保
- **React Navigation** - 画面遷移管理
- **Context API** - グローバル状態管理

### バックエンド
- **Firebase Firestore** - NoSQLデータベース
- **Firebase Auth** - 認証機能

### 開発ツール
- **ESLint + Prettier** - コード品質管理
- **Jest + React Native Testing Library** - テスト

---

## 📱 主要機能

### 1. タスク管理
- タスクの追加・編集・削除（CRUD操作）
- 1タスク1画面表示で集中力維持
- 完了タスクの自動アーカイブ

### 2. 音声入力
```typescript
// Expo Speech Recognitionの実装例
const startRecording = async () => {
  const { status } = await Audio.requestPermissionsAsync();
  if (status === 'granted') {
    // 音声認識開始
  }
};
```

### 3. 進捗可視化
- 全体進捗バー（完了率表示）
- タスク完了時の称賛メッセージ
- 統計情報の可視化

---

## 🚀 セットアップ

### 必要要件
- Node.js 18.x以上
- npm または yarn
- Expo CLI
- iOS Simulator / Android Emulator

### インストール手順

```bash
# リポジトリのクローン
git clone https://github.com/your-username/portfolio-cleartask.git
cd portfolio-cleartask

# 依存関係のインストール
npm install

# Expo開発サーバーの起動
npx expo start
```

### Firebase設定

1. Firebaseプロジェクトを作成
2. `src/config/firebase.ts` に設定を追加

```typescript
// src/config/firebase.ts
export const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
  // ...
};
```

---

## 🏗️ アーキテクチャ

### フォルダ構成の設計思想
```
src/
├── components/     # 再利用可能なUIコンポーネント
├── screens/        # 画面単位のコンポーネント
├── navigation/     # 画面遷移の管理
├── contexts/       # グローバル状態（Context API）
├── services/       # 外部API通信ロジック
├── hooks/          # カスタムフック
├── types/          # TypeScript型定義
└── utils/          # ユーティリティ関数
```

詳細は [ARCHITECTURE.md](./docs/ARCHITECTURE.md) を参照

---

## 🧪 テスト

```bash
# 単体テスト実行
npm test

# カバレッジ確認
npm run test:coverage
```

---

## 📸 スクリーンショット

| ホーム画面 | タスク詳細 | 進捗画面 |
|-----------|-----------|---------|
| ![Home](./docs/screenshots/home.png) | ![Detail](./docs/screenshots/detail.png) | ![Progress](./docs/screenshots/progress.png) |

---

## 🎓 学んだこと・工夫したポイント

### 技術的な学び
1. **TypeScriptの型システム**
   - Genericsを使った再利用可能な型定義
   - Utility Types（Omit, Pick）の活用

2. **React Nativeのパフォーマンス最適化**
   - FlatListの適切な使用
   - useMemo/useCallbackによる再レンダリング抑制

3. **Firebaseのリアルタイム同期**
   - onSnapshotリスナーの実装
   - オフライン対応

### UX設計の工夫
1. **認知負荷の最小化**
   - 1画面1タスクの徹底
   - 視覚的フィードバックの充実

2. **アクセシビリティ**
   - スクリーンリーダー対応
   - 音声入力機能

---

## 🚧 今後の改善予定

- [ ] プッシュ通知機能の追加
- [ ] タスクのカテゴリ分け機能
- [ ] ダークモード対応
- [ ] E2Eテストの実装（Detox）

---

## 👤 作成者

**小清水晶**
- GitHub: [@your-username](https://github.com/your-username)
- Email: your-email@example.com

### 開発背景
高次脳機能障がい当事者として、既存のタスク管理アプリの使いづらさを実感し、
当事者視点で設計したアプリです。このプロジェクトを通じて、
React Native開発の実務スキルとアクセシビリティ設計の重要性を学びました。

---

## 📄 ライセンス

MIT License
```

---

## 🔄 Git管理戦略（コミット規約）

### **Conventional Commits準拠**

```bash
# 新機能追加
git commit -m "feat: タスク追加機能を実装"

# バグ修正
git commit -m "fix: Firebase接続エラーを修正"

# ドキュメント更新
git commit -m "docs: README.mdにセットアップ手順を追加"

# リファクタリング
git commit -m "refactor: TaskServiceをクラスベースに変更"

# テスト追加
git commit -m "test: TaskCard コンポーネントのテストを追加"

# スタイル変更
git commit -m "style: ESLintエラーを修正"
```

### **コミットメッセージのフォーマット**

```
<type>(<scope>): <subject>

<body>

<footer>
```

**例：**
```bash
git commit -m "feat(week1-day1): 変数とデータ型の学習完了

- let, const, string, number, booleanの使い分けを理解
- アロー関数の基本構文を習得
- 実装したコード: code-examples.ts
- 学習時間: 2時間"
```

---

## 📅 毎日のGitワークフロー

### **学習日の終わりにやること**

```bash
# 1. 今日の学習内容をステージング
cd react-native-learning-journey
git add week-01-javascript-basics/day-01-variables/

# 2. コミット（今日学んだことを明記）
git commit -m "feat(week1-day1): 変数とデータ型の学習完了

- let, const, string, number, booleanの使い分けを理解
- アロー関数の基本構文を習得
- 実装したコード: code-examples.ts
- 学習時間: 2時間"

# 3. GitHubにプッシュ
git push origin main

# 4. 週次レビュー時にPROGRESS.mdを更新
git add PROGRESS.md
git commit -m "docs: Week 1の進捗を更新"
git push origin main
```

---

## 📊 PROGRESS.md の書き方

```markdown
# React Native学習進捗レポート

最終更新：2025年1月5日

---

## 📈 全体進捗

| Week | 期間 | トピック | 完了率 | 学習時間 |
|------|------|----------|--------|----------|
| Week 1 | 12/29-1/4 | JavaScript基礎 | 100% | 12時間 |
| Week 2 | 1/5-1/11 | TypeScript基礎 | 60% | 8時間 |
| Week 3 | 1/12-1/18 | React基礎 | 0% | - |

**累計学習時間：20時間 / 目標240時間**

---

## ✅ Week 1: JavaScript基礎（完了）

### 学習内容
- Day 1: 変数・データ型・アロー関数
- Day 2: 配列とmap/filter
- Day 3: オブジェクトとプロパティ
- Day 4: 非同期処理（Promise, async/await）
- Day 5: ミニプロジェクト（ToDoロジック実装）

### 成果物
- [ToDoロジック実装](./week-01-javascript-basics/day-05-mini-project/)
- [学習ノート](./week-01-javascript-basics/README.md)

### できるようになったこと
✅ アロー関数を使った関数定義
✅ 配列メソッド（map, filter）の活用
✅ async/awaitでの非同期処理

### 課題・改善点
- 分割代入の理解に時間がかかった → Week 2で復習
- エラーハンドリングのパターンをもっと学びたい

---

## 🔄 Week 2: TypeScript基礎（進行中）

### 学習内容
- ✅ Day 1: 型注釈・基本型
- ✅ Day 2: インターフェース
- ✅ Day 3: Union型・型エイリアス
- ⏳ Day 4: Generics（今日）
- 予定 Day 5: ミニプロジェクト

### 現在の課題
Genericsの概念理解に苦戦中。明日Claude Codeで復習予定。

---

## 📝 学習記録

### 2025年1月5日（Week 2 Day 3）
**学習時間：2時間**

**今日学んだこと：**
- Union型（`string | number`）の使い方
- 型エイリアス（`type`）とインターフェース（`interface`）の違い

**実装したコード：**
```typescript
type Status = 'active' | 'inactive' | 'pending';
type ID = string | number;
```

**次回の予定：**
Genericsを学ぶ

---

## 🎯 今後の目標

### 短期目標（1月中）
- [ ] React基礎の完全習得（Week 3-4）
- [ ] React Native環境構築（Week 5）
- [ ] 初めてのReact Nativeアプリ作成

### 中期目標（2月）
- [ ] ポートフォリオアプリ1つ完成
- [ ] Firebase連携の実装

### 長期目標（3月）
- [ ] ポートフォリオ3つ完成
- [ ] 技術ブログ3本執筆
- [ ] 内定獲得

---

## 📚 参考資料

### よく使うリソース
- [React Native公式ドキュメント](https://reactnative.dev/)
- [TypeScript公式ドキュメント](https://www.typescriptlang.org/)
- [Firebase公式ドキュメント](https://firebase.google.com/docs)

### 参考にしたGitHubリポジトリ
- [企業の実装例1](リンク)
- [企業の実装例2](リンク)
```

---

## 🎯 採用担当者へのアピールポイント

### **GitHubポートフォリオで示すこと**

1. **継続的学習能力**
   - 12週間の学習記録が全て残っている
   - 毎日コミットしている（GitHub Contribution Graph）

2. **技術的成長**
   - Week 1のコード vs Week 12のコードの品質差
   - 段階的なスキル向上が可視化されている

3. **実務レベルのコード品質**
   - TypeScript完全型付け
   - エラーハンドリング完備
   - テストコード作成

4. **ドキュメント力**
   - README.mdが充実
   - コメントが丁寧
   - アーキテクチャ説明がある

5. **問題解決能力**
   - トラブルシューティング記録
   - 技術選定理由の明確な説明

---

## 📈 GitHub活用の具体的指標

### **採用担当者が見る指標**

1. **Contribution Graph（草）**
   - 毎日コミットすることで、継続性を示す
   - 目標：12週間（84日）連続コミット

2. **Commit数**
   - 質の高いコミットを積み重ねる
   - 目標：200+ commits（学習期間中）

3. **README.mdの充実度**
   - プロジェクト概要
   - セットアップ手順
   - スクリーンショット
   - 技術選定理由

4. **コードの質**
   - TypeScript完全型付け
   - ESLint/Prettier準拠
   - テストコード

5. **ドキュメント**
   - ARCHITECTURE.md
   - TECHNICAL_DECISIONS.md
   - DEVELOPMENT.md

---

## 🚀 実装ロードマップ

### **Phase 1: 基礎固め（Week 1-4）**
- JavaScript/TypeScript完全習得
- React基礎・応用
- 毎日のコミット習慣確立

### **Phase 2: React Native開発（Week 5-8）**
- 環境構築
- ClearTaskプロジェクト開始
- Firebase統合

### **Phase 3: 高度な機能実装（Week 9-10）**
- SmartHireプロジェクト開始
- OpenAI API統合

### **Phase 4: 仕上げ（Week 11-12）**
- テスト実装
- ドキュメント充実
- デプロイ

---

## ✅ チェックリスト

### **各プロジェクトの完成基準**

- [ ] TypeScript完全型付け（strict mode有効）
- [ ] ESLint/Prettier準拠
- [ ] テストカバレッジ80%以上
- [ ] README.md充実
- [ ] ARCHITECTURE.md作成
- [ ] TECHNICAL_DECISIONS.md作成
- [ ] スクリーンショット・デモ動画
- [ ] Expo公開URL

---

**最終更新**: 2025-12-27
