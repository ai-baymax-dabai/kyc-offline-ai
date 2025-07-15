# 🔄 Pipeline业务模块

## 🎯 模块概述
Pipeline业务模块提供人脸注册和识别的端到端业务流程，内部自动调用AI模型完成检测→关键点→特征提取的完整链路。

## 📖 API概览

| 方法 | 功能 | 返回 |
|------|------|------|
| `quickRegisterFace(Bitmap bitmap)` | 快速注册人脸 | boolean |
| `quickRecognizeFace(Bitmap bitmap)` | 快速识别人脸 | boolean |

## 🚀 快速使用

```java
// 1. 注册人脸
KYCFaceSDK sdk = new KYCFaceSDK();
boolean success = sdk.quickRegisterFace(userBitmap);

// 2. 识别人脸  
boolean found = sdk.quickRecognizeFace(testBitmap);
```

## 📖 API详细说明

### quickRegisterFace(Bitmap bitmap)
**功能：** 注册用户人脸到数据库
**参数：** bitmap - 包含人脸的图像
**返回：** boolean - true注册成功，false注册失败

```java
// 注册用户人脸
Bitmap userPhoto = getUserPhoto();
boolean success = sdk.quickRegisterFace(userPhoto);

if (success) {
    Log.i("KYC", "人脸注册成功");
} else {
    Log.e("KYC", "人脸注册失败，请确保图像中包含清晰人脸");
}
```

**内部流程：**
1. 检测人脸位置
2. 提取98个关键点
3. 生成512维特征向量
4. 保存到用户数据库

### quickRecognizeFace(Bitmap bitmap)
**功能：** 识别图像中的人脸
**参数：** bitmap - 待识别的图像
**返回：** boolean - true识别成功，false识别失败

```java
// 识别人脸
Bitmap testPhoto = getCameraPhoto();
boolean recognized = sdk.quickRecognizeFace(testPhoto);

if (recognized) {
    Log.i("KYC", "人脸识别成功");
} else {
    Log.e("KYC", "人脸识别失败或未找到匹配用户");
}
```

**内部流程：**
1. 检测人脸位置
2. 提取特征向量
3. 与数据库特征比对
4. 返回识别结果

## 🔔 回调机制

Pipeline方法支持异步回调：

```java
// 设置回调监听
sdk.setFaceRecognizeCallback(new KYCFaceSDK.FaceRecognizeCallback() {
    @Override
    public void onRegisterResult(boolean success, String message) {
        if (success) {
            showToast("注册成功: " + message);
        } else {
            showToast("注册失败: " + message);
        }
    }
    
    @Override  
    public void onRecognizeResult(boolean success, String userId, float confidence) {
        if (success) {
            showToast("识别成功，用户: " + userId + "，置信度: " + confidence);
        } else {
            showToast("识别失败");
        }
    }
});
```

## ⚠️ 注意事项

- Pipeline方法需要先调用`loadModel()`加载AI模型
- 建议图像分辨率不超过1280x720以保证性能
- 识别阈值默认为0.65，可通过其他接口调整
