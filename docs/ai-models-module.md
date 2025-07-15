# 🤖 AI模型能力模块

## 🎯 模块概述
AI模型能力模块提供人脸识别的核心AI算法，包括人脸检测、关键点提取、特征提取等原子化能力。

## 📖 API概览

| 方法 | 功能 | 返回 |
|------|------|------|
| `loadModel(AssetManager mgr, int modelid, int cpugpu)` | 加载AI模型 | boolean |
| `detectFaces(Bitmap bitmap)` | 人脸检测 | String JSON |
| `extractLandmarks(Bitmap bitmap)` | 关键点提取 | String JSON |
| `extractFeatures(Bitmap bitmap)` | 特征提取 | float[] |

## 🚀 快速使用

```java
// 1. 加载模型
KYCFaceSDK sdk = new KYCFaceSDK();
boolean loaded = sdk.loadModel(getAssets(), 0, 1);

// 2. 检测人脸
String faces = sdk.detectFaces(bitmap);

// 3. 提取关键点
String landmarks = sdk.extractLandmarks(bitmap);

// 4. 提取特征
float[] features = sdk.extractFeatures(bitmap);
```

## 📖 API详细说明

### loadModel(AssetManager mgr, int modelid, int cpugpu)
**功能：** 加载AI模型到内存
**参数：** 
- mgr - AssetManager实例
- modelid - 模型ID（当前仅支持0）
- cpugpu - 设备类型（0=CPU，1=GPU）
**返回：** boolean - true加载成功，false加载失败

```java
// CPU模式加载
boolean success = sdk.loadModel(getAssets(), 0, 0);

// GPU模式加载（推荐，性能更好）
boolean success = sdk.loadModel(getAssets(), 0, 1);

if (success) {
    Log.i("KYC", "模型加载成功");
} else {
    Log.e("KYC", "模型加载失败");
}
```

### detectFaces(Bitmap bitmap)
**功能：** 检测图像中的人脸位置
**参数：** bitmap - 输入图像
**返回：** String - JSON格式的检测结果

```java
String result = sdk.detectFaces(bitmap);

// 解析JSON结果
try {
    JSONObject json = new JSONObject(result);
    JSONArray faces = json.getJSONArray("faces");
    
    for (int i = 0; i < faces.length(); i++) {
        JSONObject face = faces.getJSONObject(i);
        int x = face.getInt("x");
        int y = face.getInt("y");
        int width = face.getInt("width");
        int height = face.getInt("height");
        double confidence = face.getDouble("confidence");
        
        Log.i("KYC", String.format("人脸%d: (%d,%d) %dx%d 置信度%.2f", 
            i+1, x, y, width, height, confidence));
    }
} catch (JSONException e) {
    Log.e("KYC", "解析检测结果失败", e);
}
```

**返回JSON格式：**
```json
{
  "faces": [
    {
      "x": 100,
      "y": 150,
      "width": 200,
      "height": 250,
      "confidence": 0.99
    }
  ]
}
```

### extractLandmarks(Bitmap bitmap)
**功能：** 提取人脸98个关键点
**参数：** bitmap - 输入图像
**返回：** String - JSON格式的关键点结果

```java
String result = sdk.extractLandmarks(bitmap);

// 解析关键点
try {
    JSONObject json = new JSONObject(result);
    JSONArray landmarks = json.getJSONArray("landmarks");
    
    if (landmarks.length() > 0) {
        JSONObject firstFace = landmarks.getJSONObject(0);
        JSONArray points = firstFace.getJSONArray("points");
        
        Log.i("KYC", "检测到" + points.length() + "个关键点");
        
        // 获取第一个关键点坐标
        JSONObject point = points.getJSONObject(0);
        double x = point.getDouble("x");
        double y = point.getDouble("y");
        Log.i("KYC", "第一个关键点: (" + x + "," + y + ")");
    }
} catch (JSONException e) {
    Log.e("KYC", "解析关键点失败", e);
}
```

**返回JSON格式：**
```json
{
  "landmarks": [
    {
      "points": [
        {"x": 120.5, "y": 180.3},
        {"x": 125.1, "y": 182.7},
        ...
      ]
    }
  ]
}
```

### extractFeatures(Bitmap bitmap)
**功能：** 提取人脸512维特征向量
**参数：** bitmap - 输入图像
**返回：** float[] - 特征向量数组（512维）

```java
float[] features = sdk.extractFeatures(bitmap);

if (features != null && features.length == 512) {
    Log.i("KYC", "特征提取成功，维度: " + features.length);
    
    // 可用于人脸比对
    float similarity = compareFaceFeatures(features, storedFeatures);
    Log.i("KYC", "相似度: " + similarity);
} else {
    Log.e("KYC", "特征提取失败");
}
```

## ⚠️ 注意事项

- 使用AI功能前必须先调用`loadModel()`
- 建议图像分辨率控制在640-1280像素以保证性能
- GPU模式需要设备支持，不支持时会自动降级到CPU
- 特征向量可用于1:1比对或1:N识别
