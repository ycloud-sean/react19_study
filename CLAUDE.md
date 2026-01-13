# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

---

# React 19 源码学习指南 (React 19 Source Code Learning Guide)

## 学习目标 (Learning Objectives)

This is the **React 19** source code repository. This guide is designed for junior developers who want to understand React's internals to improve their practical React development skills.

这是**React 19**的源码仓库。本指南专为想要理解React内部机制以提升React实践开发能力的初级开发者设计。

## Repository Structure (仓库结构)

This is the **React** repository, containing both the core React library and the React Compiler (an optimization compiler for React apps).

### Main Components

- **`packages/`** - Core React packages:
  - `react` - Core React API (components, hooks, JSX)
  - `react-dom` - DOM rendering and browser APIs
  - `react-reconciler` - The reconciliation algorithm (heart of React's rendering)
  - `react-server` - Server-side rendering components
  - `shared` - Shared utilities used across packages
  - Various other packages for specific use cases (devtools, test-utils, etc.)

- **`compiler/`** - React Compiler (optimization compiler):
  - `babel-plugin-react-compiler` - Babel plugin for compilation
  - `react-compiler-runtime` - Runtime support for compiled code
  - `eslint-plugin-react-compiler` - Linting rules
  - `snap` - Custom test runner with golden file snapshots

- **`scripts/`** - Build and development scripts:
  - `rollup/` - Build system using Rollup
  - `jest/` - Jest configuration and custom test runner
  - `eslint/` - Linting rules

## Learning Path (学习路线)

建议学习时间：30分钟/天 (30 minutes/day recommended)

### 阶段一：基础理解阶段 (Phase 1: Foundation - Days 1-30)

**目标：建立对React整体架构的初步认知**
**Goal: Build initial understanding of React's overall architecture**

### Day 1: React入口文件 (React Entry Point)
**学习文件 (Files to Study):**
- `packages/react/index.js`

**任务 (Tasks):**
1. 了解React导出的主要API
2. 记录Component, Hook, Element等核心概念

**笔记要点 (Notes):**
- React主要导出哪些API？
- 这些API的作用是什么？

### Day 2: ReactDOM入口文件 (ReactDOM Entry Point)
**学习文件 (Files to Study):**
- `packages/react-dom/index.js`

**任务 (Tasks):**
1. 了解ReactDOM导出的API
2. 理解客户端和服务端的区别

**笔记要点 (Notes):**
- ReactDOM主要功能是什么？
- createPortal的作用？

### Day 3: React客户端实现 (React Client Implementation)
**学习文件 (Files to Study):**
- `packages/react/src/ReactClient.js` (第1-30行)

**任务 (Tasks):**
1. 理解ReactClient的结构
2. 了解主要模块的导入

**笔记要点 (Notes):**
- ReactClient导入哪些模块？
- 每个模块的作用？

### Day 4: React版本信息与构建调试入门 (React Version & Build Debugging Basics)
**学习文件 (Files to Study):**
- `packages/react/src/ReactClient.js` (第31-77行)
- `scripts/rollup/build.js` (前50行)

**任务 (Tasks):**
1. 学习export语句的写法
2. 理解React API的分组
3. **了解React构建流程**
4. **学习构建错误的调试基础**

**实践 (Practice):**
```bash
# 尝试构建React
yarn build

# 观察构建输出
ls -la build/

# 理解构建错误
# 如果有错误，尝试解读错误信息
```

**笔记要点 (Notes):**
- React如何组织export？
- 哪些是开发环境专用？
- React如何构建和打包？
- 构建错误的常见类型？
- source map的作用？

### Day 5: JSX基础 (JSX Fundamentals)
**学习文件 (Files to Study):**
- `packages/react/src/jsx/ReactJSXElement.js` (第1-50行)

**任务 (Tasks):**
1. 理解createElement函数
2. 了解Element的基本结构

**笔记要点 (Notes):**
- createElement的参数有哪些？
- Element对象包含什么属性？

### Day 6: createElement实现 (createElement Implementation)
**学习文件 (Files to Study):**
- `packages/react/src/jsx/ReactJSXElement.js` (第51-100行)

**任务 (Tasks):**
1. 深入学习createElement逻辑
2. 理解key和ref的处理

**笔记要点 (Notes):**
- 如何处理key和ref？
- props是如何解析的？

### Day 7: React元素类型 (React Element Types)
**学习文件 (Files to Study):**
- `packages/react/src/jsx/ReactJSXElement.js` (第101-150行)

**任务 (Tasks):**
1. 理解不同类型元素的处理
2. 学习Fragment的实现

**笔记要点 (Notes):**
- React如何区分元素类型？
- Fragment vs 普通元素？

### Day 8: 组件基类 (Component Base Class)
**学习文件 (Files to Study):**
- `packages/react/src/ReactBaseClasses.js` (第1-50行)

**任务 (Tasks):**
1. 学习Component类的定义
2. 理解setState的基础

**笔记要点 (Notes):**
- Component类如何工作？
- setState的基本用法？

### Day 9: PureComponent实现 (PureComponent Implementation)
**学习文件 (Files to Study):**
- `packages/react/src/ReactBaseClasses.js` (第51-100行)

**任务 (Tasks):**
1. 对比Component和PureComponent
2. 理解浅比较的含义

**笔记要点 (Notes):**
- PureComponent的优势？
- 浅比较的局限性？

### Day 10: Ref机制 (Ref Mechanism)
**学习文件 (Files to Study):**
- `packages/react/src/ReactCreateRef.js`

**任务 (Tasks):**
1. 学习createRef的实现
2. 理解ref的用途

**笔记要点 (Notes):**
- createRef如何创建ref？
- ref在什么时候使用？

### Day 11: ForwardRef使用 (ForwardRef Usage)
**学习文件 (Files to Study):**
- `packages/react/src/ReactForwardRef.js` (第1-50行)

**任务 (Tasks):**
1. 理解forwardRef的作用
2. 学习ref转发模式

**笔记要点 (Notes):**
- 为什么要使用forwardRef？
- ref转发解决了什么问题？

### Day 12: ForwardRef实现 (ForwardRef Implementation)
**学习文件 (Files to Study):**
- `packages/react/src/ReactForwardRef.js` (第51-80行)

**任务 (Tasks):**
1. 学习forwardRef的具体实现
2. 理解renderProps模式

**笔记要点 (Notes):**
- forwardRef如何处理props？
- 如何在函数组件中暴露ref？

### Day 13: Hooks概述 (Hooks Overview)
**学习文件 (Files to Study):**
- `packages/react/src/ReactHooks.js` (第1-50行)

**任务 (Tasks):**
1. 了解Hooks的基本概念
2. 学习useState的API结构

**笔记要点 (Notes):**
- Hooks解决了什么问题？
- useState的参数和返回值？

### Day 14: useState基础 (useState Basics)
**学习文件 (Files to Study):**
- `packages/react/src/ReactHooks.js` (第51-100行)

**任务 (Tasks):**
1. 学习useState的实现思路
2. 理解状态管理的概念

**笔记要点 (Notes):**
- useState如何管理状态？
- 函数式更新是什么？

### Day 15: useEffect基础 (useEffect Basics)
**学习文件 (Files to Study):**
- `packages/react/src/ReactHooks.js` (第101-150行)

**任务 (Tasks):**
1. 理解useEffect的作用
2. 学习副作用的概念

**笔记要点 (Notes):**
- useEffect的参数？
- 依赖数组的作用？

### Day 16: useEffect清理 (useEffect Cleanup)
**学习文件 (Files to Study):**
- `packages/react/src/ReactHooks.js` (第151-200行)

**任务 (Tasks):**
1. 学习cleanup函数
2. 理解组件卸载时的清理

**笔记要点 (Notes):**
- cleanup何时执行？
- 如何避免内存泄漏？

### Day 17: useContext基础 (useContext Basics)
**学习文件 (Files to Study):**
- `packages/react/src/ReactContext.js` (第1-50行)

**任务 (Tasks):**
1. 学习Context的基本用法
2. 理解全局状态管理

**笔记要点 (Notes):**
- createContext如何工作？
- Context解决了什么问题？

### Day 18: Provider实现 (Provider Implementation)
**学习文件 (Files to Study):**
- `packages/react/src/ReactContext.js` (第51-100行)

**任务 (Tasks):**
1. 理解Provider的工作原理
2. 学习值的传递机制

**笔记要点 (Notes):**
- Provider如何传递值？
- 多个Provider如何嵌套？

### Day 19: Consumer使用 (Consumer Usage)
**学习文件 (Files to Study):**
- `packages/react/src/ReactContext.js` (第101-120行)

**任务 (Tasks):**
1. 学习Consumer模式
2. 理解Context的订阅机制

**笔记要点 (Notes):**
- Consumer如何获取值？
- 什么时候使用Context？

### Day 20: memo基础 (memo Basics)
**学习文件 (Files to Study):**
- `packages/react/src/ReactMemo.js` (第1-40行)

**任务 (Tasks):**
1. 理解React.memo的作用
2. 学习组件记忆化

**笔记要点 (Notes):**
- React.memo如何优化渲染？
- 什么时候使用memo？

### Day 21: memo实现 (memo Implementation)
**学习文件 (Files to Study):**
- `packages/react/src/ReactMemo.js` (第41-60行)

**任务 (Tasks):**
1. 学习memo的内部实现
2. 理解比较函数的使用

**笔记要点 (Notes):**
- memo如何比较props？
- 自定义比较函数怎么写？

### Day 22: lazy加载 (lazy Loading)
**学习文件 (Files to Study):**
- `packages/react/src/ReactLazy.js` (第1-50行)

**任务 (Tasks):**
1. 理解代码分割概念
2. 学习懒加载的基础

**笔记要点 (Notes):**
- lazy如何实现懒加载？
- Suspense如何配合使用？

### Day 23: Suspense机制 (Suspense Mechanism)
**学习文件 (Files to Study):**
- `packages/shared/ReactSymbols.js` (第1-30行)

**任务 (Tasks):**
1. 学习Suspense类型标识
2. 理解挂起和恢复

**笔记要点 (Notes):**
- Suspense如何工作？
- 什么是异步渲染？

### Day 24: Fragment使用 (Fragment Usage)
**学习文件 (Files to Study):**
- `packages/react/src/ReactBaseClasses.js` (第101-120行)

**任务 (Tasks):**
1. 理解Fragment的用途
2. 学习短语法<> </>

**笔记要点 (Notes):**
- 为什么要使用Fragment？
- Fragment vs div？

### Day 25: Children操作 (Children Manipulation)
**学习文件 (Files to Study):**
- `packages/react/src/ReactChildren.js` (第1-50行)

**任务 (Tasks):**
1. 学习Children API
2. 理解ReactChildren工具

**笔记要点 (Notes):**
- Children.map如何使用？
- 处理嵌套Children？

### Day 26: Children高级操作 (Children Advanced)
**学习文件 (Files to Study):**
- `packages/react/src/ReactChildren.js` (第51-100行)

**任务 (Tasks):**
1. 深入理解Children.forEach
2. 学习Children.count和toArray

**笔记要点 (Notes):**
- 各种Children方法适用场景？
- 如何转换Children数组？

### Day 27: cloneElement用法 (cloneElement Usage)
**学习文件 (Files to Study):**
- `packages/react/src/ReactChildren.js` (第101-130行)

**任务 (Tasks):**
1. 理解cloneElement的作用
2. 学习元素修改和扩展

**笔记要点 (Notes):**
- cloneElement如何工作？
- 实际应用场景？

### Day 28: isValidElement检查 (isValidElement Validation)
**学习文件 (Files to Study):**
- `packages/react/src/ReactChildren.js` (第131-140行)

**任务 (Tasks):**
1. 学习元素验证
2. 理解类型检查机制

**笔记要点 (Notes):**
- isValidElement如何判断？
- 为什么需要验证？

### Day 29: only函数理解 (only Function)
**学习文件 (Files to Study):**
- `packages/react/src/ReactChildren.js` (第141-150行)

**任务 (Tasks):**
1. 理解only函数的限制
2. 学习单个子元素验证

**笔记要点 (Notes):**
- only函数的作用？
- 什么时候需要确保单个子元素？

### Day 30: 阶段一总结与调试入门 (Phase 1 Summary and Debugging Basics)
**学习任务 (Study Tasks):**
1. 回顾阶段一的所有概念
2. 整理API之间的关系
3. 学习基础调试方法

**任务 (Tasks):**
1. 学会在React中添加调试日志
2. 理解console.log的最佳实践
3. 学习如何阅读错误信息

**实践 (Practice):**
```bash
# 尝试在React源码中添加简单的调试日志
# 运行测试观察输出
yarn test -- --testPathPattern="React" --verbose

# 观察错误信息的格式和内容
```

**笔记要点 (Notes):**
- 核心概念总结
- API之间的联系
- 调试日志的添加时机
- 准备进入阶段二

### 阶段二：深入核心 (Phase 2: Deep Dive - Days 31-75)

**目标：理解React的核心算法**
**Goal: Understand React's core algorithms**

### Day 31: Fiber介绍 (Fiber Introduction)
**学习文件 (Files to Study):**
- `packages/react-reconciler/src/ReactFiber.js` (第1-50行)

**任务 (Tasks):**
1. 理解什么是Fiber
2. 学习Fiber的基本概念

**笔记要点 (Notes):**
- Fiber是什么？
- 为什么需要Fiber？

### Day 32: Fiber结构 (Fiber Structure)
**学习文件 (Files to Study):**
- `packages/react-reconciler/src/ReactFiber.js` (第51-100行)

**任务 (Tasks):**
1. 学习Fiber节点的属性
2. 理解双缓冲技术

**笔记要点 (Notes):**
- Fiber包含哪些属性？
- current vs workInProgress？

### Day 33: Fiber类型 (Fiber Types)
**学习文件 (Files to Study):**
- `packages/react-reconciler/src/ReactFiber.js` (第101-150行)

**任务 (Tasks):**
1. 理解不同类型的Fiber
2. 学习HostComponent, FunctionComponent等

**笔记要点 (Notes):**
- FunctionComponent Fiber vs HostComponent Fiber？
- 每种类型的特点？

### Day 34: Fiber状态 (Fiber State)
**学习文件 (Files to Study):**
- `packages/react-reconciler/src/ReactFiber.js` (第151-200行)

**任务 (Tasks):**
1. 学习Fiber的状态管理
2. 理解pendingProps和memoizedProps

**笔记要点 (Notes):**
- props如何变化？
- 状态如何影响渲染？

### Day 35: 协调算法概述 (Reconciliation Overview)
**学习文件 (Files to Study):**
- `packages/react-reconciler/src/ReactChildFiber.js` (第1-50行)

**任务 (Tasks):**
1. 理解协调的基本概念
2. 学习diff算法

**笔记要点 (Notes):**
- 什么是协调？
- diff算法的优势？

### Day 36: 元素比较 (Element Comparison)
**学习文件 (Files to Study):**
- `packages/react-reconciler/src/ReactChildFiber.js` (第51-100行)

**任务 (Tasks):**
1. 学习元素比较规则
2. 理解key的作用

**笔记要点 (Notes):**
- 如何判断元素相同？
- key如何优化列表渲染？

### Day 37: 子元素协调 (Children Reconciliation)
**学习文件 (Files to Study):**
- `packages/react-reconciler/src/ReactChildFiber.js` (第101-150行)

**任务 (Tasks):**
1. 理解多子元素的处理
2. 学习数组和迭代器的处理

**笔记要点 (Notes):**
- 如何处理多个子元素？
- keys的生成策略？

### Day 38: 渲染阶段开始 (Render Phase Begin)
**学习文件 (Files to Study):**
- `packages/react-reconciler/src/ReactFiberBeginWork.js` (第1-50行)

**任务 (Tasks):**
1. 学习渲染阶段的开始
2. 理解beginWork函数

**笔记要点 (Notes):**
- beginWork的作用？
- 渲染阶段做什么？

### Day 39: 函数组件渲染 (Function Component Render)
**学习文件 (Files to Study):**
- `packages/react-reconciler/src/ReactFiberBeginWork.js` (第51-100行)

**任务 (Tasks):**
1. 理解函数组件的渲染
2. 学习Hooks的调用

**笔记要点 (Notes):**
- 函数组件如何渲染？
- Hooks在渲染时如何工作？

### Day 40: 类组件渲染 (Class Component Render)
**学习文件 (Files to Study):**
- `packages/react-reconciler/src/ReactFiberBeginWork.js` (第101-150行)

**任务 (Tasks):**
1. 学习类组件的渲染
2. 理解生命周期调用

**笔记要点 (Notes):**
- 类组件如何渲染？
- 生命周期方法何时调用？

### Day 41: 状态更新机制 (State Update Mechanism)
**学习文件 (Files to Study):**
- `packages/react-reconciler/src/ReactFiberClassUpdateQueue.js` (第1-50行)

**任务 (Tasks):**
1. 理解状态更新的触发
2. 学习updateQueue

**笔记要点 (Notes):**
- setState如何触发更新？
- updateQueue的作用？

### Day 42: 批处理更新 (Batch Updates)
**学习文件 (Files to Study):**
- `packages/react-reconciler/src/ReactFiberClassUpdateQueue.js` (第51-100行)

**任务 (Tasks):**
1. 学习批处理的概念
2. 理解多次setState的合并

**笔记要点 (Notes):**
- 什么是批处理？
- 为什么需要合并更新？

### Day 43: Hooks状态管理 (Hooks State Management)
**学习文件 (Files to Study):**
- `packages/react-reconciler/src/ReactFiberHooks.js` (第1-50行)

**任务 (Tasks):**
1. 理解Hooks如何存储状态
2. 学习workInProgressHook

**笔记要点 (Notes):**
- Hooks状态如何存储？
- 为什么要用链表？

### Day 44: useState实现 (useState Implementation)
**学习文件 (Files to Study):**
- `packages/react-reconciler/src/ReactFiberHooks.js` (第51-100行)

**任务 (Tasks):**
1. 深入学习useState的实现
2. 理解dispatchAction

**笔记要点 (Notes):**
- dispatchAction如何工作？
- 状态更新如何触发渲染？

### Day 45: useEffect实现 (useEffect Implementation)
**学习文件 (Files to Study):**
- `packages/react-reconciler/src/ReactFiberHooks.js` (第101-150行)

**任务 (Tasks):**
1. 理解useEffect的调度
2. 学习effectList

**笔记要点 (Notes):**
- useEffect如何被调度？
- 渲染阶段vs提交阶段？

### Day 46: 阶段二调试实践 (Phase 2 Debugging Practice)
**学习文件 (Files to Study):**
- `packages/react-reconciler/src/ReactFiberBeginWork.js`

**任务 (Tasks):**
1. 学习渲染阶段的调试方法
2. 理解beginWork和completeWork的调试
3. 学习如何追踪Fiber树的变化

**实践 (Practice):**
```bash
# 在渲染阶段添加调试日志
console.log('beginWork:', fiber);
console.log('completeWork:', fiber);

# 运行测试观察渲染流程
yarn test -- --testPathPattern="ReactFiber" --verbose
```

**笔记要点 (Notes):**
- 渲染阶段的调试重点？
- 如何识别渲染性能问题？

### Day 47: 提交阶段调试 (Commit Phase Debugging)
**学习文件 (Files to Study):**
- `packages/react-reconciler/src/ReactFiberCommitWork.js`

**任务 (Tasks):**
1. 学习DOM更新的调试方法
2. 理解副作用的调试
3. 学习ref更新的追踪

**实践 (Practice):**
```bash
# 在DOM操作中添加日志
console.log('Inserting DOM:', container);
console.log('Updating DOM:', element);
console.log('Removing DOM:', child);
```

**笔记要点 (Notes):**
- DOM更新的调试重点？
- 如何验证DOM操作的正确性？

### Day 48-75: 更多核心概念 (Additional Core Concepts)
**学习任务 (Study Tasks):**
*继续深入学习其他重要文件...*

### 阶段三：高级主题 (Phase 3: Advanced Topics - Days 76-120)

**目标：掌握React的高级特性和优化**
**Goal: Master React's advanced features and optimizations**

### Day 76: 并发渲染调试 (Concurrent Rendering Debugging)
**学习文件 (Files to Study):**
- `packages/react/src/ReactStartTransition.js`
- `packages/react-reconciler/src/ReactFiberConcurrentUpdates.js`

**任务 (Tasks):**
1. 学习并发特性的调试方法
2. 理解优先级调度的调试
3. 学习Suspense的调试技巧

**实践 (Practice):**
```bash
# 调试transition
console.log('Transition priority:', priority);
console.log('Concurrent update:', update);

# 调试suspense
console.log('Suspense threshold:', threshold);
console.log('Promise thrown:', promise);
```

**笔记要点 (Notes):**
- 并发渲染的调试特点？
- 如何验证优先级调度的正确性？

### Day 77: Server Components调试 (Server Components Debugging)
**学习文件 (Files to Study):**
- `packages/react-server/src/ReactServerRoot.js`

**任务 (Tasks):**
1. 学习SSR vs RSC的调试方法
2. 理解流式渲染的调试
3. 学习数据获取的调试

**实践 (Practice):**
```bash
# 调试服务器渲染
console.log('Server render started');
console.log('Streaming response:', response);

# 调试客户端合并
console.log('Merging server data:', data);
```

**笔记要点 (Notes):**
- 服务端渲染的调试重点？
- 如何验证数据流？

### Day 78: Server Components调试与构建调试 (Server Components & Build Debugging)
**学习文件 (Files to Study):**
- `packages/react-server/src/ReactServerRoot.js`
- `scripts/rollup/build.js`
- `scripts/rollup/bundles.js`

**任务 (Tasks):**
1. 学习SSR vs RSC的调试方法
2. 理解流式渲染的调试
3. 学习数据获取的调试
4. **掌握构建错误的调试**
5. **理解bundle的分析技巧**
6. **掌握source map的使用**

**实践 (Practice):**
```bash
# 调试服务器渲染
console.log('Server render started');
console.log('Streaming response:', response);

# 调试客户端合并
console.log('Merging server data:', data);

# 构建调试
yarn lint-build
yarn build --type=NODE_DEV react/index,react-dom/index
du -sh build/
```

**笔记要点 (Notes):**
- 服务端渲染的调试重点？
- 如何验证数据流？
- 构建错误的常见原因？
- 如何优化构建过程？
- source map的作用？

### Day 79-120: 高级特性与优化 (Advanced Features and Optimization)
**学习任务 (Study Tasks):**
*每天专注一个高级特性...*

### 阶段四：实践应用 (Phase 4: Practical Application - Days 121-150)

**目标：结合源码知识解决实际问题**
**Goal: Apply source code knowledge to solve practical problems**

### Day 121: 本地调试环境设置 (Local Debugging Environment Setup)
**学习文件 (Files to Study):**
- `packages/react/src/ReactClient.js` (添加调试日志)
- `packages/react/src/ReactHooks.js` (添加调试日志)

**任务 (Tasks):**
1. 学习如何在源码中添加console.log
2. 理解调试日志的时机和位置
3. 设置断点调试

**实践 (Practice):**
```bash
# 在源码中添加调试日志
console.log('Debug point:', value);

# 重新构建
yarn build

# 运行测试观察输出
yarn test --testPathPattern="ReactClient" --verbose
```

**笔记要点 (Notes):**
- 在哪里添加调试日志最有效？
- 如何避免调试代码影响性能？

### Day 122: 测试调试技巧 (Test Debugging Skills)
**学习文件 (Files to Study):**
- `packages/react/__tests__/` (选择1-2个测试文件)

**任务 (Tasks):**
1. 学习如何运行单个测试
2. 理解测试的输出和错误信息
3. 使用debug模式运行测试

**实践 (Practice):**
```bash
# 运行单个测试文件
yarn test -- --testPathPattern="ReactJSXElement"

# 使用debug模式
yarn test --debug

# 详细输出模式
yarn test -- --testNamePattern="useState" --verbose
```

**笔记要点 (Notes):**
- 如何选择合适的测试用例？
- 错误信息如何解读？

### Day 123: React DevTools使用 (React DevTools Usage)
**学习文件 (Files to Study):**
- `packages/react-devtools-shared/src/`

**任务 (Tasks):**
1. 学习React DevTools的安装和使用
2. 理解Component面板和Profiler面板
3. 学习如何检查Hook的状态

**实践 (Practice):**
```bash
# 构建devtools版本
yarn build-for-devtools

# 打开浏览器扩展
open chrome://extensions

# 加载React DevTools扩展
# 使用DevTools检查React应用
```

**笔记要点 (Notes):**
- DevTools的哪些功能最有用？
- 如何利用Profiler优化性能？

### Day 124: 源码修改实验 (Source Code Modification Experiment)
**学习文件 (Files to Study):**
- `packages/react/src/ReactBaseClasses.js`

**任务 (Tasks):**
1. 修改Component类的实现
2. 观察修改后的效果
3. 学习撤销修改的方法

**实践 (Practice):**
```bash
# 1. 修改源码文件
# 2. 重新构建
yarn build

# 3. 在example项目中测试
cd fixtures/dom
yarn test

# 4. 撤销修改
git checkout -- packages/react/src/
```

**笔记要点 (Notes):**
- 如何安全地修改源码？
- 修改后的测试如何进行？

### Day 125: 错误边界调试 (Error Boundary Debugging)
**学习文件 (Files to Study):**
- `packages/react/src/ReactBaseClasses.js` (componentDidCatch)

**任务 (Tasks):**
1. 学习错误边界的实现
2. 理解错误捕获和显示机制
3. 学习如何定位组件错误

**实践 (Practice):**
```bash
# 创建一个包含错误的组件
# 使用ErrorBoundary捕获错误
# 观察错误信息
```

**笔记要点 (Notes):**
- 错误边界如何工作？
- 常见的组件错误有哪些？

### Day 126: Hook调试技巧 (Hook Debugging Techniques)
**学习文件 (Files to Study):**
- `packages/react/src/ReactHooks.js`
- `packages/react-reconciler/src/ReactFiberHooks.js`

**任务 (Tasks):**
1. 学习useState和useEffect的调试方法
2. 理解依赖数组的调试
3. 学习Hook调用顺序的验证

**实践 (Practice):**
```bash
# 在Hook中添加调试日志
console.log('useState called with:', initialState);
console.log('useEffect dependencies:', deps);

# 观察组件渲染次数
console.log('Component rendered');
```

**笔记要点 (Notes):**
- 如何验证Hook的正确使用？
- 依赖数组常见问题？

### Day 127: Fiber调试方法 (Fiber Debugging Methods)
**学习文件 (Files to Study):**
- `packages/react-reconciler/src/ReactFiber.js`

**任务 (Tasks):**
1. 学习Fiber节点的可视化
2. 理解Fiber的调试工具
3. 学习如何追踪渲染过程

**实践 (Practice):**
```bash
# 启用Fiber调试模式
# 在React Fiber中添加日志
console.log('Fiber created:', fiber);
console.log('Fiber updated:', fiber);

# 使用React DevTools检查Fiber树
```

**笔记要点 (Notes):**
- Fiber结构如何理解？
- 如何识别性能问题？

### Day 128: 性能调试入门 (Performance Debugging Basics)
**学习文件 (Files to Study):**
- `packages/react/src/ReactMemo.js`
- `packages/react/src/ReactStartTransition.js`

**任务 (Tasks):**
1. 学习如何使用React Profiler
2. 理解渲染性能分析
3. 学习识别不必要的重渲染

**实践 (Practice):**
```bash
# 使用React DevTools Profiler
# 录制渲染过程
# 分析组件渲染时间
# 识别渲染瓶颈
```

**笔记要点 (Notes):**
- 性能问题的常见原因？
- 如何优化组件渲染？

### Day 129: 内存泄漏调试 (Memory Leak Debugging)
**学习文件 (Files to Study):**
- `packages/react/src/ReactHooks.js` (useEffect cleanup)
- `packages/react-reconciler/src/ReactFiberCommitWork.js`

**任务 (Tasks):**
1. 学习useEffect的清理机制
2. 理解内存泄漏的常见原因
3. 学习如何使用浏览器内存工具

**实践 (Practice):**
```bash
# 在useEffect中添加cleanup
useEffect(() => {
  const timer = setInterval(() => {}, 1000);
  return () => clearInterval(timer);
}, []);

# 使用Chrome DevTools Memory面板
# 检测内存泄漏
```

**笔记要点 (Notes):**
- 内存泄漏的典型场景？
- 如何预防内存泄漏？

### Day 130: 状态管理调试 (State Management Debugging)
**学习文件 (Files to Study):**
- `packages/react/src/ReactHooks.js` (useState, useReducer)
- `packages/react/src/ReactContext.js`

**任务 (Tasks):**
1. 学习Context的调试方法
2. 理解复杂状态的追踪
3. 学习状态更新的调试技巧

**实践 (Practice):**
```bash
# 在Context中添加调试日志
const value = useContext(MyContext);
console.log('Context value:', value);

# 追踪状态更新
console.log('State updated:', newState);
```

**笔记要点 (Notes):**
- 状态更新的追踪方法？
- Context的性能考虑？

### Day 131: 事件系统调试 (Event System Debugging)
**学习文件 (Files to Study):**
- `packages/react-dom/src/events/`

**任务 (Tasks):**
1. 学习React事件系统的调试
2. 理解合成事件的处理机制
3. 学习事件委托的调试方法

**实践 (Practice):**
```bash
# 添加事件监听器调试
<div onClick={(e) => {
  console.log('Click event:', e);
  console.log('Synthetic event:', e instanceof SyntheticEvent);
}} />

# 观察事件冒泡和捕获
```

**笔记要点 (Notes):**
- 合成事件 vs 原生事件？
- 事件委托的优势？

### Day 132: 异步代码调试 (Asynchronous Code Debugging)
**学习文件 (Files to Study):**
- `packages/react/src/ReactStartTransition.js`
- `packages/react-reconciler/src/ReactFiberConcurrentUpdates.js`

**任务 (Tasks):**
1. 学习useTransition的调试方法
2. 理解并发渲染的调试
3. 学习异步更新的追踪

**实践 (Practice):**
```javascript
// 调试transition
const [isPending, startTransition] = useTransition();
startTransition(() => {
  console.log('Transition started');
  setState(newValue);
});

// 调试suspense
<Suspense fallback={<div>Loading...</div>}>
  <AsyncComponent />
</Suspense>
```

**笔记要点 (Notes):**
- 并发渲染的调试特点？
- Suspense的错误处理？

### Day 134: 类型错误调试 (Type Error Debugging)
**学习文件 (Files to Study):**
- Flow配置文件
- `packages/react/src/ReactBaseClasses.js` (Flow类型注解)

**任务 (Tasks):**
1. 学习Flow类型检查
2. 理解类型错误的调试方法
3. 学习如何忽略或修复Flow错误

**实践 (Practice):**
```bash
# 运行Flow类型检查
yarn flow

# 检查特定文件
yarn flow check packages/react/src/ReactClient.js

# CI版本的Flow检查
yarn flow-ci
```

**笔记要点 (Notes):**
- Flow类型错误常见类型？
- 如何编写Flow类型注解？

### Day 135: 源码贡献入门 (Source Contribution Basics)
**学习文件 (Files to Study):**
- GitHub上的React仓库issue列表

**任务 (Tasks):**
1. 学习如何阅读issue
2. 理解PR的提交流程
3. 学习测试用例的编写

**实践 (Practice):**
```bash
# 克隆仓库
git clone https://github.com/facebook/react.git
cd react

# 创建分支
git checkout -b fix-issue-123

# 查看issue
# https://github.com/facebook/react/issues

# 运行相关测试
yarn test -- --testPathPattern="相关测试"
```

**笔记要点 (Notes):**
- 如何选择合适的issue？
- PR的质量要求？

### Day 136: 测试用例编写 (Test Case Writing)
**学习文件 (Files to Study):**
- `packages/react/__tests__/React-test.js`
- Jest测试框架文档

**任务 (Tasks):**
1. 学习React测试的写法
2. 理解Jest的断言方法
3. 学习mock的使用

**实践 (Practice):**
```bash
# 运行现有测试
yarn test -- --testPathPattern="React"

# 查看测试文件
ls packages/react/__tests__/

# 编写简单测试用例
# 运行新测试
yarn test -- --testNamePattern="新测试"
```

**笔记要点 (Notes):**
- 好的测试用例的标准？
- 如何mock React组件？

### Day 137: Bug修复实践 (Bug Fix Practice)
**学习文件 (Files to Study):**
- 选择一个已知的简单bug进行修复

**任务 (Tasks):**
1. 学习bug复现的方法
2. 理解bug定位的技巧
3. 学习修复验证的流程

**实践 (Practice):**
```bash
# 复现bug
# 定位问题源码
# 修复bug
# 运行测试验证
git commit -m "Fix: [bug描述]"
```

**笔记要点 (Notes):**
- Bug定位的常用方法？
- 如何确保修复的正确性？

### Day 138: Code Review实践 (Code Review Practice)
**学习文件 (Files to Study):**
- `packages/react/src/ReactHooks.js` (Review现有代码)

**任务 (Tasks):**
1. 学习代码审查的标准
2. 理解React的编码规范
3. 学习如何提供有价值的反馈

**实践 (Practice):**
```bash
# 审查代码质量
yarn lint

# 检查代码风格
yarn prettier-check

# 运行类型检查
yarn flow

# 提出改进建议
```

**笔记要点 (Notes):**
- 代码审查的关注点？
- 如何给出建设性意见？

### Day 139: 文档阅读技巧 (Documentation Reading Skills)
**学习文件 (Files to Study):**
- React官方文档、RFC文档
- `packages/react/README.md`

**任务 (Tasks):**
1. 学习如何阅读技术文档
2. 理解RFC的阅读方法
3. 学习源码注释的理解

**实践 (Practice):**
```bash
# 阅读React RFC
# https://github.com/reactjs/rfcs

# 查看源码注释
# 理解API的设计意图
```

**笔记要点 (Notes):**
- 如何提取文档关键信息？
- RFC与源码的关系？

### Day 140: 问题排查流程 (Problem Solving Process)
**学习文件 (Files to Study):**
- `scripts/` 目录下的调试脚本

**任务 (Tasks):**
1. 学习系统化的调试流程
2. 理解问题诊断的方法
3. 学习解决方案的验证

**实践 (Practice):**
```bash
# 定义问题
# 收集信息
# 分析可能原因
# 制定解决方案
# 验证结果
```

**笔记要点 (Notes):**
- 调试的最佳实践？
- 如何避免调试误区？

### Day 141-150: 综合项目实践 (Comprehensive Project Practice)
**学习任务 (Study Tasks):**
1. 选择一个React应用进行源码级分析
2. 尝试修复一个真实issue
3. 编写一篇React源码学习总结

**实践项目 (Practice Projects):**
- 分析一个小型React应用的结构
- 理解其中关键组件的实现
- 提出性能优化建议
- 提交PR到React仓库
- 分享学习经验

**最终目标 (Final Goals):**
- 掌握React源码的调试方法
- 能够独立解决React相关问题
- 具备源码级的问题排查能力
- 准备继续深入学习React生态

## 每日学习流程 (Daily Learning Workflow)

### Step 1: 准备 (Preparation) - 5分钟
```bash
# 进入项目目录
cd /path/to/react19

# 查看当天需要学习的文件
echo "Today's files: [list from above]"

# 打开文件开始阅读
code packages/react/src/ReactClient.js
```

### Step 2: 阅读 (Reading) - 20分钟
1. **通读第一遍**：不求甚解，了解大概
2. **精读第二遍**：逐行理解，查阅文档
3. **总结第三遍**：提取核心概念

### Step 3: 实践 (Practice) - 5分钟
```bash
# 运行相关测试
yarn test --testPathPattern="ReactClient" --verbose

# 或者查看测试用例
find packages/react/__tests__ -name "*.js" | head -5
```

### Step 4: 笔记 (Notes) - 输出要求
每天必须输出学习笔记，格式如下：

```markdown
# Day X: [标题]

## 今日学习 (Today's Learning)
- 核心概念: [1-2个关键概念]
- 重点文件: [文件名列表]

## 源码理解 (Source Code Understanding)
### 文件: [文件名]
- 作用: [文件的主要作用]
- 关键函数: [函数名和作用]
- 核心逻辑: [用自己的话描述]

## 代码片段 (Code Snippets)
```javascript
// 重要代码片段
function example() {
  // 解释
}
```

## 疑问点 (Questions)
- [记录不理解的地方]

## 实践验证 (Practical Verification)
- [运行了哪些测试]
- [观察到什么现象]

## 明日任务 (Tomorrow's Tasks)
- [明天的学习重点]
```

## Common Commands (常用命令)

### Installation (安装)
```bash
yarn
```

### Building (构建)
```bash
# Build all release channels (stable, experimental, canary)
yarn build

# Build specific packages for devtools
yarn build-for-devtools

# Build React Compiler packages
cd compiler
yarn snap:build
```

### Testing (测试)
```bash
# Run all tests
yarn test

# Run specific test file
yarn test -- --testPathPattern=MyTest

# Run React Compiler tests
cd compiler
yarn snap --watch

# Run DOM fixture tests
yarn test-dom-fixture
```

### Linting and Formatting (代码检查和格式化)
```bash
# Run linter
yarn lint

# Check formatting
yarn prettier-check

# Run type checker
yarn flow
```

## Debugging Guide (调试指南)

### 本地调试 (Local Debugging)

1. **添加调试日志 (Add Debug Logs):**
```javascript
// 在源码中添加
console.log('Debug point:', value);

// 然后构建
yarn build
```

2. **运行测试 (Run Tests):**
```bash
# 调试特定测试
yarn test -- --testNamePattern="useState" --verbose
```

3. **使用React DevTools (Use React DevTools):**
```bash
# 构建devtools版本
yarn build-for-devtools

# 打开浏览器扩展
open chrome://extensions
```

### 源码修改实验 (Source Code Modification)

```bash
# 1. 修改源码文件
# 2. 重新构建
yarn build

# 3. 在example项目中测试
cd fixtures/dom
yarn test

# 4. 撤销修改
git checkout -- packages/react/src/
```

## 重要学习提示 (Important Learning Tips)

1. **从API开始 (Start from API):**
   - 先理解对外暴露的API
   - 再深入内部实现

2. **关注数据流 (Focus on Data Flow):**
   - 追踪数据如何流动
   - 理解状态变化机制

3. **多看测试 (Read Tests):**
   - 测试用例是最好的文档
   - 理解用例就理解了用法

4. **循序渐进 (Progress Gradually):**
   - 不要试图一次理解所有细节
   - 每次专注于1-2个核心概念

5. **动手实践 (Hands-on Practice):**
   - 修改代码，观察变化
   - 添加console.log，理解执行流程

6. **掌握调试技巧 (Master Debugging Skills):**
   - 学会使用React DevTools检查组件状态
   - 在源码中添加调试日志追踪执行流程
   - 使用测试验证理解和修复问题
   - 理解错误信息的阅读和分析方法

7. **系统化调试 (Systematic Debugging):**
   - 从最简单的console.log开始
   - 逐步学习使用高级调试工具
   - 学会使用断点和性能分析工具
   - 养成记录调试过程和结果的习惯

## 进阶学习资源 (Advanced Learning Resources)

- **测试用例**: `packages/react/__tests__/`
- **示例项目**: `fixtures/`
- **官方文档**: https://react.dev/
- **RFC文档**: https://github.com/reactjs/rfcs

## 常见问题 (FAQ)

**Q: 如何开始阅读源码？**
A: 从入口文件开始，先理解整体架构，再深入具体实现。

**Q: 看不懂Flow类型怎么办？**
A: 可以忽略类型注解，专注于业务逻辑。

**Q: 如何验证自己的理解？**
A: 尝试修改代码，看是否符合预期；运行相关测试验证。

**Q: 学习进度慢怎么办？**
A: 每天半小时贵在坚持，理解深度比速度更重要。

**Q: 如何开始学习调试React源码？**
A: 从最简单的console.log开始，在学习过程中添加调试日志，观察执行流程。逐步学习使用React DevTools和性能分析工具。

**Q: 调试代码会影响学习吗？**
A: 不会。调试是理解源码的重要方法。添加临时的调试日志，观察代码执行过程，可以帮助你更好地理解React的工作原理。记住在学习完成后清理调试代码。

**Q: 如何选择合适的调试时机？**
A: 在关键函数入口、状态变化处、错误发生处添加调试日志。初期建议多加点，随着理解加深再精简。重要的是要理解每个调试点的意义。

**Q: 调试时遇到错误怎么办？**
A: 这是学习的好机会！认真阅读错误信息，理解错误原因，查阅相关代码和文档。React的错误信息通常很详细，可以指导你找到问题所在。

---

## Key Architecture Concepts (核心架构概念)

**React Core (`packages/react/`):**
- `src/ReactClient.js` - Client-side entry point
- `src/ReactServer*.js` - Server-side rendering entry points
- Hooks implementation in `src/ReactHooks.js`
- JSX transformation in `src/jsx/`

**Reconciler (`packages/react-reconciler/`):**
- Fiber-based reconciliation algorithm
- `ReactFiber*.js` files implement the rendering pipeline
- Handles component lifecycle, state updates, and DOM mutations

**React Compiler (`compiler/`):**
- Compiles React components to optimize re-renders
- Validates Rules of React
- Outputs optimized code with minimal reactive scopes

**Shared (`packages/shared/`):**
- Platform-agnostic utilities
- Feature flags in `forks/ReactFeatureFlags.*.js`
- React internals and symbols

## Development Workflow (开发流程)

### Release Channels
React maintains multiple release channels configured via `RELEASE_CHANNEL` environment variable:
- **stable** - Production releases
- **experimental** - Feature development and canary builds
- **www-classic** / **www-modern** - Meta's internal Facebook.com builds

### Build Artifacts
Builds are generated in `build/` directory with channel-specific subdirectories:
- `build/node_modules/` - Node-compatible builds
- `build/oss-experimental/` - Open source experimental builds

### Flow Types
- Source files use Flow type annotations (`.js` files with `@flow` pragma)
- Flow configs are generated automatically via `postinstall` script
- Type definitions are synced with Flow's built-in React libdefs

### Test Structure
- Unit tests in `__tests__/` directories within each package
- Integration tests in `fixtures/` directory
- Compiler tests use golden file snapshots in `compiler/packages/*/src/__tests__/fixtures/`
- Custom Jest configuration in `scripts/jest/`

## Important Notes (重要说明)

- The repository uses Yarn workspaces (check `package.json` `workspaces` field)
- Always run `yarn` after pulling changes to install dependencies
- Build artifacts are git-ignored and must be rebuilt locally
- React Compiler has its own separate build system and test runner
- Multiple Node environments are supported (development/production variants)
- The `act()` testing utility is required for testing side effects

## Debugging (调试)

### Test Debugging
```bash
# Run tests with Node debugger attached
yarn test --debug

# Run specific test with verbose output
yarn test -- --testNamePattern="My Test" --verbose
```

### Build Debugging
```bash
# Validate build output
yarn lint-build

# Check for circular dependencies
yarn build --type=NODE_DEV react/index,react-dom/index
```

### Compiler Debugging
```bash
cd compiler

# Run compiler tests with snapshot updates
yarn snap --update-snap

# Inspect compiler output for specific fixture
yarn test -- --testNamePattern="hello"
```
