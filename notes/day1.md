# Day 1: React 包入口与导出结构

**日期**: 2026-01-28
**学习目标**: 理解 React 包的整体结构和导出的 API

---

## 学习文件

1. `packages/react/index.js` - React 包入口
2. `packages/react/src/ReactClient.js` - 客户端实现入口
3. `packages/shared/ReactSymbols.js` - Symbol 类型定义

---

## 源码片段与解析

### 1. React 入口文件 (packages/react/index.js)

```javascript
/**
 * @flow
 */

// Flow 类型导出
export type ComponentType<-P> = React$ComponentType<P>;
export type Element<+C> = React$Element<C>;
export type Node = React$Node;
export type Context<T> = React$Context<T>;
// ...

// 所有 API 从 ReactClient 导出
export {
  Children,
  Component,
  Fragment,
  useState,
  useEffect,
  // ... 共 40+ 个 API
} from './src/ReactClient';
```

**要点:**
- `@flow` 表示使用 Flow 类型检查
- 类型中 `+` 表示协变（只读），`-` 表示逆变（只写）
- 所有实现都在 `ReactClient.js` 中

### 2. ReactClient.js 导入结构

```javascript
// 版本号
import ReactVersion from 'shared/ReactVersion';

// Symbol 类型标识
import {
  REACT_FRAGMENT_TYPE,
  REACT_SUSPENSE_TYPE,
  // ...
} from 'shared/ReactSymbols';

// 类组件基类
import {Component, PureComponent} from './ReactBaseClasses';

// JSX 元素操作
import {createElement, cloneElement, isValidElement} from './jsx/ReactJSXElement';

// 所有 Hooks
import {
  useState,
  useEffect,
  useCallback,
  useMemo,
  // ...
} from './ReactHooks';

// 内部共享状态
import ReactSharedInternals from './ReactSharedInternalsClient';
```

**导入来源汇总:**

| 来源文件 | 导入内容 |
|----------|----------|
| `shared/ReactSymbols` | 组件类型 Symbol |
| `./ReactBaseClasses` | Component, PureComponent |
| `./jsx/ReactJSXElement` | createElement, cloneElement |
| `./ReactHooks` | 所有 Hooks |
| `./ReactContext` | createContext |
| `./ReactMemo` | memo |
| `./ReactLazy` | lazy |
| `./ReactForwardRef` | forwardRef |

### 3. Children 工具对象

```javascript
const Children = {
  map,       // 遍历子元素并返回新数组
  forEach,   // 遍历子元素（无返回值）
  count,     // 计算子元素数量
  toArray,   // 将子元素转为数组
  only,      // 验证只有一个子元素并返回它
};
```

### 4. Symbol 类型定义 (packages/shared/ReactSymbols.js)

```javascript
export const REACT_ELEMENT_TYPE: symbol = Symbol.for('react.element');
export const REACT_FRAGMENT_TYPE: symbol = Symbol.for('react.fragment');
export const REACT_STRICT_MODE_TYPE: symbol = Symbol.for('react.strict_mode');
export const REACT_PROFILER_TYPE: symbol = Symbol.for('react.profiler');
export const REACT_SUSPENSE_TYPE: symbol = Symbol.for('react.suspense');
export const REACT_MEMO_TYPE: symbol = Symbol.for('react.memo');
export const REACT_LAZY_TYPE: symbol = Symbol.for('react.lazy');
export const REACT_FORWARD_REF_TYPE: symbol = Symbol.for('react.forward_ref');
export const REACT_CONTEXT_TYPE: symbol = Symbol.for('react.context');
export const REACT_PROVIDER_TYPE: symbol = Symbol.for('react.provider');
export const REACT_CONSUMER_TYPE: symbol = Symbol.for('react.consumer');
```

**为什么使用 Symbol.for() 而不是 Symbol():**
1. 全局注册表 - 相同 key 返回同一个 Symbol
2. 跨 iframe/worker 共享
3. 安全性 - 防止 XSS 攻击伪造 React 元素

### 5. 迭代器函数

```javascript
export function getIteratorFn(maybeIterable: ?any): ?() => ?Iterator<any> {
  if (maybeIterable === null || typeof maybeIterable !== 'object') {
    return null;
  }
  const maybeIterator =
    (MAYBE_ITERATOR_SYMBOL && maybeIterable[MAYBE_ITERATOR_SYMBOL]) ||
    maybeIterable[FAUX_ITERATOR_SYMBOL];
  if (typeof maybeIterator === 'function') {
    return maybeIterator;
  }
  return null;
}
```

**作用:** 获取对象的迭代器函数，用于遍历 children。

---

## 关键概念

### 1. Flow 类型系统
- React 源码使用 Flow 而非 TypeScript
- `@flow` 注解启用类型检查
- 协变 `+` / 逆变 `-` 控制类型参数的读写

### 2. React 导出的 API 分类

| 类别 | API |
|------|-----|
| 组件基类 | Component, PureComponent |
| 内置组件 | Fragment, StrictMode, Suspense, Profiler |
| 元素操作 | createElement, cloneElement, isValidElement, createRef |
| Hooks (16个) | useState, useEffect, useContext, useReducer, useCallback, useMemo, useRef, useLayoutEffect, useInsertionEffect, useImperativeHandle, useDebugValue, useId, useDeferredValue, useTransition, useSyncExternalStore, useOptimistic |
| React 19 新增 | use, useActionState, cache |
| 高阶组件 | memo, forwardRef, lazy |
| Context | createContext |
| 并发 | startTransition |

### 3. Symbol 的安全性作用
- `$$typeof` 字段使用 Symbol 标识 React 元素
- 防止 JSON 注入攻击（JSON 无法包含 Symbol）
- 确保只有 React 创建的元素才能被渲染

### 4. API 稳定性标记
- 无前缀: 稳定 API
- `experimental_`: 实验性 API
- `unstable_`: 不稳定 API

---

## 问答记录

**Q1: 为什么 Fragment、Suspense 等组件只是 Symbol 而不是函数？**

A: 这些"组件"实际上只是类型标识符。React 在渲染时通过检查元素的 `type` 属性是否等于这些 Symbol 来进行特殊处理。例如：
```javascript
// <Fragment> 编译后
{ $$typeof: Symbol.for('react.element'), type: Symbol.for('react.fragment'), ... }
```
React reconciler 看到 `type === REACT_FRAGMENT_TYPE` 时，会特殊处理其子元素。

**Q2: 所有 Hooks 都来自哪里？**

A: 所有 Hooks 都从 `./ReactHooks.js` 导入。但 `ReactHooks.js` 只是一个代理层，真正的实现在 `react-reconciler/src/ReactFiberHooks.js` 中。

**Q3: `__CLIENT_INTERNALS_DO_NOT_USE_OR_WARN_USERS_THEY_CANNOT_UPGRADE` 是什么？**

A: 这是 React 的内部共享状态对象（ReactSharedInternals），包含当前的 Hooks dispatcher 等信息。名字故意很长是为了警告开发者不要使用它。

---

## 文件依赖关系图

```
packages/react/index.js
        │
        ▼
packages/react/src/ReactClient.js
        │
        ├──→ shared/ReactSymbols.js (Symbol 定义)
        ├──→ shared/ReactVersion.js (版本号)
        ├──→ ./ReactBaseClasses.js (Component, PureComponent)
        ├──→ ./ReactHooks.js (所有 Hooks)
        ├──→ ./jsx/ReactJSXElement.js (createElement)
        ├──→ ./ReactContext.js (createContext)
        ├──→ ./ReactMemo.js (memo)
        ├──→ ./ReactLazy.js (lazy)
        └──→ ./ReactForwardRef.js (forwardRef)
```

---

## 下一步学习

Day 2 将学习 **ReactElement 与 JSX 转换**:
- 文件: `packages/react/src/jsx/ReactJSXElement.js`
- 目标: 理解 JSX 如何转换为 ReactElement
