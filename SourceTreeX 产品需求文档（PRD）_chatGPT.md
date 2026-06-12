# SourceTreeX 产品需求文档（PRD）

Version: 1.0

Author: ChatGPT

Date: 2026-06-13

---

# 1. 产品简介

## 1.1 产品名称

SourceTreeX

## 1.2 产品定位

面向开发者的源码分析与知识沉淀平台。

通过调用树、架构图、AI解释、运行时Trace等能力，帮助开发者快速理解大型框架和企业级项目源码。

---

# 2. 产品愿景

让源码阅读像浏览地图一样简单。

实现：

源码

↓

调用树

↓

知识图谱

↓

AI理解

↓

团队知识库

形成完整闭环。

---

# 3. 用户画像

## 3.1 框架源码学习者

典型场景：

* Spring源码学习
* Spring Boot源码学习
* Dubbo源码学习
* Netty源码学习
* Hive源码学习
* Spark源码学习
* Flink源码学习

---

## 3.2 企业开发人员

典型场景：

* 新员工熟悉系统
* 排查线上问题
* 架构理解
* 技术分享

---

## 3.3 架构师

典型场景：

* 理解复杂系统
* 生成架构图
* 知识沉淀

---

# 4. 核心设计理念

当前源码阅读工具存在的问题：

* 调用链查看能力弱
* 阅读结果无法沉淀
* 缺少整体视角
* Debug信息割裂
* AI无法理解上下文

SourceTreeX核心理念：

> 将源码转化为结构化知识。

---

# 5. 功能架构

```text
SourceTreeX

├── 调用树
├── Debug Trace
├── 架构图
├── AI Copilot
├── 知识库
└── 团队协作
```

---

# 6. V1 功能

## 6.1 调用树生成

### 功能描述

从任意方法生成调用树。

### 入口

右键菜单

```text
Generate Call Tree
```

### 示例

```text
SpringApplication.run()
│
├── prepareEnvironment()
│
├── createApplicationContext()
│
├── refresh()
│
└── Tomcat.start()
```

---

## 6.2 调用树浏览

支持：

* 展开
* 折叠
* 搜索
* 快速定位

### 树风格

采用：

```text
Linux tree

IDEA Project Tree

Source Insight
```

风格。

---

## 6.3 节点跳转

双击节点：

```text
refresh()
```

自动跳转：

```java
AbstractApplicationContext.refresh()
```

源码位置。

---

## 6.4 节点标记

支持：

```text
⭐ 收藏

🔥 热点

🐞 Bug

📝 笔记

✅ 已掌握

📚 待学习
```

---

## 6.5 节点备注

支持Markdown格式。

示例：

```markdown
Spring启动核心入口

负责：

- BeanFactory初始化
- Bean实例化
- Event发布
```

---

## 6.6 阅读进度管理

状态：

```text
未阅读

阅读中

已掌握
```

展示：

```text
✓ prepareEnvironment()

✓ createContext()

□ refresh()
```

---

## 6.7 收藏夹

支持：

```text
我的收藏
```

例如：

```text
refresh()

createBean()

doDispatch()
```

---

## 6.8 导出

支持：

```text
Markdown

Mermaid

JSON

PNG

HTML
```

---

# 7. V2 功能

## 7.1 自动生成调用树

自动分析项目：

```java
SpringApplication.run()
```

生成：

```text
完整调用树
```

无需人工维护。

---

## 7.2 主干链路提取

从：

```text
5000节点
```

提取：

```text
关键链路
```

例如：

```text
SpringApplication.run()

↓

refresh()

↓

createWebServer()

↓

Tomcat.start()
```

---

## 7.3 调用频率分析

统计：

```text
调用次数
```

展示：

```text
getBean() [362]
```

---

## 7.4 热点函数排行

自动生成：

```text
Top 20 Core Methods
```

---

## 7.5 模块边界分析

识别：

```text
Spring Boot

↓

Spring Framework

↓

Tomcat

↓

JDK
```

展示跨模块调用。

---

## 7.6 架构图生成

自动生成：

* 组件图
* 模块图
* 服务图
* 依赖图

---

## 7.7 多视图

支持：

```text
调用树

时序图

架构图

知识图谱
```

切换。

---

# 8. Debug Trace

## 8.1 实时执行树

Debug期间：

```text
当前执行节点高亮
```

例如：

```text
→ refresh()
```

---

## 8.2 执行进度

展示：

```text
✓ 已执行

→ 当前执行

□ 未执行
```

---

## 8.3 Trace耗时

展示：

```text
compile() 9s

execute() 3s
```

---

## 8.4 性能分析

自动发现：

* 热点方法
* 慢函数
* 重复调用

---

## 8.5 异步链路

支持：

```java
@Async

CompletableFuture

ThreadPoolExecutor
```

展示跨线程调用。

---

# 9. AI Copilot

## 9.1 AI解释节点

右键：

```text
Explain This Node
```

输出：

* 功能
* 输入
* 输出
* 调用关系
* 注意事项

---

## 9.2 AI总结调用树

例如：

```text
5000节点
```

总结：

```text
10个关键步骤
```

---

## 9.3 AI源码笔记

自动生成：

```markdown
Spring启动流程

Hive执行流程

Spark DAG流程
```

---

## 9.4 AI问答

例如：

```text
为什么这里创建代理对象？
```

结合源码上下文回答。

---

## 9.5 AI学习模式

自动生成：

```text
推荐阅读顺序
```

例如：

```text
Spring启动

↓

Bean生命周期

↓

AOP

↓

事务
```

---

# 10. 知识库系统

## 10.1 本地知识库

保存：

* 调用树
* 标签
* 收藏
* 笔记

---

## 10.2 项目知识图谱

自动构建：

```text
Controller

↓

Service

↓

DAO

↓

MQ

↓

DB
```

---

## 10.3 搜索

支持：

* 方法搜索
* 类搜索
* 标签搜索

---

# 11. 团队协作

## 11.1 知识共享

支持：

```text
上传

同步

分享
```

---

## 11.2 团队知识库

统一维护：

* Spring启动
* Hive执行流程
* Spark DAG
* 企业系统架构

---

## 11.3 新人培训

自动生成：

```text
学习路线图
```

---

# 12. 数据模型

## TreeNode

```java
class TreeNode {

    String id;

    String className;

    String methodName;

    String filePath;

    Integer lineNumber;

    List<TreeNode> children;

    Set<String> tags;

    String note;

    Boolean favorite;

    Integer callCount;

    Long costTime;
}
```

---

## KnowledgeProject

```java
class KnowledgeProject {

    String projectId;

    String projectName;

    List<TreeNode> trees;

    String aiSummary;
}
```

---

# 13. 技术路线

IDEA Plugin

↓

PSI

↓

UAST

↓

Call Graph

↓

SQLite

↓

AI Agent

---

# 14. 商业化规划

## 免费版

支持：

* 调用树
* 标记
* 导出

---

## 专业版

支持：

* Debug Trace
* 性能分析
* AI解释

---

## 企业版

支持：

* 团队知识库
* 私有化部署
* 企业知识图谱

---

# 15. Roadmap

## Phase 1

调用树

源码跳转

标签

笔记

收藏

---

## Phase 2

Trace

耗时分析

架构图

---

## Phase 3

AI Copilot

知识图谱

团队协作

---

# 16. 产品目标

成为开发者阅读源码的第二大脑。

实现：

阅读源码

↓

理解源码

↓

沉淀知识

↓

团队共享

形成完整闭环。
