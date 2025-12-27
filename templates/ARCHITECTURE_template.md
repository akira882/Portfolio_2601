# Architecture - ClearTask Web版

**アーキテクチャ設計書**

---

## 🏗️ システムアーキテクチャ概要

```
┌─────────────────────────────────────────────────────┐
│                   Client (Browser)                   │
│  ┌──────────────────────────────────────────────┐  │
│  │         HTML + CSS + JavaScript               │  │
│  │              TypeScript (34%)                 │  │
│  └──────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────┘
                        ↓ ↑
                   HTTPS / WSS
                        ↓ ↑
┌─────────────────────────────────────────────────────┐
│                Firebase Services                     │
│  ┌──────────────────┐  ┌──────────────────────┐    │
│  │  Authentication  │  │   Cloud Firestore     │    │
│  │  - Google Login  │  │  - Tasks Collection   │    │
│  │  - Email/Pass    │  │  - Users Collection   │    │
│  └──────────────────┘  └──────────────────────┘    │
│                                                      │
│  ┌──────────────────────────────────────────────┐  │
│  │            Firebase Hosting                   │  │
│  │         (CDN + HTTPS Automatic)               │  │
│  └──────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────┘
```

---

## 📁 プロジェクト構造

```
PwD-Task-Management-App/
├── src/
│   ├── components/           # UIコンポーネント
│   │   ├── TaskCard.js       # タスク表示カード
│   │   ├── TaskForm.js       # タスク追加フォーム
│   │   └── ProgressBar.js    # 進捗バー
│   │
│   ├── services/             # ビジネスロジック・外部通信
│   │   ├── firebase.js       # Firebase初期化
│   │   ├── auth.js           # 認証ロジック
│   │   └── taskService.js    # タスクCRUD操作
│   │
│   ├── utils/                # ユーティリティ関数
│   │   ├── priorityCalc.js   # 優先度計算
│   │   ├── sanitize.js       # XSS対策サニタイズ
│   │   └── dateFormatter.js  # 日付フォーマット
│   │
│   └── types/                # TypeScript型定義
│       └── task.ts           # Task型定義
│
├── public/                   # 静的ファイル
│   ├── index.html
│   ├── styles.css
│   └── assets/
│
├── firebase.json             # Firebase設定
├── firestore.rules           # Firestoreセキュリティルール
├── .env.example              # 環境変数テンプレート
├── package.json
└── tsconfig.json
```

---

## 🔄 データフロー

### **タスク追加フロー**

```
┌──────────┐
│  User    │
│  Input   │
└────┬─────┘
     │ 1. タイトル、重要度、緊急度を入力
     ↓
┌──────────────────┐
│  sanitize.js     │ 2. XSS対策サニタイズ
└────┬─────────────┘
     │
     ↓
┌──────────────────┐
│ priorityCalc.js  │ 3. 優先度自動計算
│ priority =       │    (importance × 3 + urgency)
│ importance×3 +   │
│ urgency          │
└────┬─────────────┘
     │
     ↓
┌──────────────────┐
│ taskService.js   │ 4. Firestoreへ保存
│ addDoc()         │
└────┬─────────────┘
     │
     ↓
┌──────────────────┐
│ Cloud Firestore  │ 5. データ保存
│ tasks collection │
└────┬─────────────┘
     │
     ↓ onSnapshot
┌──────────────────┐
│  UI Update       │ 6. リアルタイム反映
│  (TaskCard)      │
└──────────────────┘
```

---

## 🗄️ データベース設計

### **Firestore Collections**

#### **tasks コレクション**

```typescript
{
  id: string;              // 自動生成ID
  userId: string;          // 作成ユーザーID
  title: string;           // タスクタイトル
  importance: number;      // 重要度 (1-5)
  urgency: number;         // 緊急度 (1-5)
  priority: number;        // 自動計算優先度 (1-20)
  completed: boolean;      // 完了フラグ
  dueDate: Timestamp;      // 期限日
  createdAt: Timestamp;    // 作成日時
  updatedAt: Timestamp;    // 更新日時
}
```

**インデックス**:
- `userId` + `priority` (降順) - 優先度ソート用
- `userId` + `completed` - 完了/未完了フィルタ用

---

#### **users コレクション**

```typescript
{
  id: string;              // ユーザーID (Firebase Auth UID)
  displayName: string;     // 表示名
  email: string;           // メールアドレス
  photoURL: string;        // プロフィール画像URL
  createdAt: Timestamp;    // 登録日時
}
```

---

## 🔐 セキュリティ設計

### **Firestoreセキュリティルール**

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // 認証済みユーザーのみ自分のタスクにアクセス可能
    match /tasks/{taskId} {
      allow read: if request.auth != null
                  && resource.data.userId == request.auth.uid;

      allow create: if request.auth != null
                    && request.resource.data.userId == request.auth.uid;

      allow update, delete: if request.auth != null
                            && resource.data.userId == request.auth.uid;
    }

    // ユーザープロファイル
    match /users/{userId} {
      allow read, write: if request.auth != null
                         && request.auth.uid == userId;
    }
  }
}
```

### **XSS対策**

```javascript
// src/utils/sanitize.js
export function sanitizeInput(input) {
  return input
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;')
    .replace(/"/g, '&quot;')
    .replace(/'/g, '&#x27;')
    .replace(/\//g, '&#x2F;');
}
```

---

## ⚡ パフォーマンス最適化

### **実装済み**

1. **Firestoreクエリ最適化**
   - 複合インデックス活用
   - リアルタイムリスナーの適切なデタッチ

2. **画像最適化**
   - WebP形式使用
   - Lazy Loading実装

3. **CDN活用**
   - Firebase Hostingによる自動CDN配信

### **今後の改善予定**

- [ ] Service Worker導入（PWA化）
- [ ] キャッシュ戦略最適化
- [ ] コード分割（Code Splitting）

---

## 🧪 テスト戦略（今後実装予定）

### **単体テスト**
- **対象**: `priorityCalc.js`, `sanitize.js`
- **ツール**: Jest

### **統合テスト**
- **対象**: Firebase連携部分
- **ツール**: Firebase Emulator

### **E2Eテスト**
- **対象**: ユーザーフロー全体
- **ツール**: Cypress

---

## 📊 スケーラビリティ

### **現状の制限**

| 項目 | 現在 | 上限 |
|------|------|------|
| ユーザー数 | ~100 | Firebaseの無料枠内 |
| タスク数/ユーザー | ~1000 | Firestoreクエリ制限 |
| 同時接続数 | ~100 | Firebase無料枠 |

### **スケール戦略**

1. **短期（1000ユーザーまで）**
   - Firebase無料枠で対応可能
   - インデックス最適化

2. **中期（10000ユーザーまで）**
   - Firebase Blazeプランへ移行
   - Cloud Functionsでバッチ処理

3. **長期（100000ユーザー以上）**
   - マイクロサービス化検討
   - データベース分散

---

## 🔄 デプロイフロー

```bash
# 1. ローカル開発
npm run dev

# 2. ビルド
npm run build

# 3. Firebase Emulatorでテスト
firebase emulators:start

# 4. 本番デプロイ
firebase deploy --only hosting

# 5. Firestoreルールデプロイ
firebase deploy --only firestore:rules
```

---

## 📚 技術選定理由

### **なぜFirebaseを選んだか**

| 要件 | Firebase | 代替案 | 選定理由 |
|------|----------|--------|----------|
| 認証 | Firebase Auth | Auth0, Cognito | 統合が容易、無料枠が充実 |
| DB | Firestore | MongoDB, PostgreSQL | リアルタイム同期、スケーラビリティ |
| Hosting | Firebase Hosting | Vercel, Netlify | CDN自動、HTTPS無料 |

### **なぜTypeScriptを一部採用したか**

- **型安全性**: タスクオブジェクトの型定義で実行時エラー防止
- **IDE補完**: 開発効率向上
- **段階的導入**: 既存JavaScriptコードと共存可能

---

## 🎯 設計原則

1. **シンプルさ最優先** - 複雑な機能は排除
2. **認知負荷の最小化** - UIの視覚的ノイズ削減
3. **型安全性** - TypeScriptによる型定義
4. **セキュリティ** - Firestoreルール、XSS対策
5. **アクセシビリティ** - WCAG 2.1 AA準拠

---

**作成日**: 2025-12-27
**最終更新**: 2025-12-27
