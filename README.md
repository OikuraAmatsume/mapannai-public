# マップ案内 (MapAnNai) - 交互式地图编辑器

一个基于Next.js和Mapbox的交互式地图标记编辑平台，支持富文本内容编辑、坐标跳转、标记分类等功能。

## 🚀 快速开始

### 1. Mapbox 配置申请

#### 1.1 创建 Mapbox 账户
1. 访问 [Mapbox官网](https://www.mapbox.com/)
2. 注册新账户或登录现有账户
3. 进入 [Account页面](https://account.mapbox.com/)

#### 1.2 获取 Access Token
1. 在 Account 页面找到 "Access tokens" 部分
2. 创建新的 Public token（用于地图显示）
3. 创建新的 Secret token（用于 Dataset API）
4. 记录下这两个 token

#### 1.3 创建 Dataset
1. 访问 [Mapbox Datasets](https://studio.mapbox.com/datasets/)
2. 点击 "New dataset"
3. 选择 "Empty dataset"
4. 记录下 Dataset ID

### 2. AWS S3 配置

#### 2.1 创建 S3 存储桶
1. 登录 [AWS Console](https://console.aws.amazon.com/)
2. 进入 S3 服务
3. 创建新的存储桶，名称如：`mapannai`
4. 配置存储桶权限（允许公共读取）

#### 2.2 创建 IAM 用户
1. 进入 IAM 服务
2. 创建新用户，如：`mapannai-s3-user`
3. 附加 `AmazonS3FullAccess` 策略
4. 创建 Access Key 和 Secret Key

### 3. 编辑配置文件

#### 3.1 更新 config.ts
编辑 `src/lib/config.ts` 文件：

```typescript
export const config = {
    mapbox: {
        accessToken: 'pk.YOUR_PUBLIC_ACCESS_TOKEN', // 替换为您的 Public Token
        style: 'mapbox://styles/mapbox/satellite-streets-v12',
        dataset: {
            username: 'YOUR_MAPBOX_USERNAME', // 替换为您的 Mapbox 用户名
            secretAccessToken: 'sk.YOUR_SECRET_ACCESS_TOKEN', // 替换为您的 Secret Token
            datasetId: 'YOUR_DATASET_ID', // 替换为您的 Dataset ID
        },
    },
    aws: {
        s3: {
            accessKeyId: 'YOUR_AWS_ACCESS_KEY_ID',
            secretAccessKey: 'YOUR_AWS_SECRET_ACCESS_KEY',
            region: 'ap-northeast-1',
            bucket: 'mapannai', // 您的 S3 存储桶名称
        },
    },
    app: {
        name: 'マップ案内 - 交互式地图编辑器',
        version: '1.0.0',
        defaultCenter: {
            latitude: 35.452,
            longitude: 139.638,
        },
        defaultZoom: 14.09,
    },
    // 城市快速跳转配置
    cities: {
        kyoto: {
            name: '京都',
            coordinates: { longitude: 135.7681, latitude: 35.0116 },
            zoom: 14
        },
        osaka: {
            name: '大阪',
            coordinates: { longitude: 135.5022, latitude: 34.6937 },
            zoom: 14
        },
        yokohama: {
            name: '横滨',
            coordinates: { longitude: 139.6380, latitude: 35.452 },
            zoom: 14
        },
        // 您可以在这里添加更多城市
        tokyo: {
            name: '东京',
            coordinates: { longitude: 139.6917, latitude: 35.6895 },
            zoom: 11
        },
        nagoya: {
            name: '名古屋',
            coordinates: { longitude: 136.9066, latitude: 35.1815 },
            zoom: 11
        },
    },
} as const
```

#### 3.2 添加城市配置
在 `cities` 配置中添加您需要的城市：

```typescript
cities: {
    // 现有城市...
    yourCity: {
        name: '您的城市名',
        coordinates: { longitude: 经度, latitude: 纬度 },
        zoom: 缩放级别
    },
}
```

## 🚀 部署到 AWS Amplify

#### 创建 Amplify 应用
1. 登录 [AWS Amplify Console](https://console.aws.amazon.com/amplify/)
2. 点击 "New app" → "Host web app"
3. 选择 "GitHub" 或其他代码仓库
4. 连接您的代码仓库


### 基本功能

#### 1. 地图导航
- **平移**：鼠标拖拽
- **缩放**：鼠标滚轮或双指缩放
- **旋转**：按住 Ctrl + 鼠标拖拽

#### 2. 标记管理

##### 添加标记
1. 启用编辑模式（左侧边栏开关）
2. 点击地图空白处
3. 点击"添加"按钮
4. 填写标记信息：
   - 标题
   - 图标类型（活动/位置/酒店/购物）
   - 首图（可选）
   - 详细内容（使用富文本编辑器）

##### 编辑标记
1. 点击现有标记
2. 点击"编辑"按钮
3. 修改标记信息
4. 点击"保存"

##### 删除标记
1. 点击标记
2. 点击"删除"按钮
3. 确认删除

#### 3. 搜索功能

##### 标记搜索
1. 打开左侧边栏
2. 在搜索框中输入关键词
3. 点击搜索结果跳转到标记位置

##### 坐标跳转
1. 打开左侧边栏
2. 展开"坐标跳转"区域
3. 输入坐标（格式：纬度, 经度）
4. 点击"跳转"按钮

支持的坐标格式：
- `35.452, 139.638`
- `35.452 139.638`

#### 4. 城市快速跳转
- 点击右下角的城市按钮
- 地图自动跳转到对应城市

#### 5. 右侧边栏
- 点击标记后自动打开
- 显示标记的详细内容
- 支持滚动查看长内容

### 功能详情

#### 1. 标记分类
- **活动** 🎯：活动和娱乐场所
- **位置** 📍：一般地点标记
- **酒店** 🏨：住宿和酒店
- **购物** 🛍️：购物中心和商店

#### 2. 富文本编辑
- 支持标题、段落、列表
- 支持引用、图片
- 支持链接和格式化

#### 3. 数据同步
- 标记数据自动保存到 Mapbox Dataset
- 图片自动上传到 AWS S3
- 支持多人协作编辑


## 🤝 贡献指南

1. Fork 项目
2. 创建功能分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 打开 Pull Request

## 📞 支持

如果您遇到问题或有建议，请：

1. 查看 [Issues](../../issues) 页面
2. 创建新的 Issue
3. 联系项目维护者

---

**マップ案内** - 让地图编辑变得简单而强大！ 🗺️✨
