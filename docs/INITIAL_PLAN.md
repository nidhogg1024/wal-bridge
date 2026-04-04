# wal-bridge 初步方案

## 项目定位

`wal-bridge` 是一个面向 Postgres 的轻量 CDC 引擎，目标是从 WAL 中捕获变更事件，并稳定地投递到外部系统。

## 为什么先做 Postgres

- Postgres 逻辑复制能力成熟
- WAL/slot 模型清晰
- 更适合作为第一个 CDC 项目的 source

先把一个 source 做深，比一开始铺开 MySQL / MongoDB / Oracle 更有学习价值。

## 核心能力

### 1. Snapshot

在首次接入时导出现有数据，作为后续增量同步的起点。

### 2. Incremental

基于 WAL 持续读取 insert / update / delete 事件。

### 3. Offset / Checkpoint

记录当前处理位置，保证程序重启后可以继续消费。

### 4. Sink

初版建议只实现两种：

- `Webhook`
- `Kafka` 或 `ClickHouse`

## 初版数据模型

### sources

- `id`
- `name`
- `dsn`
- `publication`
- `slot_name`
- `status`

### offsets

- `source_id`
- `lsn`
- `snapshot_done`
- `updated_at`

### sinks

- `id`
- `kind`
- `config_json`

### pipelines

- `id`
- `source_id`
- `sink_id`
- `table_filters`
- `status`

## 技术难点

- snapshot 与增量消费的衔接
- offset 提交时机
- schema 变化带来的事件兼容性
- sink 投递失败时的重试和幂等
- 删除事件和主键更新场景的语义处理

## 里程碑

### Milestone 1

- Postgres source
- 单表 snapshot
- WAL 增量读取
- Webhook sink

### Milestone 2

- checkpoint 恢复
- 表过滤
- 多表支持
- 失败重试

### Milestone 3

- Kafka 或 ClickHouse sink
- schema 变化基础处理
- metrics / tracing
- 压测与恢复演练

## 仓库结构建议

```text
cmd/
  wal-bridge/
internal/
  source/
    postgres/
  snapshot/
  offset/
  sink/
  pipeline/
  schema/
  storage/
docs/
```
