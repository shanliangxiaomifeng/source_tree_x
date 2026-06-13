# CodeFlowMind Architecture v1.0

Version: 1.0

Status: Draft

Date: 2026-06-13

---

# 1. 文档目标

本文档描述 CodeFlowMind MVP 阶段的整体技术架构设计。

主要解决：

* 如何组织代码结构
* 如何划分模块
* 各模块职责是什么
* MVP 需要实现哪些核心能力

---

# 2. MVP目标

第一阶段目标：

验证调用树阅读模式是否成立。

支持：

```text
Generate Call Tree

↓

Call Tree View

↓

双击跳转源码
```

不包含：

```text
AI Copilot

Knowledge Graph

Team Workspace

Architecture View
```

---

# 3. 总体架构

```text
CodeFlowMind Plugin

├── Action Layer
├── UI Layer
├── Tree Engine
├── Parser Layer
├── Service Layer
├── Storage Layer
├── Model Layer
└── Utility Layer
```

---

# 4. 包结构设计

```text
com.codeflowmind

├── action
│
├── ui
│
├── tree
│
├── parser
│
├── workspace
│
├── note
│
├── tag
│
├── storage
│
├── model
│
├── service
│
└── util
```

---

# 5. Action Layer

负责与 IDEA 交互。

---

## GenerateCallTreeAction

### 作用

右键菜单：

```text
Generate Call Tree
```

生成调用树。

---

### 流程

```text
获取当前 PsiMethod

↓

CallTreeService.generate()

↓

打开 ToolWindow

↓

展示调用树
```

---

## OpenWorkspaceAction

### 作用

打开 Workspace 窗口。

---

## ExportCallTreeAction

### 作用

导出：

```text
Markdown

PNG

Mermaid

.calltree
```

---

# 6. UI Layer

负责界面展示。

---

## MainToolWindow

### 作用

插件主窗口。

负责组装：

```text
WorkspacePanel

CallTreePanel

DetailPanel
```

---

### 布局

```text
┌────────────┬────────────────────┬────────────┐
│ Workspace  │ Call Tree          │ Detail     │
│            │                    │            │
└────────────┴────────────────────┴────────────┘
```

---

## WorkspacePanel

### 作用

管理多个调用树工作区。

---

### 示例

```text
Spring Startup

Hive Compiler

Spark Submit
```

---

### 实现

```java
JTree
```

---

## CallTreePanel

### 作用

展示调用树。

---

### 示例

```text
SpringApplication.run()

├── prepareEnvironment()

├── createContext()

└── refresh()
```

---

### 功能

支持：

```text
展开

折叠

搜索

过滤
```

---

### 实现

```java
JTree
```

---

## DetailPanel

### 作用

展示节点详情。

---

### 内容

```text
方法名

类名

文件

源码位置

标签

笔记
```

---

# 7. Tree Engine

核心模块。

负责构建调用树。

---

## CallTreeBuilder

### 核心入口

```java
CallTree build(PsiMethod rootMethod)
```

---

### 作用

```text
PsiMethod

↓

分析调用关系

↓

生成 CallTree
```

---

## CallTreeWalker

### 作用

遍历调用关系。

---

### 支持

```text
DFS

BFS
```

---

## TreeNodeFactory

### 作用

统一创建 TreeNode。

---

# 8. Parser Layer

负责 PSI 解析。

---

## MethodCallParser

### 作用

解析：

```java
refresh();
```

得到：

```java
AbstractApplicationContext.refresh()
```

---

## InterfaceResolver

### 作用

解析接口实现。

---

### 示例

```java
UserService
```

找到：

```java
MysqlUserService

RedisUserService
```

---

## SpringBeanResolver（V2）

### 作用

解析：

```java
@Autowired
```

找到真实 Bean。

---

# 9. Model Layer

核心数据模型。

---

## TreeNode

```java
public class TreeNode {

    String id;

    String className;

    String methodName;

    String signature;

    List<TreeNode> children;
}
```

---

## CallTree

```java
public class CallTree {

    String id;

    String name;

    TreeNode root;
}
```

---

## Workspace

```java
public class Workspace {

    String id;

    String name;

    List<CallTree> trees;
}
```

---

## Note

```java
public class Note {

    String nodeId;

    String markdown;
}
```

---

## Tag

```java
public class Tag {

    String id;

    String name;
}
```

---

# 10. Service Layer

负责业务逻辑。

---

## CallTreeService

### 作用

管理调用树。

---

### 功能

```text
生成

更新

删除
```

---

## NavigationService

### 作用

跳转源码。

---

### 功能

```text
Tree → Editor

Editor → Tree
```

---

## NoteService

### 作用

管理笔记。

---

## TagService

### 作用

管理标签。

---

# 11. Storage Layer

负责持久化。

---

## WorkspaceRepository

### 作用

保存 Workspace。

---

## CallTreeRepository

### 作用

保存调用树。

---

## JsonStorageManager

### 作用

读写：

```text
.calltree
```

文件。

---

# 12. MVP核心类

第一阶段需要实现：

```text
GenerateCallTreeAction

MainToolWindow

WorkspacePanel

CallTreePanel

DetailPanel

CallTreeBuilder

MethodCallParser

TreeNode

CallTree

Workspace

CallTreeService

NavigationService

JsonStorageManager
```

共：

```text
13个核心类
```

即可实现 MVP。

---

# 13. MVP开发顺序

## Sprint 1

实现：

```text
Generate Call Tree

CallTreePanel

双击跳转源码
```

---

## Sprint 2

实现：

```text
Workspace

保存

加载
```

---

## Sprint 3

实现：

```text
标签

收藏

笔记
```

---

## Sprint 4

实现：

```text
Markdown 导出

PNG 导出

.calltree 导出
```

---

# 14. V2扩展

增加：

```text
InterfaceResolver

SpringBeanResolver

StackTrace Import

Performance Analysis
```

---

# 15. V3扩展

增加：

```text
AI Copilot

Explain Node

AI Summary

Learning Path
```

---

# 16. V4扩展

增加：

```text
Knowledge Graph

Architecture View

Team Workspace
```

---

# 17. 最终目标

构建开发者的源码认知平台。

实现：

```text
代码

↓

调用关系

↓

执行流程

↓

知识沉淀

↓

AI理解
```

帮助开发者建立完整的代码思维模型。
