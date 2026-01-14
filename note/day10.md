# Day 10: Ref机制 (Ref Mechanism)

## 今日学习 (Today's Learning)
- 核心概念: createRef 实现、不可变容器模式
- 重点文件: `packages/react/src/ReactCreateRef.js`

## 源码理解 (Source Code Understanding)

### 文件: ReactCreateRef.js
- **作用**: 提供 `createRef` API，用于创建 ref 对象
- **关键函数**: `createRef()`
- **核心逻辑**: 返回一个 `{ current: null }` 对象，开发环境下使用 `Object.seal` 密封

### 设计哲学
| 概念 | 说明 |
|------|------|
| 不可变容器 | ref 对象结构固定，不能添加/删除属性 |
| 可变值 | `current` 属性可以随时修改 |
| 容器模式 | ref 像一个"盒子"，结构不变但内容可变 |

### Object.seal vs Object.freeze
| 方法 | 添加属性 | 删除属性 | 修改属性值 |
|------|----------|----------|------------|
| `Object.seal` | ❌ | ❌ | ✅ |
| `Object.freeze` | ❌ | ❌ | ❌ |

## 代码片段 (Code Snippets)

```javascript
// ReactCreateRef.js 完整实现
import type {RefObject} from 'shared/ReactTypes';

// an immutable object with a single mutable value
export function createRef(): RefObject {
  const refObject = {
    current: null,
  };
  if (__DEV__) {
    Object.seal(refObject);  // 开发环境密封对象
  }
  return refObject;
}
```

```javascript
// 类组件中使用 createRef
class MyComponent extends React.Component {
  myRef = React.createRef();  // { current: null }

  componentDidMount() {
    console.log(this.myRef.current);  // <div>...</div>
  }

  render() {
    return <div ref={this.myRef}>Hello</div>;
  }
}
```

## 疑问点 (Questions)

### Q: 函数组件中为什么推荐用 useRef 而不是 createRef？

**A:** 函数组件每次渲染都会重新执行：

```javascript
// ❌ 错误用法 - 每次渲染都创建新对象
function MyComponent() {
  const ref = createRef();  // 每次都是新的 { current: null }
  return <div ref={ref}>Hello</div>;
}

// ✅ 正确用法 - useRef 保持同一引用
function MyComponent() {
  const ref = useRef(null);  // 始终返回同一个对象
  return <div ref={ref}>Hello</div>;
}
```

### Q: useRef 为什么能"记住"？

**A:** useRef 的状态存储在 Fiber 节点的 `memoizedState` 链表中：
- 首次渲染：创建对象并存储到 Fiber
- 后续渲染：从 Fiber 中取出同一个对象返回

（将在 Day 43 深入学习 `ReactFiberHooks.js`）

## 实践验证 (Practical Verification)

```bash
# 运行相关测试
yarn test -- --testPathPattern="createRef" --verbose

# 查看 RefObject 类型定义
cat packages/shared/ReactTypes.js | grep -A 3 "RefObject"
```

## 知识关联 (Knowledge Connection)

```
Day 8-9: Component/PureComponent
    ↓
Day 10: createRef ← 今天
    ↓
Day 11-12: forwardRef (ref 转发)
    ↓
Day 43: ReactFiberHooks (useRef 实现原理)
```

## 明日任务 (Tomorrow's Tasks)
- Day 11: ForwardRef 使用
- 学习文件: `packages/react/src/ReactForwardRef.js` (第1-50行)
- 重点: 理解 forwardRef 的作用，学习 ref 转发模式
