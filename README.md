# 🚀 Meteora LB Pair PNL Tracker

Professional Meteora liquidity mining analysis tool that displays real P&L data and participant information for each pool.

## 📖 Usage

1. **Open**: `meteora-tracker.html` in your browser
2. **Browse**: Meteora pools sorted by earning potential
3. **Analyze**: Click pools to see wallet participants and their PnL

The app works entirely in your browser with live data from Meteora API.

## ✨ Features

- **Pool Rankings**: Pools sorted by earning potential (30min Fee/TVL ratio)
- **Wallet Analysis**: See which wallets are in each pool and their PnL
- **Real-time Data**: Live data from Meteora API
- **Smart Filtering**: Search and filter pools by various criteria
- **Portfolio Links**: Direct links to view wallets on LP Agent

## 📁 What's Included

- `meteora-tracker.html` - The main web application
- `fetch_with_meteora.py` - 数据获取脚本（支持增量更新）
- `push_data.py` - GitHub 自动推送脚本
- `v2.svg` - App icon
- `README.md` - This documentation

## 🔄 增量更新功能

### 智能数据更新
现在支持智能增量更新，只获取新池子的数据：

```bash
# 增量更新（默认模式）- 只获取新池子数据
python3 fetch_with_meteora.py --limit 20

# 全量更新 - 重新获取所有池子数据
python3 fetch_with_meteora.py --limit 20 --mode full

# 自动推送到 GitHub
python3 push_data.py
```

### 工作原理
1. **检查现有数据**: 读取 `dune_data.json` 中已有的池子数据
2. **获取 Meteora 列表**: 从 API 获取最新的池子列表
3. **智能对比**: 找出需要更新的新池子
4. **增量查询**: 只对新池子执行 Dune 查询
5. **数据合并**: 将新数据与现有数据合并
6. **自动保存**: 保存完整的合并结果

### 优势
- ⚡ **高效**: 避免重复查询已有数据
- 💰 **省钱**: 减少 Dune API 调用次数
- 🚀 **快速**: 只处理必要的新数据
- 🔄 **智能**: 自动检测需要更新的内容

## 🎯 Target Users

- **Beginner LP Traders**: Get insights into profitable pools and earning potential
- **Intermediate LP Traders**: Discover successful wallet strategies to copy
- **Advanced LP Traders**: Analyze sophisticated patterns and extract logic

---

**Happy LP Trading! 🌊💰**

### Main Interface

#### 1. **LB Pairs Overview**
- View top 50 LB pairs sorted by 24h fees (default)
- Each pair shows: Liquidity, 24h Volume, 24h Fees, APR, Bin Step, Base Fee
- Click pair names to open in Meteora app

#### 2. **Filtering & Sorting**
- **Search**: Enter pair name or address
- **Min Liquidity**: Filter pairs with minimum liquidity threshold
- **Min 24h Volume**: Filter pairs with minimum volume threshold
- **Sort Options**: Click buttons to sort by different metrics:
  - **Liquidity**: Total value locked in the pool
  - **24h Volume**: Trading volume in the last 24 hours
  - **24h Fees**: Fees generated in the last 24 hours
  - **APR**: Annual percentage rate
  - **Bin Step**: Price step between bins
  - **Base Fee**: Base fee percentage
  - **30min Fee/TVL**: 🔥 **NEW** - Fee to TVL ratio in the last 30 minutes (best for finding hot pools)
- **Items per Page**: Choose how many pairs to display

#### 3. **Wallet PNL Analysis**
- Click "Get PNL" button to load wallet data for a pair
- Expand pairs to see detailed wallet rankings
- Each wallet shows total PNL and token breakdown

### Wallet Management

#### 1. **Loading Wallet Data**
- Click "Load Wallet Data" to fetch from Dune
- System tries local file first, then API
- Data loads once and persists during session

#### 2. **Wallet Filtering**
- **Search Wallet**: Filter by wallet address
- **Min PNL**: Show only wallets above PNL threshold
- **Sort Options**: Sort by PNL amount or wallet address
- **Pagination**: Navigate through wallet lists

#### 3. **Portfolio Viewing**
- Click any wallet address to view portfolio on LP Agent
- Opens in new tab for seamless experience

## 🧠 智能数据加载

### 工作原理
应用采用智能数据加载策略，按以下优先级获取钱包数据：

1. **🔍 检查本地文件**: 首先尝试读取 `dune_data.json`
2. **📡 API 获取**: 如果本地文件不存在，自动从 Dune API 获取
3. **💾 自动保存**: API 获取成功后，自动下载数据文件供下次使用
4. **⚡ 即时加载**: 数据获取后立即处理并显示

### 配置选项
```javascript
const DEVELOPMENT_MODE = true;  // 启用智能模式
const DUNE_API_KEY = 'your_api_key_here';  // 你的 Dune API Key
const DUNE_QUERY_ID = 5545533;  // Dune 查询 ID
```

### 使用场景
- **首次使用**: 没有本地文件时，自动从 API 获取
- **数据更新**: 删除本地文件后重新获取最新数据
- **离线使用**: 有本地文件时，无需网络连接

## 🔥 新 API 优势

### 智能排序
- **预排序数据**: API 直接返回按 30分钟 Fee/TVL 比率排序的数据
- **热门池优先**: 最有潜力的池子排在最前面
- **实时更新**: 数据实时反映市场热度

### 质量过滤
- **最低 TVL**: 自动过滤 TVL < $200 的小池子
- **减少噪音**: 专注于有意义的流动性池
- **提高效率**: 减少无效数据的处理

### 高效分页
- **一次性加载**: 获取所有符合条件的池子数据
- **客户端分页**: 快速翻页，无需额外 API 请求
- **性能优化**: 减少网络请求，提升用户体验

## 🔧 Configuration

### Dune Data Setup

#### Option 1: Local File
Create `dune_data.json` with this structure:
```json
{
  "result": {
    "rows": [
      {
        "lbPair": "2BYARaQtyAo22XRmdh3F5amfRr8pX7SUDTYMAMheetje",
        "wallet_array": [
          "HmnoD2K1A4J6g8cYPuo3pZoMzx96qRqvxCJmfL26Airx",
          "AnotherWalletAddressHere123456789",
          "YetAnotherWalletAddress987654321"
        ]
      }
    ]
  }
}
```

**New Dune SQL Format:**
```sql
SELECT
    lb.lbPair,
    array_agg(DISTINCT pc.evt_tx_signer) as wallet_array
FROM lbpairs lb
-- ... rest of your query
```

**Data Format Compatibility:**
The application supports multiple Dune data formats:
- ✅ **New Format**: `{lbPair: "...", wallet_array: ["wallet1", "wallet2", ...]}`
- ✅ **Old Format**: `{lbPair: "...", wallet: "single_wallet"}`
- ✅ **Legacy Format**: `"lbPair/wallet"` string format

This ensures backward compatibility with existing data files.

#### Option 2: API Configuration
1. **Get Dune Analytics API key**
   - Visit [Dune Analytics](https://dune.com)
   - Create an account and get your API key from settings

2. **Update the API key in `meteora-tracker.html`**
   - Find this line (around line 157):
   ```javascript
   const options = {method: 'GET', headers: {'X-DUNE-API-KEY': 'your dune api'}};
   ```
   - Replace `'your dune api'` with your actual API key:
   ```javascript
   const options = {method: 'GET', headers: {'X-DUNE-API-KEY': 'YOUR_ACTUAL_API_KEY'}};
   ```

⚠️ **Important**: The application will not load wallet data without a valid Dune API key or local data file.

### Customization Options

#### Pagination Defaults
```javascript
let itemsPerPage = 10;  // Main pairs pagination
itemsPerPage: 5         // Wallet pagination per pair
```

#### Sorting Defaults
```javascript
let sortBy = 'fees';    // Default sort by 24h fees
let sortOrder = 'desc'; // Descending order
```

## 🎯 Key Features Explained

### 🧠 Smart Data Loading
- **智能加载策略**: 本地文件 → Dune API → 错误处理
- **自动下载**: API 获取数据后自动下载到本地
- **会话缓存**: 钱包数据在会话期间持久化
- **API 限制**: 内置延迟以遵守 API 限制

### Advanced Filtering
- **Real-time Search**: Instant filtering as you type
- **Multi-criteria**: Combine search, liquidity, and volume filters
- **Independent States**: Each pair maintains separate wallet filters

### User Experience
- **Responsive Design**: Works on desktop, tablet, and mobile
- **Loading States**: Clear indicators for all async operations
- **Error Handling**: Graceful degradation when APIs fail
- **External Links**: Seamless integration with external platforms

## 🔍 Troubleshooting

### Common Issues

#### 1. **No Wallet Data**
- **Check API Key**: Ensure you've replaced `'your dune api'` with your actual Dune API key
- **Local File**: Alternatively, create a `dune_data.json` file with wallet data
- **Console Errors**: Check browser console (F12) for error messages
- **Manual Load**: Click "Load Wallet Data" button manually

#### 2. **API Errors**
- Verify Dune API key is valid and has sufficient credits
- Check network connectivity
- Try refreshing the page

#### 3. **Performance Issues**
- Reduce items per page if loading is slow
- Use local file instead of API for faster loading
- Clear browser cache if needed

### Debug Tools
- Click "Debug Info" button to view current state
- Open browser console (F12) for detailed logs
- Check Network tab for API call status

## 🚀 Advanced Usage

### Custom Queries
Modify the Dune query ID to use your own queries:
```javascript
const response = await fetch('https://api.dune.com/api/v1/query/YOUR_QUERY_ID/results', options);
```

### Styling Customization
The app uses Tailwind CSS classes. Modify colors and layout by editing the HTML classes.

### Data Export
Use browser developer tools to export filtered data:
```javascript
console.log(JSON.stringify(filteredPairs, null, 2));
```

## 📊 Data Sources

- **Meteora DLMM API**: Live pair data and PNL information
  - **API Endpoint**: `https://dlmm-api.meteora.ag/pair/all_by_groups`
  - **Smart Sorting**: Pre-sorted by 30min Fee/TVL ratio (highest first)
  - **Quality Filter**: Only shows pairs with TVL ≥ $200
  - **One-time Load**: Fetches all data once, client-side pagination
- **Dune Analytics**: Wallet-to-pair mappings
- **LP Agent**: Portfolio visualization
- **Meteora App**: Official pair interface

## 🤝 Contributing

1. Fork the repository
2. Make your changes
3. Test thoroughly
4. Submit a pull request

## 📄 License

This project is open source. Feel free to use, modify, and distribute.

## 🚀 API 使用说明

### 新 API 接口
- **接口地址**: `https://dlmm-api.meteora.ag/pair/all_by_groups`
- **请求参数**:
  - `page=0`: 页码（固定为 0，获取所有数据）
  - `limit=20`: 限制数量（实际返回所有符合条件的数据）
  - `sort_key=feetvlratio30m`: 按 30分钟 Fee/TVL 比率排序
  - `hide_low_tvl=200`: 隐藏 TVL < $200 的池子

### 数据结构
```json
{
  "groups": [
    {
      "name": "TOKEN-SOL",
      "pairs": [
        {
          "address": "...",
          "name": "TOKEN-SOL",
          "liquidity": "1000.0",
          "fees_24h": 500.0,
          "fee_tvl_ratio": {
            "min_30": 25.5
          },
          ...
        }
      ]
    }
  ],
  "total": 2121
}
```

### 使用方式
1. **一次性加载**: 应用启动时获取所有符合条件的池子数据
2. **分组数据**: API 按代币对分组返回数据（如 CLIPPY-SOL 组包含多个不同参数的池子）
3. **预排序数据**: 每组内的池子按 30分钟 Fee/TVL 比率排序
4. **扁平化处理**: 前端将所有组的池子合并为一个列表
5. **客户端分页**: 在前端进行分页处理，翻页速度快
6. **质量过滤**: 只显示有价值的池子（TVL ≥ $200）

### 优势
- ✅ **减少请求**: 只需一次 API 调用
- ✅ **快速翻页**: 客户端分页，无需等待
- ✅ **数据质量**: 预过滤低价值池子
- ✅ **智能排序**: 最热门的池子排在前面

## 🌐 GitHub Pages 部署

### 快速部署到 GitHub Pages

1. **创建 GitHub 仓库**
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/meteora-tracker.git
   git push -u origin main
   ```

2. **启用 GitHub Pages**
   - 进入 GitHub 仓库设置
   - 找到 **Pages** 部分
   - 选择 **Deploy from a branch**
   - 选择 **main** 分支和 **/ (root)** 文件夹
   - 点击 **Save**

3. **访问应用**
   ```
   https://YOUR_USERNAME.github.io/meteora-tracker/meteora-tracker.html
   ```

### GitHub Pages 优势
- ✅ **免费托管**：完全免费的静态网站托管
- ✅ **自动部署**：推送代码后自动更新
- ✅ **HTTPS 支持**：自动提供安全连接
- ✅ **CDN 加速**：全球内容分发网络
- ✅ **无 CORS 问题**：同域名下访问 JSON 文件

### 更新数据流程
```bash
# 更新 dune_data.json 后
git add dune_data.json
git commit -m "Update wallet data"
git push
```

## 🆘 Support

For issues or questions:
1. Check the troubleshooting section
2. Review browser console logs
3. Verify API configurations
4. Test with sample data

---

**Happy Trading! 🚀**
