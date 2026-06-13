# CodeFlowMind Data Model v1.0

Version: 1.0

Status: Draft

Date: 2026-06-13

---

# 1. 文档目标

本文档定义 CodeFlowMind MVP 阶段及未来扩展阶段的数据模型设计。

目标：

* 定义核心领域模型
* 定义模型关系
* 定义 `.calltree` 文件格式
* 为后续代码生成提供统一规范
* 保证模型可扩展性

---

# 2. 设计原则

## 2.1 Workspace First

所有数据都隶属于 Workspace。

```text
Workspace

├── CallTrees
├── Notes
├── Tags
├── Settings
└── Metadata
```

---

## 2.2 聚合根原则

Workspace 为整个系统聚合根。

统一管理：

* 调用树
* 标签
* 笔记
* 设置

---

## 2.3 可扩展原则

必须支持未来扩展：

* AI Copilot
* Knowledge Graph
* Team Workspace
* Cloud Sync

避免大规模重构。

---

# 3. 数据模型总览

```text
Workspace
│
├── CallTree
│    │
│    └── TreeNode
│           │
│           ├── MethodRef
│           ├── SourceLocation
│           ├── NodeDecoration
│           └── Metadata
│
├── Note
│
├── Tag
│
├── WorkspaceSettings
│
└── Metadata
```

---

# 4. Workspace

Workspace 是系统最顶层对象。

一个 Workspace 对应一个学习主题。

例如：

```text
Spring Startup

Hive Compiler

Spark Submit
```

---

## Java定义

```java
public class Workspace {

    private String id;

    private String name;

    private String description;

    private List<CallTree> trees;

    private List<Tag> tags;

    private WorkspaceSettings settings;

    private Metadata metadata;
}
```

---

## 字段说明

| 字段          | 类型                | 说明          |
| ----------- | ----------------- | ----------- |
| id          | String            | 唯一标识        |
| name        | String            | Workspace名称 |
| description | String            | 描述          |
| trees       | List<CallTree>    | 调用树集合       |
| tags        | List<Tag>         | 标签集合        |
| settings    | WorkspaceSettings | 工作区配置       |
| metadata    | Metadata          | 元数据         |

---

# 5. WorkspaceSettings

工作区配置。

---

## Java定义

```java
public class WorkspaceSettings {

    private Integer maxDepth;

    private Boolean enableLazyLoad;

    private Boolean showLibraryMethods;

    private Set<String> ignoredPackages;
}
```

---

## 字段说明

| 字段                 | 说明       |
| ------------------ | -------- |
| maxDepth           | 最大分析深度   |
| enableLazyLoad     | 是否启用懒加载  |
| showLibraryMethods | 是否显示第三方库 |
| ignoredPackages    | 忽略包列表    |

---

# 6. CallTree

调用树根对象。

---

## Java定义

```java
public class CallTree {

    private String id;

    private String name;

    private TreeType type;

    private TreeNode root;

    private TreeStatistics statistics;

    private Metadata metadata;
}
```

---

## 字段说明

| 字段         | 说明    |
| ---------- | ----- |
| id         | 唯一标识  |
| name       | 调用树名称 |
| type       | 调用树类型 |
| root       | 根节点   |
| statistics | 统计信息  |
| metadata   | 元数据   |

---

# 7. TreeType

调用树类型。

---

## Java定义

```java
public enum TreeType {

    CALL_TREE,

    MAIN_FLOW,

    STACK_TRACE,

    DEBUG_TRACE
}
```

---

## 类型说明

| 类型          | 说明          |
| ----------- | ----------- |
| CALL_TREE   | 标准调用树       |
| MAIN_FLOW   | 主执行链路       |
| STACK_TRACE | 异常堆栈        |
| DEBUG_TRACE | Debug Trace |

---

# 8. TreeStatistics

统计信息。

---

## Java定义

```java
public class TreeStatistics {

    private Integer totalNodes;

    private Integer maxDepth;

    private Integer noteCount;

    private Integer favoriteCount;
}
```

---

# 9. TreeNode

系统最核心模型。

代表调用树中的一个节点。

---

## Java定义

```java
public class TreeNode {

    private String id;

    private NodeType nodeType;

    private MethodRef method;

    private List<TreeNode> children;

    private NodeState state;

    private NodeDecoration decoration;

    private SourceLocation location;

    private Metadata metadata;
}
```

---

## 字段说明

| 字段         | 说明     |
| ---------- | ------ |
| id         | 唯一ID   |
| nodeType   | 节点类型   |
| method     | 方法信息   |
| children   | 子节点    |
| state      | 学习状态   |
| decoration | UI装饰信息 |
| location   | 源码位置   |
| metadata   | 元数据    |

---

# 10. MethodRef

方法引用。

用于唯一标识方法。

---

## Java定义

```java
public class MethodRef {

    private String className;

    private String methodName;

    private String signature;

    private String returnType;

    private List<String> parameterTypes;
}
```

---

## 示例

```java
AbstractApplicationContext.refresh()
```

对应：

```text
className:
org.springframework.context.support.AbstractApplicationContext

methodName:
refresh

signature:
refresh()

returnType:
void

parameterTypes:
[]
```

---

# 11. SourceLocation

源码位置。

---

## Java定义

```java
public class SourceLocation {

    private String filePath;

    private Integer lineNumber;

    private Integer columnNumber;
}
```

---

## 用途

支持：

```text
双击跳转源码
```

---

# 12. NodeType

节点类型。

---

## Java定义

```java
public enum NodeType {

    METHOD,

    INTERFACE,

    IMPLEMENTATION,

    SPRING_BEAN,

    LAMBDA,

    ASYNC,

    EXTERNAL
}
```

---

## 类型说明

| 类型             | 说明          |
| -------------- | ----------- |
| METHOD         | 普通方法        |
| INTERFACE      | 接口          |
| IMPLEMENTATION | 接口实现        |
| SPRING_BEAN    | Spring Bean |
| LAMBDA         | Lambda表达式   |
| ASYNC          | 异步调用        |
| EXTERNAL       | 外部库         |

---

# 13. NodeState

学习状态。

---

## Java定义

```java
public enum NodeState {

    NOT_STARTED,

    LEARNING,

    UNDERSTOOD,

    MASTERED
}
```

---

## UI展示

```text
□ Not Started

◐ Learning

✓ Understood

★ Mastered
```

---

# 14. NodeDecoration

节点装饰信息。

---

## Java定义

```java
public class NodeDecoration {

    private Boolean favorite;

    private Set<String> tagIds;

    private Boolean hasNote;
}
```

---

## UI对应

```text
⭐ Favorite

🔥 Hot

📝 Note
```

---

# 15. Note

笔记对象。

---

## Java定义

```java
public class Note {

    private String id;

    private String nodeId;

    private String markdown;

    private Metadata metadata;
}
```

---

## 设计原则

Note 独立存储。

不直接挂载 TreeNode。

支持未来：

* 团队协作
* 全文检索
* 云同步

---

# 16. Tag

标签对象。

---

## Java定义

```java
public class Tag {

    private String id;

    private String name;

    private String color;

    private TagType type;
}
```

---

# 17. TagType

标签类型。

---

## Java定义

```java
public enum TagType {

    FAVORITE,

    HOT,

    BUG,

    NOTE,

    CUSTOM
}
```

---

# 18. Metadata

统一元数据。

---

## Java定义

```java
public class Metadata {

    private Instant createdAt;

    private Instant updatedAt;

    private String createdBy;

    private String version;
}
```

---

## 用途

支持未来：

* 云同步
* 团队协作
* 历史追踪

---

# 19. .calltree 文件格式

采用 JSON 格式。

---

## 文件结构

```json
{
  "workspace": {},

  "trees": [],

  "notes": [],

  "tags": []
}
```

---

## 示例

```json
{
  "workspace": {
    "id": "workspace-001",
    "name": "Spring Startup"
  },

  "trees": [
    {
      "id": "tree-001",
      "name": "SpringApplication.run"
    }
  ],

  "notes": [],

  "tags": []
}
```

---

# 20. UML关系图

```text
Workspace
│
├── CallTree
│    │
│    └── TreeNode
│           │
│           ├── MethodRef
│           ├── SourceLocation
│           ├── NodeDecoration
│           └── Metadata
│
├── Note
│
├── Tag
│
├── WorkspaceSettings
│
└── Metadata
```

---

# 21. MVP实现范围

V1 实现：

```text
Workspace

CallTree

TreeNode

MethodRef

SourceLocation

NodeDecoration

Note

Tag

Metadata
```

共：

```text
9 个核心模型
```

---

# 22. V2扩展

增加：

```text
KnowledgeNode

ArchitectureNode

DebugTraceNode
```

---

# 23. V3扩展

增加：

```text
User

Team

Permission

SyncMetadata
```

支持团队协作。

---

# 24. 总结

DataModel 是 CodeFlowMind 的核心基础设施。

设计目标：

```text
稳定

简单

可扩展

面向未来
```

支撑：

```text
调用树

知识沉淀

AI理解

团队协作
```

逐步演进。
