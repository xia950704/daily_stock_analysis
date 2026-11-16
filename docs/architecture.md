# 系统架构说明

## 总体分层

```
入口层: main.py / server.py / webui.py / GitHub Actions workflow
    |
编排层: src/core/pipeline.py (任务调度、并发控制、结果汇总)
    |
+----------------+----------------+-----------------+
| 数据抓取       | 技术分析        | AI 分析          |
| data_provider/ | src/services/  | src/analyzer.py  |
|                |                | src/llm/         |
+----------------+----------------+-----------------+
    |
LLM 路由: LiteLLM 多 provider fallback
    |
通知层: src/notification.py (邮件 / 飞书 / Telegram / 企业微信)
```

## 核心模块

### src/core/ -- 主流程编排
- `pipeline.py` -- 股票分析主流程，线程池并发（默认 max_workers=3），单股失败不影响整体
- `market_review.py` -- 大盘复盘（指数、板块、资金面），结果可缓存复用
- `config_manager.py` -- 运行时配置管理

### src/services/ -- 业务服务
- `analysis_service.py` -- 单股分析入口
- `alert_service.py` -- 异动监控（价格偏离、成交量突变）
- `alphasift_service.py` -- AlphaSift 因子计算集成
- `decision_signal_service.py` -- 决策信号提取与归档

### data_provider/ -- 数据源适配
- 统一 base.py 接口，支持 efinance / akshare / tushare / yfinance / longbridge
- 多源自动 fallback，健康检查机制
- fundamental_adapter.py -- 基本面数据
- tencent_fetcher.py -- 腾讯 K 线

### src/search_service.py -- 新闻搜索
- 支持 Bocha / Tavily / Brave / SerpAPI / MiniMax / SearXNG / Anspire
- 优先级路由，自动故障转移
- SearXNG 公共实例：从 searx.space 自动发现 + 运行时健康探针

### src/llm/ -- LLM 路由
- LiteLLM 统一客户端，多 provider 支持
- 降级策略：主模型失败 -> fallback 模型
- provider_cache.py -- provider 路由缓存

## 数据流

1. 数据抓取: data_provider/ 拉取行情、基本面、股东数据
2. 技术分析: src/services/ 计算 MA、RSI、MACD、乖离率等
3. 新闻搜索: src/search_service.py 拉取个股新闻
4. AI 分析: src/analyzer.py + src/llm/ 调用 LLM 生成分析报告
5. 报告生成: Markdown 格式，保存至 reports/
6. 通知推送: src/notification.py 多渠道推送（邮件为主）

## 配置管理

- src/config.py -- 统一配置入口，支持环境变量 / .env 文件
- src/core/config_registry.py -- 配置项注册表（Web UI 可读）
- 敏感信息（API Key、持仓、成本价）通过环境变量或 .env 传入，不写入代码
- LLM 路由: LITELLM_MODEL（主模型）+ LITELLM_FALLBACK_MODELS（fallback 列表）
- 搜索: SEARXNG_BASE_URLS（自建实例）或自动发现公共实例
