# 📱 Portfolio Projects Strategy

**採用責任者への「実務レベル証明」を最大化する3つのポートフォリオプロジェクト戦略**

---

## 🎯 ポートフォリオ戦略の目的

React Nativeエンジニアとして**即戦力性**を証明するため、以下3つの観点から設計されたアプリを開発します：

1. **技術的深さ** - 実務で求められる技術スタックの完全習得
2. **問題解決能力** - ユーザー課題の理解から実装まで
3. **コード品質** - 保守性・拡張性・テストの徹底

---

## 📊 プロジェクト構成

| プロジェクト | 状態 | 目的 | 主要技術 | アピールポイント |
|-------------|------|------|---------|----------------|
| **ClearTask (Web版)** | ✅ **実装済み** | 実務レベルのアプリ開発能力証明 | HTML/CSS/JavaScript, TypeScript, Firebase | 認知負荷を考慮したUX設計、Firebase統合 |
| **ClearTask (React Native版)** | 📋 計画中 | クロスプラットフォーム開発能力証明 | React Native, TypeScript, Firebase | iOS/Android対応、Web版との統一設計 |
| **SmartHire** | 📋 計画中 | AI連携・外部API統合能力証明 | React Native, OpenAI GPT-4 | 複雑な外部API連携、ストリーミング処理 |

---

## 📱 Project 1: ClearTask (Web版) ✅ **実装済み**

**リポジトリ**: [https://github.com/akira882/PwD-Task-Management-App](https://github.com/akira882/PwD-Task-Management-App)

### **認知障害者向けシンプルなタスク管理Webアプリケーション**

#### 🎯 解決する課題

**問題**: 発達障害や注意欠如多動症を持つユーザーが、既存のタスク管理ツールの複雑さにストレスを感じ、効果的なタスク管理が困難

**解決策**:
- **3ステップでタスク追加** - 複雑な操作を排除
- **優先度自動計算** - 重要度×3+緊度による自動スコアリング
- **視覚的フィードバック** - 進捗バー、期限アラートで達成感を可視化

---

#### 🛠️ 技術スタック（実装済み）

```typescript
Frontend:
  - HTML5, CSS3
  - JavaScript (ES6+)
  - TypeScript 34.0%

Backend:
  - Firebase Authentication v10.7.1 (Google/メール認証)
  - Cloud Firestore v10.7.1 (NoSQLデータベース)
  - Firebase Hosting

Security & Accessibility:
  - XSS対策実装
  - WCAG 2.1 AAレベル準拠
```

**開発状況**:
- ✅ Firebase統合完了（13コミット）
- ✅ 認証機能実装（Google/メール）
- ✅ 基本CRUD操作完了
- 🔄 UI/UX改善中
- 📅 最終更新: 2025-12-26

---

#### 🚀 主要機能

##### 1. タスク管理（CRUD）

```typescript
// タスクの型定義
export interface Task {
  id: string;
  title: string;
  description: string;
  completed: boolean;
  priority: 'high' | 'medium' | 'low';
  dueDate: Date | null;
  createdAt: Date;
  updatedAt: Date;
  userId: string;
}

// CRUD操作
export interface TaskService {
  createTask(input: CreateTaskInput): Promise<Task>;
  updateTask(id: string, updates: Partial<Task>): Promise<void>;
  deleteTask(id: string): Promise<void>;
  getTasks(userId: string): Promise<Task[]>;
  subscribeToTasks(userId: string, callback: (tasks: Task[]) => void): () => void;
}
```

**実装のポイント**:
- Firestore onSnapshotによるリアルタイム同期
- 楽観的UI更新（Optimistic UI）
- エラーハンドリング・リトライロジック

---

##### 2. 1タスク1画面表示

```typescript
// 認知負荷を最小化するUI設計
const TaskFocusScreen: React.FC = () => {
  const { tasks } = useTasks();
  const [currentIndex, setCurrentIndex] = useState(0);
  const currentTask = tasks[currentIndex];

  return (
    <View style={styles.container}>
      {/* 現在のタスクのみ表示 */}
      <TaskCard task={currentTask} fullScreen />

      {/* 進捗インジケーター */}
      <ProgressBar
        current={currentIndex + 1}
        total={tasks.length}
      />

      {/* シンプルなナビゲーション */}
      <NavigationButtons
        onPrevious={() => setCurrentIndex(i => Math.max(0, i - 1))}
        onNext={() => setCurrentIndex(i => Math.min(tasks.length - 1, i + 1))}
        hasPrevious={currentIndex > 0}
        hasNext={currentIndex < tasks.length - 1}
      />
    </View>
  );
};
```

**UX設計の工夫**:
- 視覚的ノイズの排除
- 大きなタップターゲット（44x44px以上）
- 色覚異常対応（カラーパレット）

---

##### 3. 音声入力対応

```typescript
// Expo Speech Recognitionの統合
import * as Speech from 'expo-speech';
import { Audio } from 'expo-av';

export const useVoiceInput = () => {
  const [isRecording, setIsRecording] = useState(false);
  const [transcript, setTranscript] = useState('');

  const startRecording = async () => {
    const { status } = await Audio.requestPermissionsAsync();
    if (status !== 'granted') {
      throw new Error('Microphone permission denied');
    }

    setIsRecording(true);
    // 音声認識開始
  };

  const stopRecording = async () => {
    setIsRecording(false);
    // 音声認識停止
  };

  return { isRecording, transcript, startRecording, stopRecording };
};
```

**アクセシビリティ向上**:
- 手入力が困難なユーザーへの配慮
- スクリーンリーダー対応
- ボイスオーバーによるフィードバック

---

##### 4. 視覚的進捗管理

```typescript
// 進捗バーとモチベーション維持
const ProgressDashboard: React.FC = () => {
  const { tasks } = useTasks();
  const completedCount = tasks.filter(t => t.completed).length;
  const completionRate = (completedCount / tasks.length) * 100;

  return (
    <View>
      {/* 全体進捗 */}
      <ProgressBar value={completionRate} />

      {/* 統計情報 */}
      <StatsCard
        totalTasks={tasks.length}
        completedTasks={completedCount}
        remainingTasks={tasks.length - completedCount}
      />

      {/* 称賛メッセージ */}
      {completionRate > 80 && (
        <PraiseMessage message="素晴らしい進捗です！" />
      )}
    </View>
  );
};
```

**モチベーション設計**:
- 視覚的フィードバック（アニメーション）
- 達成感の可視化
- 段階的な称賛メッセージ

---

#### 🏗️ アーキテクチャ

```
src/
├── components/
│   ├── common/              # 汎用UIコンポーネント
│   │   ├── Button.tsx
│   │   ├── Input.tsx
│   │   ├── ProgressBar.tsx
│   │   └── Card.tsx
│   └── features/            # 機能固有コンポーネント
│       ├── TaskCard.tsx
│       ├── TaskList.tsx
│       ├── VoiceInputButton.tsx
│       └── PraiseMessage.tsx
│
├── screens/
│   ├── HomeScreen.tsx
│   ├── TaskFocusScreen.tsx
│   ├── TaskDetailScreen.tsx
│   ├── ProgressScreen.tsx
│   └── SettingsScreen.tsx
│
├── navigation/
│   └── AppNavigator.tsx
│
├── contexts/
│   ├── TaskContext.tsx
│   └── AuthContext.tsx
│
├── services/
│   ├── firebase.ts
│   ├── taskService.ts
│   └── authService.ts
│
├── hooks/
│   ├── useTasks.ts
│   ├── useAuth.ts
│   ├── useVoiceInput.ts
│   └── useTaskOperations.ts
│
├── types/
│   ├── task.ts
│   ├── user.ts
│   └── index.ts
│
└── utils/
    ├── formatDate.ts
    ├── validation.ts
    └── constants.ts
```

---

#### 🧪 テスト戦略

```typescript
// 1. コンポーネントテスト
describe('TaskCard', () => {
  it('renders task information correctly', () => {
    const { getByText } = render(<TaskCard task={mockTask} />);
    expect(getByText(mockTask.title)).toBeTruthy();
  });

  it('calls onPress when tapped', () => {
    const onPress = jest.fn();
    const { getByTestId } = render(<TaskCard task={mockTask} onPress={onPress} />);
    fireEvent.press(getByTestId('task-card'));
    expect(onPress).toHaveBeenCalled();
  });
});

// 2. カスタムフックテスト
describe('useTasks', () => {
  it('loads tasks from Firestore', async () => {
    const { result, waitForNextUpdate } = renderHook(() => useTasks());
    await waitForNextUpdate();
    expect(result.current.tasks).toHaveLength(3);
  });
});

// 3. E2Eテスト（Detox）
describe('ClearTask App', () => {
  beforeAll(async () => {
    await device.launchApp();
  });

  it('should add a new task', async () => {
    await element(by.id('add-task-button')).tap();
    await element(by.id('task-title-input')).typeText('New Task');
    await element(by.id('save-button')).tap();
    await expect(element(by.text('New Task'))).toBeVisible();
  });
});
```

**テストカバレッジ目標**: 80%以上

---

#### 📈 採用責任者へのアピールポイント

✅ **実務レベルのアーキテクチャ**
- 関心の分離（Separation of Concerns）
- スケーラブルなフォルダ構成
- 保守性・拡張性を考慮した設計

✅ **TypeScript完全型付け**
- strict mode有効
- Generics、Utility Typesの活用
- 型安全な状態管理

✅ **パフォーマンス最適化**
- FlatListの最適化
- useMemo/useCallbackによるメモ化
- 不要な再レンダリング抑制

✅ **アクセシビリティ重視**
- スクリーンリーダー対応
- 音声入力対応
- 認知負荷を考慮したUI設計

✅ **テスト実装**
- 単体テスト（Jest）
- コンポーネントテスト（React Native Testing Library）
- E2Eテスト（Detox）

---

## 📱 Project 2: SmartHire

### **AI面接準備アプリ**

#### 🎯 解決する課題

**問題**: 就職活動における面接準備の効率化と、障がい者雇用面接特有の課題（配慮事項の伝え方、想定質問への対策）への対応が不十分

**解決策**: OpenAI GPT-4を活用した模擬面接と、音声認識による実践的な面接練習環境を提供

---

#### 🛠️ 技術スタック

```typescript
Frontend:
  - React Native (Expo)
  - TypeScript
  - React Navigation

AI/ML:
  - OpenAI GPT-4 API
  - ストリーミングレスポンス処理

Voice:
  - Expo Speech Recognition
  - Text-to-Speech

Backend:
  - Firebase Firestore (面接履歴保存)
  - Firebase Functions (サーバーレス関数)

State Management:
  - Context API + useReducer
```

---

#### 🚀 主要機能

##### 1. AI模擬面接

```typescript
// OpenAI GPT-4による模擬面接
export class InterviewService {
  private openai: OpenAI;

  async conductInterview(
    jobDescription: string,
    userProfile: UserProfile,
    onChunk?: (text: string) => void
  ): Promise<InterviewSession> {
    const systemPrompt = `
      あなたは経験豊富な採用担当者です。
      以下の職務内容に基づいて、面接を実施してください。

      職務内容: ${jobDescription}
      応募者プロフィール: ${JSON.stringify(userProfile)}

      面接では以下を評価してください：
      1. 技術的スキル
      2. コミュニケーション能力
      3. 問題解決能力
      4. チーム適応性
    `;

    const response = await this.openai.chat.completions.create({
      model: 'gpt-4',
      messages: [
        { role: 'system', content: systemPrompt },
        { role: 'user', content: 'よろしくお願いします' },
      ],
      stream: true,
    });

    // ストリーミングレスポンスの処理
    for await (const chunk of response) {
      const text = chunk.choices[0]?.delta?.content || '';
      onChunk?.(text);
    }

    return interviewSession;
  }

  async evaluateAnswer(
    question: string,
    answer: string
  ): Promise<Evaluation> {
    const prompt = `
      面接質問: ${question}
      回答: ${answer}

      以下の観点から評価してください：
      1. 質問への適切な回答（1-5点）
      2. 具体性（1-5点）
      3. 論理性（1-5点）

      改善点も提示してください。
    `;

    const response = await this.openai.chat.completions.create({
      model: 'gpt-4',
      messages: [{ role: 'user', content: prompt }],
    });

    return JSON.parse(response.choices[0].message.content);
  }
}
```

---

##### 2. 音声認識による回答入力

```typescript
// リアルタイム音声認識
export const useInterviewVoice = () => {
  const [isListening, setIsListening] = useState(false);
  const [transcript, setTranscript] = useState('');

  const startListening = async () => {
    const { status } = await Audio.requestPermissionsAsync();
    if (status !== 'granted') return;

    setIsListening(true);
    // 音声認識開始
  };

  const stopListening = async () => {
    setIsListening(false);
    // 音声認識停止
  };

  return { isListening, transcript, startListening, stopListening };
};
```

---

##### 3. フィードバックとスコアリング

```typescript
// 面接評価の可視化
export interface InterviewEvaluation {
  overallScore: number;        // 総合スコア (0-100)
  technicalSkills: number;     // 技術スキル (0-100)
  communication: number;       // コミュニケーション能力 (0-100)
  problemSolving: number;      // 問題解決能力 (0-100)
  strengths: string[];         // 強み
  improvements: string[];      // 改善点
  feedback: string;            // 詳細フィードバック
}

const EvaluationScreen: React.FC<{ evaluation: InterviewEvaluation }> = ({
  evaluation,
}) => {
  return (
    <ScrollView>
      {/* 総合スコア */}
      <ScoreCard score={evaluation.overallScore} />

      {/* 項目別スコア */}
      <SkillRadarChart
        technical={evaluation.technicalSkills}
        communication={evaluation.communication}
        problemSolving={evaluation.problemSolving}
      />

      {/* フィードバック */}
      <FeedbackSection
        strengths={evaluation.strengths}
        improvements={evaluation.improvements}
        feedback={evaluation.feedback}
      />
    </ScrollView>
  );
};
```

---

#### 📈 採用責任者へのアピールポイント

✅ **複雑な外部API連携**
- OpenAI GPT-4 APIのストリーミング処理
- エラーハンドリング・リトライロジック
- レート制限対応

✅ **非同期処理の徹底**
- Promise、async/awaitの適切な使用
- 並行処理の最適化

✅ **音声技術の統合**
- Speech Recognition
- Text-to-Speech
- リアルタイム処理

---

## 📚 Project 3: Learning Journey

### **学習記録・技術検証リポジトリ**

#### 🎯 目的

技術的成長過程と継続的学習姿勢の可視化

---

#### 📂 構成

```
react-native-learning-journey/
├── README.md                          # 学習ロードマップ
├── PROGRESS.md                        # 週次進捗レポート
│
├── week-01-javascript-basics/
│   ├── README.md
│   ├── day-01-variables/
│   ├── day-02-functions/
│   └── day-05-mini-project/
│
├── week-02-typescript-basics/
├── week-03-react-basics/
├── week-04-react-intermediate/
├── week-05-react-native-setup/
├── week-06-react-native-fundamentals/
├── week-07-navigation-state/
├── week-08-firebase-integration/
├── week-09-advanced-patterns/
├── week-10-testing/
├── week-11-performance/
└── week-12-deployment/
```

---

#### 📈 採用責任者へのアピールポイント

✅ **継続的学習姿勢**
- 12週間の体系的な学習記録
- 毎日のコミット履歴

✅ **技術的深化**
- 週次ミニプロジェクト
- トラブルシューティング記録

✅ **自己マネジメント能力**
- 計画的な学習
- 進捗の可視化

---

## 📊 3つのプロジェクトの相乗効果

### **採用責任者が見るポイント**

| 観点 | ClearTask | SmartHire | Learning Journey |
|------|-----------|-----------|------------------|
| **技術的深さ** | Firebase統合、リアルタイム同期 | OpenAI API、ストリーミング処理 | 基礎から応用までの学習過程 |
| **問題解決能力** | 認知負荷を考慮したUX設計 | AI活用による効率化 | トラブルシューティング記録 |
| **コード品質** | TypeScript完全型付け、テスト | 非同期処理、エラーハンドリング | コード品質の向上過程 |
| **継続的成長** | パフォーマンス最適化 | 新技術（AI）への挑戦 | 12週間の学習記録 |

---

## 🎯 実装ロードマップ

### **Week 1-4: 基礎固め**
- JavaScript/TypeScript完全習得
- React基礎・応用

### **Week 5-8: React Native開発**
- 環境構築
- ClearTaskプロジェクト開始
- Firebase統合

### **Week 9-10: 高度な機能実装**
- SmartHireプロジェクト開始
- OpenAI API統合

### **Week 11-12: 仕上げ**
- テスト実装
- ドキュメント充実
- デプロイ

---

## ✅ 完成時の成果物

### **各プロジェクトに含まれるもの**

1. **実装コード**
   - TypeScript完全型付け
   - ESLint/Prettier準拠
   - コメント・ドキュメント充実

2. **テストコード**
   - 単体テスト
   - E2Eテスト
   - カバレッジ80%以上

3. **ドキュメント**
   - README.md（セットアップ手順、機能説明）
   - ARCHITECTURE.md（設計思想）
   - TECHNICAL_DECISIONS.md（技術選定理由）

4. **デモ**
   - スクリーンショット
   - デモ動画（GIF）
   - Expo公開URL

---

**最終更新**: 2025-12-27
