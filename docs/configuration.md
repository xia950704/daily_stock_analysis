# 配置项说明

## LLM 配置

| 环境变量 | 默认值 | 说明 |
|---------|--------|------|
| LITELLM_MODEL | (auto-infer) | 主模型，格式 `provider/model-name`（如 `openai/agnes-2.5-flash`） |
| LITELLM_FALLBACK_MODELS | (empty) | 逗号分隔的 fallback 模型列表 |
| LLM_TEMPERATURE | 0.7 | 全局温度参数 |
| OPENAI_API_KEY | - | OpenAI 兼容网关 API Key |
| OPENAI_BASE_URL | - | 自定义网关地址（如 `https://apihub.agnes-ai.com/v1`） |
| GEMINI_API_KEY | - | Google Gemini API Key |
| ANTHROPIC_API_KEY | - | Anthropic Claude API Key |
| DEEPSEEK_API_KEY | - | DeepSeek API Key |
| AGENT_LITELLM_MODEL | - | Agent 专用模型（留空继承 LITELLM_MODEL） |

### 模型前缀规则

`LITELLM_MODEL` 必须含 provider 前缀（如 `openai/agnes-2.5-flash`）。
若 `OPENAI_BASE_URL` 指向第三方 OpenAI 兼容网关（非 `api.openai.com`）且模型名无 `/`，
系统会自动补 `openai/` 前缀（`src/config.py` 行 1264 附近）。

## 股票列表

| 环境变量 | 默认值 | 说明 |
|---------|--------|------|
| STOCK_LIST | `600519` | 逗号分隔的股票代码列表 |

## 分析参数

| 环境变量 | 默认值 | 说明 |
|---------|--------|------|
| BIAS_THRESHOLD | 5.0 | 乖离率阈值（%），超过此值提示不追高 |
| MAX_WORKERS | 3 | 并发线程数（低并发防封禁） |
| NEWS_MAX_AGE_DAYS | 3 | 新闻最大时效（天） |
| ANALYSIS_TIMEOUT_MINUTES | 60 | 单次分析超时（分钟） |

## 通知渠道

| 环境变量 | 说明 |
|---------|------|
| EMAIL_SENDER / EMAIL_PASSWORD / EMAIL_RECEIVERS | SMTP 邮件配置 |
| FEISHU_WEBHOOK_URL | 飞书群机器人 |
| TELEGRAM_BOT_TOKEN / TELEGRAM_CHAT_ID | Telegram 通知 |
| WECHAT_WEBHOOK_URL | 企业微信 |
| PUSHPLUS_TOKEN | PushPlus 推送 |
| NTFY_URL | ntfy 推送 |

## 搜索引擎

| 环境变量 | 说明 |
|---------|------|
| BOCHA_API_KEYS | 博查搜索（优先，中文优化） |
| TAVILY_API_KEYS | Tavily（免费额度 1000 次/月） |
| BRAVE_API_KEYS | Brave Search |
| SERPAPI_API_KEYS | SerpAPI（100 次/月） |
| MINIMAX_API_KEYS | MiniMax 搜索 |
| SEARXNG_BASE_URLS | 自建 SearXNG 实例（无配额，逗号分隔） |
| SEARXNG_PUBLIC_INSTANCES_ENABLED | 未配置自建时是否自动发现公共实例（默认 true） |

## 数据源

| 环境变量 | 说明 |
|---------|------|
| TUSHARE_TOKEN | Tushare Pro Token |
| LONGBRIDGE_APP_KEY / LONGBRIDGE_APP_SECRET | 长桥 API 密钥 |
| FINNHUB_API_KEY | Finnhub（美股） |
