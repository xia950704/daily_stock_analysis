# API 接口文档

## 健康检查

### GET /health
返回服务状态。

### GET /api/health
返回服务状态（兼容旧路径）。

## 股票分析

### POST /api/v1/analysis/analyze
触发 AI 智能分析。

```json
{
  "stocks": ["600519", "000001"],
  "mode": "full",
  "force": false
}
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| stocks | string[] | 否 | 股票代码列表（默认使用 STOCK_LIST） |
| mode | string | 否 | `full` / `market-only` / `stocks-only` |
| force | bool | 否 | 强制运行（跳过交易日检查） |

### GET /api/v1/analysis/tasks
查询当前分析任务列表。

### GET /api/v1/analysis/tasks/stream
SSE 实时推送分析任务进度。

## 历史记录

### GET /api/v1/history
查询历史分析报告。

| 参数 | 类型 | 说明 |
|------|------|------|
| stock | string | 股票代码（可选） |
| date | string | 日期 YYYY-MM-DD（可选） |
| limit | int | 返回条数（默认 10） |

### GET /api/v1/history/{query_id}
获取单条历史报告详情。

### GET /api/v1/history/{query_id}/news
获取关联新闻。

## 系统配置

### GET /api/v1/system/config
获取当前系统配置（脱敏）。

### POST /api/v1/system/config/export
导出完整配置。

### POST /api/v1/system/config/import
导入配置。

### POST /api/v1/system/config/validate
校验配置。

### GET /api/v1/system/config/schema
获取配置 schema。

## 决策信号

### GET /api/v1/signals
查询决策信号列表。

### GET /api/v1/signals/{signal_id}
获取单条信号详情。

## 认证

当 `ADMIN_AUTH_ENABLED=true` 时，除健康检查端点外，`/api/v1/*` 需要有效的管理员会话 Cookie。
