# 🛠️ Technical Skills - React Native Developer

**実務レベルのスキルセットと技術的深さの証明**

---

## 📋 目次

1. [Frontend Development](#frontend-development)
2. [Backend & Services](#backend--services)
3. [Development Tools & Practices](#development-tools--practices)
4. [UX/UI Design](#uxui-design)
5. [実務経験相当のスキル証明](#実務経験相当のスキル証明)

---

## Frontend Development

### React Native (Expo)

#### **習得レベル: 実務レベル**

**できること:**
- クロスプラットフォーム（iOS/Android）アプリ開発
- Expo Managed Workflowでの開発から本番デプロイまで
- ネイティブ機能の活用（カメラ、位置情報、音声認識）

**実装経験:**

```typescript
// ✅ FlatListの最適化
import { FlatList, memo } from 'react-native';

const TaskList: React.FC<Props> = ({ tasks }) => {
  const renderItem = useCallback(({ item }: { item: Task }) => (
    <TaskCard task={item} />
  ), []);

  const keyExtractor = useCallback((item: Task) => item.id, []);

  return (
    <FlatList
      data={tasks}
      renderItem={renderItem}
      keyExtractor={keyExtractor}
      removeClippedSubviews={true}
      maxToRenderPerBatch={10}
      windowSize={5}
    />
  );
};
```

```typescript
// ✅ React Navigation v6による画面遷移管理
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { NavigationContainer } from '@react-navigation/navigation';

export type RootStackParamList = {
  Home: undefined;
  TaskDetail: { taskId: string };
  Settings: undefined;
};

const Stack = createNativeStackNavigator<RootStackParamList>();

export const AppNavigator = () => {
  return (
    <NavigationContainer>
      <Stack.Navigator
        initialRouteName="Home"
        screenOptions={{
          headerStyle: { backgroundColor: '#6200EE' },
          headerTintColor: '#fff',
        }}
      >
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="TaskDetail" component={TaskDetailScreen} />
        <Stack.Screen name="Settings" component={SettingsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
};
```

```typescript
// ✅ Expo Speech Recognitionによる音声入力
import * as Speech from 'expo-speech';
import { Audio } from 'expo-av';

export const useSpeechRecognition = () => {
  const [isRecording, setIsRecording] = useState(false);
  const [transcript, setTranscript] = useState('');

  const startRecording = async () => {
    const { status } = await Audio.requestPermissionsAsync();
    if (status !== 'granted') {
      throw new Error('Permission denied');
    }

    setIsRecording(true);
    // 音声認識開始ロジック
  };

  const stopRecording = async () => {
    setIsRecording(false);
    // 音声認識停止ロジック
  };

  return { isRecording, transcript, startRecording, stopRecording };
};
```

**パフォーマンス最適化:**
- `useMemo` / `useCallback` による不要な再レンダリング抑制
- `React.memo` によるコンポーネントメモ化
- FlatListの最適化設定（`removeClippedSubviews`, `windowSize`）
- 画像の遅延読み込み（Lazy Loading）

**実装した機能:**
- タブナビゲーション、スタックナビゲーション
- モーダル、ボトムシート
- アニメーション（React Native Reanimated）
- カスタムフックによるロジック分離

---

### TypeScript

#### **習得レベル: 実務レベル（strict mode有効）**

**できること:**
- 完全型付けによる型安全なコード設計
- Generics、Utility Types の活用
- インターフェースによる契約プログラミング

**実装経験:**

```typescript
// ✅ Genericsを使った型安全な状態管理
export interface ApiResponse<T> {
  data: T;
  error: string | null;
  loading: boolean;
}

export const useApi = <T,>(fetcher: () => Promise<T>): ApiResponse<T> => {
  const [data, setData] = useState<T | null>(null);
  const [error, setError] = useState<string | null>(null);
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    const fetchData = async () => {
      setLoading(true);
      try {
        const result = await fetcher();
        setData(result);
      } catch (err) {
        setError(err instanceof Error ? err.message : 'Unknown error');
      } finally {
        setLoading(false);
      }
    };
    fetchData();
  }, [fetcher]);

  return { data, error, loading };
};
```

```typescript
// ✅ Utility Typesの活用
export interface Task {
  id: string;
  title: string;
  description: string;
  completed: boolean;
  createdAt: Date;
  updatedAt: Date;
}

// 作成時は id, createdAt, updatedAt 不要
export type CreateTaskInput = Omit<Task, 'id' | 'createdAt' | 'updatedAt'>;

// 更新時は一部のフィールドのみ変更可能
export type UpdateTaskInput = Pick<Task, 'title' | 'description' | 'completed'>;

// 部分的な更新
export type PartialUpdateTaskInput = Partial<UpdateTaskInput>;
```

```typescript
// ✅ Union型とType Guardによる型安全な分岐処理
export type TaskStatus = 'active' | 'completed' | 'archived';

export interface TaskBase {
  id: string;
  title: string;
}

export interface ActiveTask extends TaskBase {
  status: 'active';
  dueDate: Date;
}

export interface CompletedTask extends TaskBase {
  status: 'completed';
  completedAt: Date;
}

export type Task = ActiveTask | CompletedTask;

// Type Guard
export const isActiveTask = (task: Task): task is ActiveTask => {
  return task.status === 'active';
};

// 使用例
const renderTask = (task: Task) => {
  if (isActiveTask(task)) {
    // task は ActiveTask 型として扱える
    console.log(task.dueDate);
  } else {
    // task は CompletedTask 型として扱える
    console.log(task.completedAt);
  }
};
```

**型安全性の徹底:**
- `tsconfig.json` で `strict: true` を有効化
- `noImplicitAny`, `strictNullChecks` による厳格な型チェック
- 全関数に明示的な戻り値の型注釈
- `any` 型の使用禁止（必要な場合は `unknown` を使用）

---

### State Management

#### **習得レベル: 実務レベル**

**できること:**
- Context API + useReducer パターンによるグローバル状態管理
- カスタムフックによる状態ロジック分離
- 適切な状態の配置（グローバル vs ローカル）

**実装経験:**

```typescript
// ✅ Context API + useReducer による状態管理
import { createContext, useContext, useReducer, ReactNode } from 'react';

// State型定義
export interface TaskState {
  tasks: Task[];
  loading: boolean;
  error: string | null;
}

// Action型定義
type TaskAction =
  | { type: 'SET_TASKS'; payload: Task[] }
  | { type: 'ADD_TASK'; payload: Task }
  | { type: 'UPDATE_TASK'; payload: { id: string; updates: Partial<Task> } }
  | { type: 'DELETE_TASK'; payload: string }
  | { type: 'SET_LOADING'; payload: boolean }
  | { type: 'SET_ERROR'; payload: string };

// Reducer
const taskReducer = (state: TaskState, action: TaskAction): TaskState => {
  switch (action.type) {
    case 'SET_TASKS':
      return { ...state, tasks: action.payload, loading: false };
    case 'ADD_TASK':
      return { ...state, tasks: [...state.tasks, action.payload] };
    case 'UPDATE_TASK':
      return {
        ...state,
        tasks: state.tasks.map((task) =>
          task.id === action.payload.id
            ? { ...task, ...action.payload.updates }
            : task
        ),
      };
    case 'DELETE_TASK':
      return {
        ...state,
        tasks: state.tasks.filter((task) => task.id !== action.payload),
      };
    case 'SET_LOADING':
      return { ...state, loading: action.payload };
    case 'SET_ERROR':
      return { ...state, error: action.payload, loading: false };
    default:
      return state;
  }
};

// Context作成
const TaskContext = createContext<{
  state: TaskState;
  dispatch: React.Dispatch<TaskAction>;
} | undefined>(undefined);

// Provider
export const TaskProvider: React.FC<{ children: ReactNode }> = ({ children }) => {
  const [state, dispatch] = useReducer(taskReducer, {
    tasks: [],
    loading: false,
    error: null,
  });

  return (
    <TaskContext.Provider value={{ state, dispatch }}>
      {children}
    </TaskContext.Provider>
  );
};

// カスタムフック
export const useTasks = () => {
  const context = useContext(TaskContext);
  if (!context) {
    throw new Error('useTasks must be used within TaskProvider');
  }
  return context;
};
```

```typescript
// ✅ カスタムフックによるロジック分離
export const useTaskOperations = () => {
  const { dispatch } = useTasks();

  const addTask = useCallback(async (input: CreateTaskInput) => {
    dispatch({ type: 'SET_LOADING', payload: true });
    try {
      const newTask = await taskService.createTask(input);
      dispatch({ type: 'ADD_TASK', payload: newTask });
    } catch (error) {
      dispatch({ type: 'SET_ERROR', payload: error.message });
    }
  }, [dispatch]);

  const updateTask = useCallback(async (id: string, updates: Partial<Task>) => {
    dispatch({ type: 'SET_LOADING', payload: true });
    try {
      await taskService.updateTask(id, updates);
      dispatch({ type: 'UPDATE_TASK', payload: { id, updates } });
    } catch (error) {
      dispatch({ type: 'SET_ERROR', payload: error.message });
    }
  }, [dispatch]);

  const deleteTask = useCallback(async (id: string) => {
    dispatch({ type: 'SET_LOADING', payload: true });
    try {
      await taskService.deleteTask(id);
      dispatch({ type: 'DELETE_TASK', payload: id });
    } catch (error) {
      dispatch({ type: 'SET_ERROR', payload: error.message });
    }
  }, [dispatch]);

  return { addTask, updateTask, deleteTask };
};
```

---

## Backend & Services

### Firebase

#### **習得レベル: 実務レベル**

**できること:**
- Firestore: NoSQLデータベース設計、CRUD操作
- Firebase Auth: メール/匿名認証
- リアルタイム同期（onSnapshot）
- オフライン対応

**実装経験:**

```typescript
// ✅ Firestoreのリアルタイム同期
import {
  collection,
  doc,
  onSnapshot,
  addDoc,
  updateDoc,
  deleteDoc,
  query,
  where,
  orderBy,
  Timestamp,
} from 'firebase/firestore';
import { db } from './firebase';

export class TaskService {
  private collectionName = 'tasks';

  // リアルタイムリスナー
  subscribeToTasks(userId: string, callback: (tasks: Task[]) => void) {
    const q = query(
      collection(db, this.collectionName),
      where('userId', '==', userId),
      orderBy('createdAt', 'desc')
    );

    return onSnapshot(
      q,
      (snapshot) => {
        const tasks = snapshot.docs.map((doc) => ({
          id: doc.id,
          ...doc.data(),
          createdAt: doc.data().createdAt.toDate(),
          updatedAt: doc.data().updatedAt.toDate(),
        })) as Task[];
        callback(tasks);
      },
      (error) => {
        console.error('Error subscribing to tasks:', error);
      }
    );
  }

  // タスク作成
  async createTask(userId: string, input: CreateTaskInput): Promise<Task> {
    try {
      const docRef = await addDoc(collection(db, this.collectionName), {
        ...input,
        userId,
        createdAt: Timestamp.now(),
        updatedAt: Timestamp.now(),
      });

      return {
        id: docRef.id,
        ...input,
        createdAt: new Date(),
        updatedAt: new Date(),
      };
    } catch (error) {
      throw new Error(`Failed to create task: ${error.message}`);
    }
  }

  // タスク更新
  async updateTask(taskId: string, updates: Partial<Task>): Promise<void> {
    try {
      const taskRef = doc(db, this.collectionName, taskId);
      await updateDoc(taskRef, {
        ...updates,
        updatedAt: Timestamp.now(),
      });
    } catch (error) {
      throw new Error(`Failed to update task: ${error.message}`);
    }
  }

  // タスク削除
  async deleteTask(taskId: string): Promise<void> {
    try {
      const taskRef = doc(db, this.collectionName, taskId);
      await deleteDoc(taskRef);
    } catch (error) {
      throw new Error(`Failed to delete task: ${error.message}`);
    }
  }
}

export const taskService = new TaskService();
```

```typescript
// ✅ Firebase Authentication
import {
  getAuth,
  signInWithEmailAndPassword,
  createUserWithEmailAndPassword,
  signOut,
  onAuthStateChanged,
  User,
} from 'firebase/auth';

export class AuthService {
  private auth = getAuth();

  // ユーザー登録
  async signUp(email: string, password: string): Promise<User> {
    try {
      const userCredential = await createUserWithEmailAndPassword(
        this.auth,
        email,
        password
      );
      return userCredential.user;
    } catch (error) {
      throw new Error(`Sign up failed: ${error.message}`);
    }
  }

  // ログイン
  async signIn(email: string, password: string): Promise<User> {
    try {
      const userCredential = await signInWithEmailAndPassword(
        this.auth,
        email,
        password
      );
      return userCredential.user;
    } catch (error) {
      throw new Error(`Sign in failed: ${error.message}`);
    }
  }

  // ログアウト
  async signOut(): Promise<void> {
    try {
      await signOut(this.auth);
    } catch (error) {
      throw new Error(`Sign out failed: ${error.message}`);
    }
  }

  // 認証状態の監視
  onAuthStateChanged(callback: (user: User | null) => void) {
    return onAuthStateChanged(this.auth, callback);
  }
}

export const authService = new AuthService();
```

**セキュリティルール設計:**
```javascript
// Firestore Security Rules
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /tasks/{taskId} {
      // 認証済みユーザーのみアクセス可能
      allow read, write: if request.auth != null
                        && resource.data.userId == request.auth.uid;
    }
  }
}
```

---

### RESTful API Integration

#### **習得レベル: 実務レベル**

**できること:**
- axiosによるHTTP通信
- エラーハンドリング、リトライロジック
- OpenAI GPT-4 APIのストリーミング処理

**実装経験:**

```typescript
// ✅ OpenAI GPT-4 APIのストリーミング処理
import axios, { AxiosError } from 'axios';

export class OpenAIService {
  private apiKey: string;
  private baseURL = 'https://api.openai.com/v1';

  constructor(apiKey: string) {
    this.apiKey = apiKey;
  }

  async createChatCompletion(
    messages: { role: string; content: string }[],
    onChunk?: (chunk: string) => void
  ): Promise<string> {
    try {
      const response = await axios.post(
        `${this.baseURL}/chat/completions`,
        {
          model: 'gpt-4',
          messages,
          stream: true,
        },
        {
          headers: {
            'Content-Type': 'application/json',
            Authorization: `Bearer ${this.apiKey}`,
          },
          responseType: 'stream',
        }
      );

      let fullResponse = '';

      // ストリーミングレスポンスの処理
      response.data.on('data', (chunk: Buffer) => {
        const text = chunk.toString();
        fullResponse += text;
        onChunk?.(text);
      });

      return new Promise((resolve, reject) => {
        response.data.on('end', () => resolve(fullResponse));
        response.data.on('error', reject);
      });
    } catch (error) {
      if (axios.isAxiosError(error)) {
        const axiosError = error as AxiosError;
        if (axiosError.response?.status === 429) {
          throw new Error('Rate limit exceeded');
        } else if (axiosError.response?.status === 401) {
          throw new Error('Invalid API key');
        }
      }
      throw new Error(`API request failed: ${error.message}`);
    }
  }

  // リトライロジック
  async retryRequest<T>(
    fn: () => Promise<T>,
    maxRetries = 3,
    delay = 1000
  ): Promise<T> {
    for (let i = 0; i < maxRetries; i++) {
      try {
        return await fn();
      } catch (error) {
        if (i === maxRetries - 1) throw error;
        await new Promise((resolve) => setTimeout(resolve, delay * (i + 1)));
      }
    }
    throw new Error('Max retries exceeded');
  }
}
```

---

## Development Tools & Practices

### Code Quality

**ESLint + Prettier設定:**

```json
// .eslintrc.json
{
  "extends": [
    "airbnb",
    "airbnb-typescript",
    "plugin:@typescript-eslint/recommended",
    "plugin:react-hooks/recommended",
    "prettier"
  ],
  "parserOptions": {
    "project": "./tsconfig.json"
  },
  "rules": {
    "react/react-in-jsx-scope": "off",
    "react/prop-types": "off",
    "@typescript-eslint/no-explicit-any": "error",
    "@typescript-eslint/explicit-function-return-type": "warn"
  }
}
```

```json
// .prettierrc
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 80
}
```

### Testing

**Jest + React Native Testing Library:**

```typescript
// ✅ コンポーネントテスト
import { render, fireEvent, waitFor } from '@testing-library/react-native';
import { TaskCard } from './TaskCard';

describe('TaskCard', () => {
  const mockTask: Task = {
    id: '1',
    title: 'Test Task',
    description: 'Test Description',
    completed: false,
    createdAt: new Date(),
    updatedAt: new Date(),
  };

  it('renders task title and description', () => {
    const { getByText } = render(<TaskCard task={mockTask} />);
    expect(getByText('Test Task')).toBeTruthy();
    expect(getByText('Test Description')).toBeTruthy();
  });

  it('calls onPress when card is pressed', () => {
    const onPress = jest.fn();
    const { getByTestId } = render(
      <TaskCard task={mockTask} onPress={onPress} />
    );
    fireEvent.press(getByTestId('task-card'));
    expect(onPress).toHaveBeenCalledWith(mockTask.id);
  });

  it('toggles completed state', async () => {
    const onToggle = jest.fn();
    const { getByTestId } = render(
      <TaskCard task={mockTask} onToggle={onToggle} />
    );
    fireEvent.press(getByTestId('toggle-button'));
    await waitFor(() => {
      expect(onToggle).toHaveBeenCalledWith(mockTask.id, true);
    });
  });
});
```

```typescript
// ✅ カスタムフックのテスト
import { renderHook, act } from '@testing-library/react-hooks';
import { useTaskOperations } from './useTaskOperations';

describe('useTaskOperations', () => {
  it('adds a task successfully', async () => {
    const { result } = renderHook(() => useTaskOperations());

    await act(async () => {
      await result.current.addTask({
        title: 'New Task',
        description: 'Description',
        completed: false,
      });
    });

    expect(result.current.tasks).toHaveLength(1);
    expect(result.current.tasks[0].title).toBe('New Task');
  });
});
```

### Git/GitHub

**Conventional Commits準拠:**

```bash
# 機能追加
git commit -m "feat: タスク追加機能を実装"

# バグ修正
git commit -m "fix: Firebase接続エラーを修正"

# ドキュメント
git commit -m "docs: README.mdにセットアップ手順を追加"

# リファクタリング
git commit -m "refactor: TaskServiceをクラスベースに変更"

# テスト
git commit -m "test: TaskCardコンポーネントのテストを追加"

# スタイル
git commit -m "style: ESLintエラーを修正"
```

**ブランチ戦略:**
```bash
main          # 本番環境
├── develop   # 開発環境
    ├── feature/task-list      # 機能開発
    ├── feature/voice-input    # 機能開発
    └── fix/firebase-auth      # バグ修正
```

---

## UX/UI Design

### アクセシビリティ

**当事者視点での設計:**

```typescript
// ✅ スクリーンリーダー対応
<TouchableOpacity
  accessibilityLabel="タスクを追加"
  accessibilityHint="タップして新しいタスクを作成します"
  accessibilityRole="button"
  onPress={handleAddTask}
>
  <Text>追加</Text>
</TouchableOpacity>
```

```typescript
// ✅ 認知負荷を考慮したUI設計
// 1画面1タスク表示による集中力維持
const TaskFocusScreen: React.FC = () => {
  const [currentTaskIndex, setCurrentTaskIndex] = useState(0);
  const currentTask = tasks[currentTaskIndex];

  return (
    <View style={styles.container}>
      {/* 1つのタスクのみ表示 */}
      <TaskCard task={currentTask} />

      {/* シンプルなナビゲーション */}
      <View style={styles.navigation}>
        <Button title="前のタスク" onPress={handlePrevious} />
        <Button title="次のタスク" onPress={handleNext} />
      </View>
    </View>
  );
};
```

---

## 実務経験相当のスキル証明

### ✅ 実装できる機能

1. **認証・ユーザー管理**
   - Firebase AuthenticationによるEmail/Password認証
   - ソーシャルログイン（Google、Apple）
   - パスワードリセット機能

2. **データ管理**
   - Firestore CRUD操作
   - リアルタイム同期
   - オフライン対応
   - ページネーション

3. **ネイティブ機能連携**
   - カメラ・写真ライブラリアクセス
   - 音声認識・音声合成
   - プッシュ通知（Expo Notifications）
   - 位置情報取得

4. **外部API連携**
   - OpenAI GPT-4 API
   - RESTful API通信
   - GraphQL（Apollo Client）

5. **パフォーマンス最適化**
   - FlatList最適化
   - 画像の遅延読み込み
   - メモ化による再レンダリング抑制

6. **テスト**
   - 単体テスト（Jest）
   - コンポーネントテスト（React Native Testing Library）
   - E2Eテスト（Detox）

---

### ✅ 実務レベルのコード品質

- TypeScript完全型付け（strict mode有効）
- ESLint/Prettier準拠
- コードレビュー視点での自己チェック
- エラーハンドリング完備
- ドキュメント・コメント充実

---

### ✅ アーキテクチャ設計能力

```
src/
├── components/     # 再利用可能なUIコンポーネント
│   ├── common/     # 汎用コンポーネント
│   └── features/   # 機能固有コンポーネント
├── screens/        # 画面単位のコンポーネント
├── navigation/     # 画面遷移の管理
├── contexts/       # グローバル状態（Context API）
├── services/       # 外部API通信ロジック
├── hooks/          # カスタムフック
├── types/          # TypeScript型定義
└── utils/          # ユーティリティ関数
```

**設計原則:**
- 関心の分離（Separation of Concerns）
- 単一責任の原則（Single Responsibility Principle）
- DRY（Don't Repeat Yourself）
- YAGNI（You Aren't Gonna Need It） - 過剰設計の回避

---

## 📚 継続的学習

### 学習リソース

- **公式ドキュメント**: React Native、TypeScript、Firebase
- **技術書**: 『React Native実践入門』、『TypeScript実践ガイド』
- **オンラインコース**: Udemy、Pluralsight
- **技術記事**: Qiita、Zenn、Medium

### アウトプット

- GitHubでのコード公開
- 技術ブログ執筆（予定）
- ポートフォリオプロジェクト（3つ完成予定）

---

**最終更新**: 2025-12-27
