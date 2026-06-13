# CodeFlowMind PRD v2.0

Version: 2.0
Status: Draft
Author: ChatGPT + User
Date: 2026-06-13

---

# 1. 产品概述

## 1.1 产品名称

CodeFlowMind

## 1.2 产品定位

CodeFlowMind 是一个面向开发者的 **源码理解与代码导航平台（Source Code Intelligence Platform）**。

通过调用树（Call Tree）、执行流程（Execution Flow）、知识沉淀（Knowledge Base）、AI 辅助分析（AI Copilot）等能力，将复杂源码转化为可理解、可导航、可沉淀的知识地图。

---

## 1.3 产品愿景

让开发者像浏览地图一样理解代码。

实现：

```text
Source Code
    ↓
Call Graph
    ↓
Execution Flow
    ↓
Knowledge Graph
    ↓
AI Understanding
    ↓
Team Knowledge Base
```

---

# 2. 用户画像

## 2.1 源码学习者

典型场景：

* Spring Framework 源码学习
* Spring Boot 启动流程学习
* Hive SQL 编译流程学习
* Spark Job 提交流程学习
* Flink Checkpoint 流程学习

痛点：

* 调用链太深
* 容易迷失上下文
* 无法形成整体认知

---

## 2.2 企业开发人员

典型场景：

* 新员工熟悉系统
* Bug 排查
* 需求开发
* 技术分享

痛点：

* 系统复杂
* 文档缺失
* 上手周期长

---

## 2.3 架构师

典型场景：

* 系统架构梳理
* 模块边界识别
* 核心链路分析

痛点：

* 缺乏全局视角
* 架构知识无法沉淀

---

# 3. 产品核心价值

## 3.1 调用链可视化

传统方式：

```text
Ctrl + Click
↓
Ctrl + Click
↓
Ctrl + Click
↓
迷失
```

CodeFlowMind：

```text
SpringApplication.run()
│
├── prepareEnvironment()
├── createApplicationContext()
├── refresh()
└── Tomcat.start()
```

---

## 3.2 知识沉淀

支持：

* 标签（Tags）
* 收藏（Favorites）
* 笔记（Notes）
* 阅读状态（Learning Status）

形成长期知识资产。

---

## 3.3 AI 辅助理解

AI 能够理解：

* 当前节点
* 上下游关系
* 所属模块
* 当前调用树

提供解释和总结。

---

# 4. 功能架构

```text
CodeFlowMind

├── Workspace
├── Call Tree
├── Navigation
├── Search
├── Notes
├── Tags
├── Favorites
├── Import / Export
├── AI Copilot
├── Debug Trace
├── Architecture View
├── Knowledge Graph
└── Team Workspace
```

---

# 5. MVP（V1.0）

目标：

验证调用树阅读模式是否成立。

原则：

* 不做 AI
* 不做知识图谱
* 不做团队协作
* 聚焦调用树核心能力

---

## 5.1 Workspace

支持：

* 新建
* 删除
* 重命名
* 排序
* 导入
* 导出

示例：

```text
Workspace

├── Spring Startup
├── Hive Compiler
├── Spark Submit
└── Custom Project
```

---

## 5.2 调用树生成

入口：

右键菜单：

```text
Generate Call Tree
```

支持：

* 当前方法
* 当前类
* 当前模块

---

## 5.3 调用树浏览

支持：

* 展开
* 折叠
* 搜索
* 快速定位

UI 风格：

* IDEA Project Tree
* Linux tree
* Source Insight

---

## 5.4 双向导航

### Tree → Editor

双击：

```text
refresh()
```

跳转：

```java
AbstractApplicationContext.refresh()
```

### Editor → Tree

编辑器定位：

```java
finishRefresh()
```

树自动高亮。

---

## 5.5 标签系统

支持：

* ⭐ Favorite
* 🔥 Hot
* 🐞 Bug
* 📝 Note
* 📚 Learning
* ✅ Mastered

---

## 5.6 笔记系统

支持 Markdown。

支持：

* 标题
* 列表
* 代码块
* 引用

---

## 5.7 导出

支持：

* `.calltree`
* Markdown
* Mermaid
* PNG
* HTML

---

# 6. 技术选型

## 语言

Java 21

---

## UI

Swing

---

## 主题

FlatLaf

---

## 布局

MigLayout

---

## 静态分析

* PSI
* UAST

---

## 存储

V1：

* `.calltree`（JSON）

V2：

* SQLite

---

## AI Provider

SPI 方式支持：

* OpenAI
* Claude
* Gemini
* DeepSeek
* Qwen
* Ollama

---

# 7. Roadmap

## Phase 1

MVP：

* 调用树
* 跳转
* 标签
* 笔记
* 收藏
* 导出

---

## Phase 2

高级分析：

* 接口实现分析
* Spring Bean 分析
* StackTrace 导入
* 性能优化

---

## Phase 3

AI Copilot：

* Explain Node
* Explain Tree
* AI Q&A
* Learning Path

---

## Phase 4

Knowledge Graph：

* 模块关系图
* 项目知识图谱

---

## Phase 5

Team Workspace：

* 团队共享
* 团队知识库
* 私有化部署

---

# 8. 最终目标

成为开发者阅读源码的第二大脑。

让复杂代码变成：

```text
可导航
可理解
可沉淀
可共享
```

的知识地图。
