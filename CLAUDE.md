# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

---

# React 19 源码学习指南

## 学习目标

这是 **React 19** 的源码仓库。本指南专为想要理解 React 内部机制以提升实践开发能力的初级开发者设计。

## 仓库结构

### 核心目录

- **`packages/`** - React 核心包:
  - `react` - 核心 API (组件、Hooks、JSX)
  - `react-dom` - DOM 渲染和浏览器 API
  - `react-reconciler` - 协调算法 (React 渲染的核心)
  - `react-server` - 服务端渲染组件
  - `shared` - 跨包共享的工具
  - `scheduler` - 任务调度器

- **`compiler/`** - React 编译器 (优化编译器)

- **`scripts/`** - 构建和开发脚本

- **`fixtures/`** - 测试用例和调试示例

---

## 调试方法

```bash
# 方法一: 使用 fixtures 目录 (推荐)
cd fixtures/dom && yarn install && yarn start

# 方法二: 构建开发版本
yarn build react/index,react-dom/index --type=NODE_DEV

# 方法三: 在源码中添加 console.log 或 debugger
```

---

## 学习进度

- **当前进度**: Day 1 ✅ 完成
- **开始日期**: 2026-01-28
- **总计划**: 20 天
- **笔记目录**: `notes/`

### 学习计划概览

| 阶段 | 天数 | 内容 |
|------|------|------|
| 第一阶段 | Day 1-4 | React 基础概念 |
| 第二阶段 | Day 5-8 | Hooks 系统 |
| 第三阶段 | Day 9-12 | Fiber 架构基础 |
| 第四阶段 | Day 13-16 | 渲染流程 |
| 第五阶段 | Day 17-20 | Diff 算法与提交 |

### 关键文件索引

| 文件 | 作用 |
|------|------|
| `packages/react/index.js` | React 包入口 |
| `packages/react/src/ReactHooks.js` | Hooks 定义 |
| `packages/react/src/jsx/ReactJSXElement.js` | JSX 元素创建 |
| `packages/react-reconciler/src/ReactFiber.js` | Fiber 节点结构 |
| `packages/react-reconciler/src/ReactFiberWorkLoop.js` | 渲染主循环 |
| `packages/react-reconciler/src/ReactFiberHooks.js` | Hooks 实现 |
| `packages/react-reconciler/src/ReactFiberBeginWork.js` | beginWork 阶段 |
| `packages/react-reconciler/src/ReactFiberCompleteWork.js` | completeWork 阶段 |
| `packages/react-reconciler/src/ReactChildFiber.js` | Diff 算法 |
| `packages/react-reconciler/src/ReactFiberCommitWork.js` | Commit 阶段 |
| `packages/shared/ReactSymbols.js` | Symbol 类型定义 |
| `packages/react-reconciler/src/ReactWorkTags.js` | WorkTag 定义 |

---

## 学习笔记
每天学习完，根据对话内容生成notes/dayn.md，包含源码与问答内容

### Day 1: React 包入口与导出结构 ✅

**状态**: 已完成 | **笔记**: [notes/day1.md](notes/day1.md)

**学习文件**:
- `packages/react/index.js`
- `packages/react/src/ReactClient.js`
- `packages/shared/ReactSymbols.js`

**关键收获**:
- React 使用 Flow 类型系统，`@flow` 注解启用类型检查
- 所有 API 从 `ReactClient.js` 导出，共 40+ 个
- Fragment、Suspense 等内置组件只是 Symbol 标识符
- Symbol.for() 用于安全性，防止 XSS 攻击伪造 React 元素
- 所有 Hooks 来自 `./ReactHooks.js`（代理层）

### Day 2: ReactElement 与 JSX 转换

**学习目标**: 理解 JSX 如何转换为 ReactElement

**学习文件**:
- `packages/react/src/jsx/ReactJSXElement.js`

**关键概念**:
- ReactElement 结构 (`$$typeof`, `type`, `key`, `ref`, `props`)
- createElement 函数实现
- JSX 编译过程

---

## 学习方式说明

1. 逐行或一小段代码地阅读源码
2. 说 "下一步" 继续输出下一段代码
3. 每日学习完后生成笔记
4. 笔记更新到本文件作为项目记忆


