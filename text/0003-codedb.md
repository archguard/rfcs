- Start Date: 2022-11-11
- RFC PR: (leave this empty)
- ArchGuard Issue: (leave this empty)

# Summary

CodeDB 将基于 MongoDB 构建数据操作能力。

- 数据操作
  - 兼容现有数据接口
  - 自定义数据结构。
     - 场景：APM 相关工具，如 Skywalking
     - 场景：用户体验度量。诸如于
  - 基于 MongoDB 构建 DSL 查询
- 自服务数据分析
  - 自助式构建适应度函数
  - 函数公式计算支持
  - 分析元数据存储

# Basic example



# Motivation

现有的架构治理机制是基于技术架构，对于数字化架构的支持是有限的。为了提供更好的数字化架构支持，需要提供自助式的数据操作能力及分析能力。

# Detailed design

1. 数据接入标准化。
2. 自助式数据查询与处理接口（参考 MongoDB）。
3. 适应度函数设计与公式计算
4. 公式计算缓存机制设计

# Drawbacks

迁移现有的应用架构成本高

# Alternatives

无

# Adoption strategy

需要构建一个新的数据查询机制。

# How we teach this

- 适应度函数
- 

# Unresolved questions

如何构建元计算引擎？