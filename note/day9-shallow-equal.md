# Day 9: PureComponent 与浅比较 (PureComponent & Shallow Equal)

## 今日学习 (Today's Learning)
- 核心概念: 浅比较（shallowEqual）、Object.is、PureComponent 原理
- 重点文件:
  - `packages/shared/shallowEqual.js`
  - `packages/shared/objectIs.js`
  - `packages/shared/hasOwnProperty.js`

---

## 一、shallowEqual.js 完整源码

```javascript
/**
 * Copyright (c) Meta Platforms, Inc. and affiliates.
 *
 * This source code is licensed under the MIT license found in the
 * LICENSE file in the root directory of this source tree.
 *
 * @flow
 */

import is from './objectIs';
import hasOwnProperty from './hasOwnProperty';

/**
 * Performs equality by iterating through keys on an object and returning false
 * when any key has values which are not strictly equal between the arguments.
 * Returns true when the values of all keys are strictly equal.
 */
function shallowEqual(objA: mixed, objB: mixed): boolean {
  if (is(objA, objB)) {
    return true;
  }

  if (
    typeof objA !== 'object' ||
    objA === null ||
    typeof objB !== 'object' ||
    objB === null
  ) {
    return false;
  }

  const keysA = Object.keys(objA);
  const keysB = Object.keys(objB);

  if (keysA.length !== keysB.length) {
    return false;
  }

  // Test for A's keys different from B.
  for (let i = 0; i < keysA.length; i++) {
    const currentKey = keysA[i];
    if (
      !hasOwnProperty.call(objB, currentKey) ||
      !is(objA[currentKey], objB[currentKey])
    ) {
      return false;
    }
  }

  return true;
}

export default shallowEqual;
```

---

## 二、objectIs.js 完整源码

```javascript
/**
 * Copyright (c) Meta Platforms, Inc. and affiliates.
 *
 * This source code is licensed under the MIT license found in the
 * LICENSE file in the root directory of this source tree.
 *
 * @flow
 */

/**
 * inlined Object.is polyfill to avoid requiring consumers ship their own
 * https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is
 */
function is(x: any, y: any) {
  return (
    (x === y && (x !== 0 || 1 / x === 1 / y)) || (x !== x && y !== y)
  );
}

const objectIs: (x: any, y: any) => boolean =
  typeof Object.is === 'function' ? Object.is : is;

export default objectIs;
```

---

## 三、hasOwnProperty.js 完整源码

```javascript
/**
 * Copyright (c) Meta Platforms, Inc. and affiliates.
 *
 * This source code is licensed under the MIT license found in the
 * LICENSE file in the root directory of this source tree.
 *
 * @flow
 */

const hasOwnProperty = Object.prototype.hasOwnProperty;

export default hasOwnProperty;
```

---

## 四、逐行源码解析

### 4.1 objectIs.js 解析

#### Object.is vs === 的区别

| 比较 | `===` | `Object.is` |
|------|-------|-------------|
| `NaN === NaN` | `false` | `true` |
| `+0 === -0` | `true` | `false` |
| 其他情况 | 相同 | 相同 |

#### is 函数实现

```javascript
function is(x: any, y: any) {
  return (
    (x === y && (x !== 0 || 1 / x === 1 / y)) || (x !== x && y !== y)
  );
}
```

**拆解：**

**1. 处理 +0 和 -0：`x === y && (x !== 0 || 1 / x === 1 / y)`**
```javascript
// 问题：=== 认为 +0 和 -0 相等
+0 === -0  // true（但 Object.is 应该返回 false）

// 解决：用 1/x 来区分
1 / +0   // Infinity
1 / -0   // -Infinity
1 / +0 === 1 / -0  // false ✅
```

**2. 处理 NaN：`x !== x && y !== y`**
```javascript
// 问题：NaN 不等于自身
NaN === NaN  // false（但 Object.is 应该返回 true）

// 解决：利用 NaN 的特性（只��� NaN 不等于自身）
NaN !== NaN  // true
// 如果 x !== x 且 y !== y，说明两者都是 NaN
```

#### 特性检测与导出

```javascript
const objectIs: (x: any, y: any) => boolean =
  typeof Object.is === 'function' ? Object.is : is;

export default objectIs;
```

- 优先使用原生 `Object.is`（性能更好）
- 不支持时使用 polyfill

---

### 4.2 shallowEqual.js 解析

#### 粒度 1：导入语句

```javascript
import is from './objectIs';
import hasOwnProperty from './hasOwnProperty';
```

| 导入项 | 说明 |
|--------|------|
| `is` | Object.is 的实现/polyfill |
| `hasOwnProperty` | 安全的属性检查方法 |

#### 粒度 2：快速路径

```javascript
function shallowEqual(objA: mixed, objB: mixed): boolean {
  if (is(objA, objB)) {
    return true;
  }
```

- 使用 `Object.is` 先做快速判断
- 同一引用或相同原始值，直接返回 `true`
- **性能优化**：避免不必要的遍历

#### 粒度 3：类型检查

```javascript
  if (
    typeof objA !== 'object' ||
    objA === null ||
    typeof objB !== 'object' ||
    objB === null
  ) {
    return false;
  }
```

- 排除非对象类型
- 注意：`typeof null === 'object'`，需要单独判断

#### 粒度 4：键数量比较

```javascript
  const keysA = Object.keys(objA);
  const keysB = Object.keys(objB);

  if (keysA.length !== keysB.length) {
    return false;
  }
```

- 键数量不同，对象肯定不相等
- **性能优化**：快速排除

#### 粒度 5：键值比较循环

```javascript
  for (let i = 0; i < keysA.length; i++) {
    const currentKey = keysA[i];
    if (
      !hasOwnProperty.call(objB, currentKey) ||
      !is(objA[currentKey], objB[currentKey])
    ) {
      return false;
    }
  }

  return true;
}
```

**两个检查条件：**
| 条件 | 说明 |
|------|------|
| `!hasOwnProperty.call(objB, currentKey)` | B 没有这个键 |
| `!is(objA[currentKey], objB[currentKey])` | 键存在但值不相等 |

**为什么用 `hasOwnProperty.call`？**
```javascript
// 安全写法
hasOwnProperty.call(objB, currentKey)

// 不安全写法（如果 objB 重写了 hasOwnProperty 会出问题）
objB.hasOwnProperty(currentKey)
```

---

## 五、shallowEqual 算法流程图

```
shallowEqual(objA, objB)
         │
         ▼
┌─────────────────────┐
│ Object.is(A, B)?    │──── true ───→ return true
└─────────────────────┘
         │ false
         ▼
┌─────────────────────┐
│ A 或 B 不是对象?    │──── true ───→ return false
└─────────────────────┘
         │ false
         ▼
┌─────────────────────┐
│ 键数量不同?         │──── true ───→ return false
└─────────────────────┘
         │ false
         ▼
┌─────────────────────┐
│ 遍历 A 的每个键     │
│ B 有这个键?         │──── false ──→ return false
│ 值相等(Object.is)?  │──── false ──→ return false
└─────────────────────┘
         │ 全部通过
         ▼
    return true
```

---

## 六、使用示例

### ✅ 返回 true 的情况

```javascript
// 1. 同一引用
const obj = { a: 1 };
shallowEqual(obj, obj);  // true

// 2. 相同原始值
shallowEqual(1, 1);      // true
shallowEqual('hello', 'hello');  // true

// 3. 第一层属性相同
shallowEqual(
  { name: 'React', version: 19 },
  { name: 'React', version: 19 }
);  // true

// 4. 嵌套对象是同一引用
const nested = { x: 1 };
shallowEqual(
  { data: nested },
  { data: nested }
);  // true（nested 是同一引用）
```

### ❌ 返回 false 的情况

```javascript
// 1. 不同原始值
shallowEqual(1, 2);  // false

// 2. 键数量不同
shallowEqual(
  { a: 1 },
  { a: 1, b: 2 }
);  // false

// 3. 键相同但值不同
shallowEqual(
  { name: 'React' },
  { name: 'Vue' }
);  // false

// 4. 嵌套对象不同引用（即使内容相同！）
shallowEqual(
  { data: { x: 1 } },
  { data: { x: 1 } }
);  // false ⚠️ 两个 { x: 1 } 是不同引用

// 5. 数组不同引用
shallowEqual(
  { list: [1, 2, 3] },
  { list: [1, 2, 3] }
);  // false ⚠️ 两个数组是不同引用
```

---

## 七、浅比较的局限性

### 核心问题：只比较第一层

```javascript
// 浅比较无法检测深层变化
const prevProps = {
  user: { name: 'Alice', age: 20 }
};

const nextProps = {
  user: { name: 'Alice', age: 21 }  // age 变了！
};

shallowEqual(prevProps, nextProps);  // false ✅ 能检测到
// 原因：prevProps.user !== nextProps.user（不同引用）
```

**但如果直接修改对象（反模式）：**

```javascript
const user = { name: 'Alice', age: 20 };
const prevProps = { user };

// 直接修改（错误做法！）
user.age = 21;
const nextProps = { user };

shallowEqual(prevProps, nextProps);  // true ❌ 检测不到！
// 原因：prevProps.user === nextProps.user（同一引用）
```

---

## 八、PureComponent 中浅比较的应用

### React 内部实现（简化版）

```javascript
// PureComponent 的 shouldComponentUpdate 逻辑
function checkShouldComponentUpdate(
  workInProgress,
  oldProps,
  newProps,
  oldState,
  newState
) {
  const instance = workInProgress.stateNode;

  // 如果是 PureComponent
  if (instance.isPureReactComponent) {
    return (
      !shallowEqual(oldProps, newProps) ||
      !shallowEqual(oldState, newState)
    );
  }

  // 普通 Component，默认返回 true（总是更新）
  return true;
}
```

### Component vs PureComponent 对比

| 特性 | Component | PureComponent |
|------|-----------|---------------|
| shouldComponentUpdate | 默认返回 `true` | 自动浅比较 |
| props 变化检测 | 不检测，总是更新 | 浅比较 props |
| state 变化检测 | 不检测，总是更新 | 浅比较 state |
| 性能 | 可能有不必要的渲染 | 减少不必要的渲染 |

---

## 九、函数式组件中的浅比较

### 1. React.memo - 等价于 PureComponent

```javascript
// 普通函数组件（每次父组件更新都会重新渲染）
function Child({ name, age }) {
  console.log('Child rendered');
  return <div>{name} - {age}</div>;
}

// 使用 React.memo 包裹（自动浅比较 props）
const MemoChild = React.memo(function Child({ name, age }) {
  console.log('MemoChild rendered');
  return <div>{name} - {age}</div>;
});

// 自定义比较函数
const MemoChildCustom = React.memo(Child, (prevProps, nextProps) => {
  // 返回 true 表示相等，不更新
  // 返回 false 表示不等，需要更新
  return prevProps.name === nextProps.name;  // 只比较 name
});
```

### 2. useMemo - 缓存计算结果

```javascript
function Parent({ list }) {
  // ❌ 错误：每次渲染都创建新数组
  const activeItems = list.filter(x => x.active);

  // ✅ 正确：只有 list 变化时才重新计算
  const activeItems = useMemo(() => {
    return list.filter(x => x.active);
  }, [list]);

  // ✅ 缓存对象，保持引用稳定
  const style = useMemo(() => ({
    color: 'red',
    fontSize: 14
  }), []);  // 空依赖，永不变化

  return <MemoChild items={activeItems} style={style} />;
}
```

### 3. useCallback - 缓存函数引用

```javascript
function Parent() {
  const [count, setCount] = useState(0);

  // ❌ 错误：每次渲染都创建新函数
  const handleClick = () => {
    setCount(count + 1);
  };

  // ✅ 正确：函数引用稳定
  const handleClick = useCallback(() => {
    setCount(c => c + 1);
  }, []);  // 空依赖，函数永不变化

  return <MemoChild onClick={handleClick} />;
}
```

### 4. 完整的函数式组件最佳实践

```javascript
import React, { useState, useMemo, useCallback, memo } from 'react';

// 子组件：使用 memo 包裹
const UserCard = memo(function UserCard({ user, onEdit, style }) {
  console.log('UserCard rendered:', user.name);
  return (
    <div style={style}>
      <span>{user.name} - {user.age}</span>
      <button onClick={() => onEdit(user.id)}>Edit</button>
    </div>
  );
});

// 父组件
function UserList({ users }) {
  const [selectedId, setSelectedId] = useState(null);

  // ✅ 缓存样式对象
  const cardStyle = useMemo(() => ({
    padding: 10,
    border: '1px solid #ccc'
  }), []);

  // ✅ 缓存回调函数
  const handleEdit = useCallback((id) => {
    setSelectedId(id);
    console.log('Editing user:', id);
  }, []);

  // ✅ 缓存过滤结果
  const activeUsers = useMemo(() => {
    return users.filter(u => u.active);
  }, [users]);

  return (
    <div>
      {activeUsers.map(user => (
        <UserCard
          key={user.id}
          user={user}
          onEdit={handleEdit}
          style={cardStyle}
        />
      ))}
    </div>
  );
}
```

---

## 十、类组件 vs 函数组件对比

| 功能 | 类组件 | 函数组件 |
|------|--------|----------|
| 浅比较组件 | `PureComponent` | `React.memo()` |
| 缓存计算值 | 手动实现 memoization | `useMemo()` |
| 缓存函数 | 类方法（自动稳定） | `useCallback()` |
| 自定义比较 | 重写 `shouldComponentUpdate` | `React.memo(Comp, compareFn)` |

### 代码对比

```javascript
// 类组件写法
class MyComponent extends PureComponent {
  style = { color: 'red' };

  handleClick = () => {
    this.setState(s => ({ count: s.count + 1 }));
  };

  render() {
    return <Child style={this.style} onClick={this.handleClick} />;
  }
}

// 函数组件写法
const MyComponent = memo(function MyComponent() {
  const style = useMemo(() => ({ color: 'red' }), []);

  const handleClick = useCallback(() => {
    setCount(c => c + 1);
  }, []);

  return <Child style={style} onClick={handleClick} />;
});
```

---

## 十一、最佳实践总结

### ✅ 正确做法

**1. 保持 props 引用稳定**
```javascript
// 类组件：使用类属性
class Parent extends Component {
  handleClick = () => { /* ... */ };
  render() {
    return <PureChild onClick={this.handleClick} />;
  }
}

// 函数组件：使用 useCallback
function Parent() {
  const handleClick = useCallback(() => { /* ... */ }, []);
  return <MemoChild onClick={handleClick} />;
}
```

**2. 创建新对象/数组来触发更新**
```javascript
// 正确：创建新引用
this.setState(prevState => ({
  user: { ...prevState.user, age: 21 }  // 新对象
}));

this.setState(prevState => ({
  list: [...prevState.list, newItem]    // 新数组
}));
```

**3. 使用不可变数据**
```javascript
const newUser = { ...user, age: 21 };
const newList = [...list, item];
```

### ❌ 错误做法

**1. 直接修改对象（Mutation）**
```javascript
// 错误：直接修改，引用不变，PureComponent 检测不到
this.state.user.age = 21;
this.setState({ user: this.state.user });  // ❌ 不会触发更新
```

**2. 在 render 中创建新引用**
```javascript
class Parent extends Component {
  render() {
    return (
      <PureChild
        // ❌ 每次 render 都创建新对象
        style={{ color: 'red' }}

        // ❌ 每次 render 都创建新函数
        onClick={() => this.handleClick()}

        // ❌ 每次 render 都创建新数组
        items={this.state.list.filter(x => x.active)}
      />
    );
  }
}
```

---

## 十二、核心知识点总结

| 概念 | 说明 |
|------|------|
| 浅比较 | 只比较第一层属性，不递归 |
| Object.is | 处理 NaN 和 +0/-0 的严格相等 |
| PureComponent | 类组件自动浅比较 |
| React.memo | 函数组件自动浅比较 |
| useMemo | 缓存计算结果 |
| useCallback | 缓存函数引用 |

### 浅比较局限性

| 问题 | 原因 |
|------|------|
| 检测不到深层变化 | 只比较第一层 |
| 直接修改对象无效 | 引用不变 |
| 新建对象总是不等 | 引用不同 |

---

## 十三、明日任务 (Tomorrow's Tasks)

- Day 10: Ref 机制
- 学习文件: `packages/react/src/ReactCreateRef.js`
- 重点: createRef 的实现，ref 的用途

---

## 十四、参考链接 (References)

- [React 源码 - shallowEqual.js](https://github.com/facebook/react/blob/main/packages/shared/shallowEqual.js)
- [React 源码 - objectIs.js](https://github.com/facebook/react/blob/main/packages/shared/objectIs.js)
- [MDN - Object.is](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is)
