# Google Maps API 配置说明

## 功能概述

Map页面已集成Google Maps JavaScript API和Places API，支持以下功能：
- ✅ 交互式地图显示
- ✅ 地点搜索和自动完成
- ✅ 充电站标记显示
- ✅ 用户定位功能
- ✅ 信息窗口显示

## 配置步骤

### 1. 获取Google Maps API密钥

1. 访问 [Google Cloud Console](https://console.cloud.google.com/)
2. 创建新项目或选择现有项目
3. 启用以下API：
   - **Maps JavaScript API**
   - **Places API**
   - **Geocoding API**（可选，用于地址解析）

4. 创建API密钥：
   - 进入"凭据"页面
   - 点击"创建凭据" → "API密钥"
   - 复制生成的API密钥

5. （推荐）限制API密钥：
   - 点击创建的API密钥进行编辑
   - 在"API限制"中选择"限制密钥"
   - 仅选择上述启用的API
   - 在"应用限制"中设置HTTP引荐来源（可选，提高安全性）

### 2. 配置API密钥

打开 `map.html` 文件，找到第11行：

```html
<script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY&libraries=places&callback=initMap" async defer></script>
```

将 `YOUR_API_KEY` 替换为您的实际API密钥：

```html
<script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyYourActualAPIKeyHere&libraries=places&callback=initMap" async defer></script>
```

### 3. 测试配置

1. 保存文件
2. 在浏览器中打开页面
3. 如果配置正确，应该能看到Google Maps地图
4. 在搜索框中输入地点名称，应该能看到自动完成建议

## 功能说明

### 地点搜索

- 在顶部搜索框输入地点名称
- Google Places Autocomplete会自动提供建议
- 选择地点后，地图会定位到该位置并显示标记

### 充电站标记

地图上显示三个示例充电站：
- **绿色标记**：可用充电桩数量 > 5
- **黄色标记**：可用充电桩数量 2-5
- **红色标记**：可用充电桩数量 < 2

点击标记可查看详细信息。

### 定位功能

点击右上角的定位按钮（十字图标）：
- 请求浏览器位置权限
- 定位成功后，地图会移动到您的位置
- 显示蓝色标记表示您的位置

### 信息窗口

点击地图标记或搜索地点后：
- 显示信息窗口，包含地点详细信息
- 底部卡片会同步更新显示的信息

## 费用说明

Google Maps API采用按使用量计费：
- **Maps JavaScript API**：每月前28,000次地图加载免费
- **Places API**：每月前$200额度免费（约17,000次请求）
- 超出免费额度后按使用量收费

详细价格请参考：[Google Maps Platform Pricing](https://mapsplatform.google.com/pricing/)

## 故障排查

### 问题1：地图不显示，显示"Failed to load map"

**原因**：API密钥未配置或无效

**解决方法**：
1. 检查API密钥是否正确配置
2. 确认API密钥已启用Maps JavaScript API
3. 检查浏览器控制台是否有错误信息
4. 确认API密钥没有设置过严的限制

### 问题2：搜索框没有自动完成建议

**原因**：Places API未启用或API密钥限制

**解决方法**：
1. 确认已启用Places API
2. 检查API密钥限制设置
3. 查看浏览器控制台错误信息

### 问题3：定位功能不工作

**原因**：浏览器阻止了位置权限或HTTPS要求

**解决方法**：
1. 允许浏览器的位置权限
2. 如果使用本地文件，需要通过HTTP服务器访问（Google Maps API要求HTTPS或localhost）
3. 使用Live Server等工具运行项目

### 问题4：控制台显示"RefererNotAllowedMapError"

**原因**：API密钥的HTTP引荐来源限制设置过严

**解决方法**：
1. 在Google Cloud Console中编辑API密钥
2. 在"应用限制"中添加您的域名或设置为"无限制"（仅用于开发）

## 开发建议

### 本地开发

对于本地开发，建议：
1. 使用Live Server或类似工具运行项目
2. 在API密钥限制中添加 `localhost` 和 `127.0.0.1`
3. 使用开发环境的API密钥（不要在生产环境使用）

### 生产环境

对于生产环境：
1. 创建单独的API密钥
2. 设置严格的HTTP引荐来源限制
3. 启用API使用量监控和告警
4. 定期检查API使用情况

## 代码结构

主要功能在 `map.html` 的 `<script>` 标签中：

- `initMap()` - 初始化地图和Places Autocomplete
- `onPlaceChanged()` - 处理地点选择
- `addSampleStations()` - 添加示例充电站标记
- `showStationInfo()` - 显示充电站信息
- `locateUser()` - 获取用户位置
- `clearMarkers()` - 清除所有标记

## 自定义配置

### 修改默认位置

在代码中找到：
```javascript
const defaultLocation = { lat: 48.8566, lng: 2.3522 }; // Paris
```

修改为您想要的坐标。

### 添加更多充电站

在 `addSampleStations()` 函数中添加更多站点数据。

### 自定义地图样式

在 `initMap()` 函数中的 `styles` 数组中添加自定义样式。

## 相关资源

- [Google Maps JavaScript API文档](https://developers.google.com/maps/documentation/javascript)
- [Places API文档](https://developers.google.com/maps/documentation/places/web-service)
- [Places Autocomplete文档](https://developers.google.com/maps/documentation/javascript/places-autocomplete)

