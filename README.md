# Meteora LB Pair PNL Tracker

A comprehensive web application for tracking and analyzing Meteora Liquidity Book (LB) pairs and their associated wallet PNL data. This tool provides real-time insights into liquidity pool performance and wallet profitability.

## 🚀 Features

### 📊 LB Pair Analytics
- **Real-time Data**: Fetches live data from Meteora DLMM API
- **Comprehensive Metrics**: Displays liquidity, 24h volume, fees, APR, bin step, and base fee
- **Smart Sorting**: Multiple sorting options (fees, liquidity, volume, APR, name, bin step, base fee, 30min fee/TVL ratio)
- **Advanced Filtering**: Search by name/address, filter by minimum liquidity and volume
- **Pagination**: Configurable items per page (10, 25, 50, 100)

### 💰 Wallet PNL Tracking
- **Dune Integration**: Loads wallet data from Dune Analytics with local file fallback
- **Individual PNL**: Fetch detailed PNL data for each wallet
- **Wallet Filtering**: Search wallets and filter by minimum PNL
- **Wallet Pagination**: Independent pagination for each pair's wallet list
- **Portfolio Links**: Direct links to LP Agent portfolio viewer

### 🔗 External Integrations
- **Meteora App**: Click pair names to view on official Meteora platform
- **LP Agent**: Click wallet addresses to view portfolios
- **Dune Analytics**: Automated wallet data fetching with API key support

## 📁 File Structure

```
meteora-tracker/
├── meteora-tracker.html    # Main application file
├── dune_data.json         # Local Dune data cache (optional)
└── README.md             # This documentation
```

## 🛠️ Setup & Installation

### Prerequisites
- Modern web browser (Chrome, Firefox, Safari, Edge)
- Internet connection for API calls
- **Dune Analytics API key (required for wallet data)** - Get yours at [dune.com](https://dune.com)

### Quick Start
1. **Download Files**
   ```bash
   git clone <repository-url>
   cd meteora-tracker
   ```

2. **Configure Dune API Key (REQUIRED)**
   ⚠️ **This step is mandatory for the application to work properly**

   - Get your free API key from [Dune Analytics](https://dune.com)
   - Open `meteora-tracker.html` in a text editor
   - Find this line (around line 157):
   ```javascript
   const options = {method: 'GET', headers: {'X-DUNE-API-KEY': 'your dune api'}};
   ```
   - Replace `'your dune api'` with your actual API key:
   ```javascript
   const options = {method: 'GET', headers: {'X-DUNE-API-KEY': 'dqkFw8N3_your_actual_key_here'}};
   ```

3. **Open Application**
   - Double-click `meteora-tracker.html` to open in browser
   - Or serve via local web server for better performance

4. **Verify Setup**
   - Open the application in your browser
   - Click "Load Wallet Data" button
   - Check browser console (F12) for any API errors
   - If successful, you should see wallet data loading

## 📖 Usage Guide

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
        "wallet": "HmnoD2K1A4J6g8cYPuo3pZoMzx96qRqvxCJmfL26Airx"
      }
    ]
  }
}
```

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
