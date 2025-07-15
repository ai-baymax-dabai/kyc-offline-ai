# 📚 KYC Face SDK API 总览

## 🎯 购买功能一览表

| 模块 | API数量 | 核心功能 | 文档链接 |
|------|---------|----------|----------|
| **🔐 许可证管理** | 4 | 授权验证、激活管理 | [license-module.md](./license-module.md) |
| **🤖 AI模型** | 4 | 人脸检测、特征提取 | [ai-models-module.md](./ai-models-module.md) |
| **📷 相机控制** | 5 | 相机操作、帧捕获 | [camera-control-module.md](./camera-control-module.md) |
| **👥 用户数据库** | 12 | 用户管理、数据持久化 | [user-database-module.md](./user-database-module.md) |
| **🔄 业务流程** | 2 | 端到端业务封装 | [pipeline-business-module.md](./pipeline-business-module.md) |

**总计：27个核心API**

## 🚀 快速集成路径

### 1️⃣ 基础集成 (5分钟)
```java
// 初始化SDK
KYCFaceSDK sdk = new KYCFaceSDK();

// 授权验证
boolean authorized = sdk.verifyLicense();

// 加载AI模型
boolean loaded = sdk.loadModel("/path/to/model");
```

### 2️⃣ 相机集成 (10分钟)
```java
// 开启相机
boolean opened = sdk.openCamera();

// 设置预览
sdk.setOutputWindow(surfaceView.getHolder().getSurface());

// 捕获帧
Bitmap frame = sdk.captureCurrentFrame();
```

### 3️⃣ 业务集成 (15分钟)
```java
// 注册用户
boolean registered = sdk.quickRegisterFace(bitmap);

// 识别用户
boolean recognized = sdk.quickRecognizeFace(bitmap);
```

## 📖 API分类详览

### 🔐 许可证管理模块
| API | 功能 |
|-----|------|
| `verifyLicense()` | 验证许可证有效性 |
| `loadLicense(String)` | 加载许可证文件 |
| `getLicenseInfo()` | 获取许可证信息 |
| `isLicenseValid()` | 检查许可证状态 |

### 🤖 AI模型模块
| API | 功能 |
|-----|------|
| `loadModel(String)` | 加载AI模型文件 |
| `detectFaces(Bitmap)` | 检测人脸位置 |
| `extractLandmarks(Bitmap)` | 提取人脸关键点 |
| `extractFeatures(Bitmap)` | 提取人脸特征 |

### 📷 相机控制模块
| API | 功能 |
|-----|------|
| `openCamera()` | 打开相机 |
| `closeCamera()` | 关闭相机 |
| `setOutputWindow(Surface)` | 设置预览窗口 |
| `captureCurrentFrame()` | 捕获当前帧 |
| `getCurrentFrameBytes()` | 获取帧字节数据 |

### 👥 用户数据库模块
| API | 功能 |
|-----|------|
| `registerFaceFromBitmap(Bitmap)` | 注册用户人脸 |
| `recognizeFaceFromBitmap(Bitmap)` | 识别用户人脸 |
| `saveFaceDatabase(String)` | 保存用户数据库 |
| `loadFaceDatabase(String)` | 加载用户数据库 |
| `getFaceCount()` | 获取用户数量 |
| `setRecognitionMode(boolean)` | 设置识别模式 |
| `getUserList()` | 获取用户列表 |
| `getUserAvatarPath(String)` | 获取用户头像路径 |
| `getUserName(String)` | 获取用户姓名 |
| `updateUserName(String, String)` | 更新用户姓名 |
| `loadUserAvatar(String)` | 加载用户头像 |
| `deleteUser(String)` | 删除用户 |

### 🔄 业务流程模块
| API | 功能 |
|-----|------|
| `quickRegisterFace(Bitmap)` | 一键注册用户 |
| `quickRecognizeFace(Bitmap)` | 一键识别用户 |

## 🎯 使用场景匹配

| 使用场景 | 推荐模块 | 核心API |
|----------|----------|---------|
| **门禁系统** | 业务流程 + 相机 | `quickRecognizeFace` + `openCamera` |
| **员工考勤** | 用户数据库 + AI模型 | `recognizeFaceFromBitmap` + `getUserList` |
| **身份验证** | AI模型 + 许可证 | `detectFaces` + `verifyLicense` |
| **用户管理** | 用户数据库 | `registerFaceFromBitmap` + 管理API |
| **实时监控** | 相机 + AI模型 | `captureCurrentFrame` + `detectFaces` |

## 🔔 集成建议

1. **新手推荐：** 先使用业务流程模块的2个API快速体验
2. **自定义需求：** 组合AI模型+相机+用户数据库模块
3. **企业级部署：** 完整使用所有5个模块的27个API

## 📞 技术支持

- **文档中心：** `/docs/` 目录下各模块详细文档
- **示例代码：** 每个模块文档都包含完整使用示例
- **API精度：** 所有API均与实际JNI实现100%匹配
