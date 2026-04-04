# wal-bridge

一个基于 WAL / binlog 的轻量 CDC 引擎。

## 为什么做这个项目

CDC 的核心价值不只是“同步数据”，而是把数据库的变更事件稳定、低延迟地送到其他系统。`wal-bridge` 希望从一个非常窄但扎实的场景切入，先把单源、多 sink、offset 恢复和 schema 演进做扎实。

## 第一阶段目标

- 优先支持 `Postgres WAL`
- 支持 `snapshot + incremental`
- 支持 offset/checkpoint
- 支持 1-2 种 sink
  - `Webhook`
  - `Kafka` 或 `ClickHouse`
- 支持表过滤和基础 schema 变化处理
- 支持失败恢复与重放

## 非目标

- 不在第一版同时支持多种数据库
- 不在第一版追求 exactly-once
- 不在第一版做复杂 UI

## 核心设计

- 从单一 source 做深，而不是一开始做很多 connector
- 把 snapshot、offset、schema、sink 幂等做好
- 优先保证 at-least-once 语义

## 建议技术栈

- `Go`
- `Postgres`
- `Kafka` 可选
- `ClickHouse` 可选

## 开发计划

见 [docs/INITIAL_PLAN.md](docs/INITIAL_PLAN.md)。
