# Meteora Pool Tracker - 使用说明

## 🚀 快速开始

### 1. 配置环境
确保 `.env` 文件包含：
```
DUNE_API_KEY=your_dune_api_key
DUNE_QUERY_ID=5730578
```

### 2. 获取数据

**优先使用缓存（推荐）**：
```bash
python3 fetch_dune_data.py
```

**强制重新执行查询**：
```bash
python3 fetch_dune_data.py --refresh
# 或
python3 fetch_dune_data.py -r
```

### 3. 启动前端
```bash
python3 -m http.server 8000
```
然后访问：http://localhost:8000/meteora-tracker.html

## 📊 数据流程

1. **Dune 查询** → 获取所有池子的钱包数据
2. **数据转换** → 转换为前端兼容格式
3. **保存文件** → 保存到 `dune_data.json`
4. **前端加载** → 自动加载并显示数据

## 🔧 核心文件

- `fetch_dune_data.py` - 数据获取脚本
- `meteora-tracker.html` - 前端界面
- `dune_data.json` - 数据文件
- `.env` - 配置文件

## 📈 数据格式

输出的 `dune_data.json` 格式：
```json
{
  "result": {
    "rows": [
      {
        "contract_identifier": "池子地址",
        "participant_collection": ["钱包1", "钱包2"]
      }
    ]
  },
  "fetched_at": "2025-01-05T12:00:00"
}
```

## ⚡ 特点

- **智能缓存**：优先使用最近结果，节省查询配额
- **强制刷新**：支持 `--refresh` 参数强制重新执行
- **直接替换模式**：每次运行完全更新数据
- **单次查询**：一次获取所有池子数据
- **自动格式转换**：兼容现有前端
- **简单配置**：只需配置 .env 文件

## 💡 使用建议

- **日常使用**：直接运行 `python3 fetch_dune_data.py`（使用缓存）
- **数据更新**：使用 `python3 fetch_dune_data.py --refresh`（重新执行SQL）
- **开发调试**：使用缓存模式可以快速测试，避免消耗查询配额

## 🎯 成功运行示例

```
🚀 开始获取 Meteora 池子钱包数据...
查询 ID: 5730578
✅ 数据获取完成！
  - 总池子数: 56
  - 总钱包数: 22354
  - 平均每池钱包数: 399.2
```
