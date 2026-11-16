# 部署指南

## 快速开始

### 本地开发

```bash
git clone <repo-url>
cd daily_stock_analysis
pip install -r requirements.txt

# 复制配置文件
cp .env.example .env
vim .env  # 填入 API Key、STOCK_LIST 等

# 运行
python main.py            # 完整分析
python main.py --market-review  # 仅大盘
python main.py --dry-run  # 仅抓取数据
```

### GitHub Actions（推荐）

1. Fork / 克隆仓库
2. 配置 Secrets：

| Secret | 必填 | 说明 |
|--------|------|------|
| OPENAI_API_KEY | 是 | LLM API Key |
| OPENAI_BASE_URL | 否 | 自定义网关地址 |
| EMAIL_SENDER | 否 | SMTP 发件地址 |
| EMAIL_PASSWORD | 否 | SMTP 密码 |
| EMAIL_RECEIVERS | 否 | 收件人（逗号分隔） |
| STOCK_LIST | 否 | 自选股列表 |

3. 配置 Variables：

| Variable | 默认值 | 说明 |
|----------|--------|------|
| LITELLM_MODEL | auto | 主模型（如 `openai/agnes-2.5-flash`） |
| REPORT_TYPE | simple | 报告类型 |

4. Workflow 自动触发：
   - Push to `main` → 自动运行分析
   - 定时 cron `35 7 * * 1-5`（UTC 07:35 = 北京时间 15:35）

### Docker 部署

```bash
docker-compose -f ./docker/docker-compose.yml up -d
# 访问 http://服务器IP:8000
```

## 生产配置建议

- **并发控制**：`MAX_WORKERS=3`（避免数据源限流）
- **超时**：`ANALYSIS_TIMEOUT_MINUTES=60`（GitHub Actions 上限 360 分钟）
- **LLM fallback**：配置 `LITELLM_FALLBACK_MODELS` 提高可用性
- **搜索降级**：若 SearXNG 公共实例不可用，配置 `SEARXNG_BASE_URLS` 自建实例
