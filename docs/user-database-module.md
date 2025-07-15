# 👥 用户数据库模块

## 🎯 模块概述
用户数据库模块提供人脸用户信息的存储、查询、管理功能，支持用户注册、识别、数据持久化等操作。

## 📖 API概览

| 方法 | 功能 | 返回 |
|------|------|------|
| `registerFaceFromBitmap(Bitmap bitmap)` | 注册用户人脸 | boolean |
| `recognizeFaceFromBitmap(Bitmap bitmap)` | 识别用户人脸 | boolean |
| `saveFaceDatabase(String path)` | 保存数据库 | boolean |
| `loadFaceDatabase(String path)` | 加载数据库 | boolean |
| `getFaceCount()` | 获取用户数量 | int |
| `setRecognitionMode(boolean enabled)` | 设置识别模式 | boolean |
| `getUserList()` | 获取用户列表 | String[] |
| `getUserAvatarPath(String userId)` | 获取用户头像路径 | String |
| `getUserName(String userId)` | 获取用户姓名 | String |
| `updateUserName(String userId, String newName)` | 更新用户姓名 | boolean |
| `loadUserAvatar(String userId)` | 加载用户头像 | Bitmap |
| `deleteUser(String userId)` | 删除用户 | boolean |

## 🚀 快速使用

```java
// 1. 注册用户
KYCFaceSDK sdk = new KYCFaceSDK();
boolean registered = sdk.registerFaceFromBitmap(userBitmap);

// 2. 识别用户
boolean recognized = sdk.recognizeFaceFromBitmap(testBitmap);

// 3. 获取用户列表
String[] userList = sdk.getUserList();

// 4. 保存数据库
boolean saved = sdk.saveFaceDatabase("/sdcard/face_db.dat");
```

## 📖 API详细说明

### registerFaceFromBitmap(Bitmap bitmap)
**功能：** 将图像中的人脸注册为新用户
**参数：** bitmap - 包含人脸的图像
**返回：** boolean - true注册成功，false注册失败

```java
Bitmap userPhoto = getUserPhoto();
boolean success = sdk.registerFaceFromBitmap(userPhoto);

if (success) {
    Log.i("KYC", "用户注册成功");
    
    // 获取当前用户数量
    int count = sdk.getFaceCount();
    Log.i("KYC", "当前用户总数: " + count);
} else {
    Log.e("KYC", "用户注册失败");
}
```

### recognizeFaceFromBitmap(Bitmap bitmap)
**功能：** 识别图像中的人脸用户
**参数：** bitmap - 待识别的图像
**返回：** boolean - true识别成功，false识别失败

```java
Bitmap testPhoto = getCameraPhoto();
boolean recognized = sdk.recognizeFaceFromBitmap(testPhoto);

if (recognized) {
    Log.i("KYC", "用户识别成功");
} else {
    Log.i("KYC", "用户识别失败或未找到匹配用户");
}
```

### saveFaceDatabase(String path)
**功能：** 保存用户数据库到文件
**参数：** path - 保存路径
**返回：** boolean - true保存成功，false保存失败

```java
String dbPath = getExternalFilesDir(null) + "/face_database.dat";
boolean saved = sdk.saveFaceDatabase(dbPath);

if (saved) {
    Log.i("KYC", "数据库保存成功: " + dbPath);
} else {
    Log.e("KYC", "数据库保存失败");
}
```

### loadFaceDatabase(String path)
**功能：** 从文件加载用户数据库
**参数：** path - 数据库文件路径
**返回：** boolean - true加载成功，false加载失败

```java
String dbPath = getExternalFilesDir(null) + "/face_database.dat";
boolean loaded = sdk.loadFaceDatabase(dbPath);

if (loaded) {
    int count = sdk.getFaceCount();
    Log.i("KYC", "数据库加载成功，用户数量: " + count);
} else {
    Log.e("KYC", "数据库加载失败");
}
```

### getFaceCount()
**功能：** 获取当前数据库中的用户数量
**返回：** int - 用户数量

```java
int userCount = sdk.getFaceCount();
Log.i("KYC", "当前注册用户数: " + userCount);

if (userCount == 0) {
    showEmptyDatabaseHint();
} else {
    showUserListView();
}
```

### setRecognitionMode(boolean enabled)
**功能：** 设置识别模式开关
**参数：** enabled - true开启识别，false关闭识别
**返回：** boolean - true设置成功，false设置失败

```java
// 开启识别模式
boolean enabled = sdk.setRecognitionMode(true);

if (enabled) {
    Log.i("KYC", "识别模式已开启");
} else {
    Log.e("KYC", "识别模式开启失败");
}
```

### getUserList()
**功能：** 获取所有注册用户的ID列表
**返回：** String[] - 用户ID数组

```java
String[] userIds = sdk.getUserList();

if (userIds != null && userIds.length > 0) {
    Log.i("KYC", "获取到" + userIds.length + "个用户");
    
    for (String userId : userIds) {
        String userName = sdk.getUserName(userId);
        Log.i("KYC", "用户ID: " + userId + ", 姓名: " + userName);
    }
} else {
    Log.i("KYC", "数据库中没有注册用户");
}
```

### getUserAvatarPath(String userId)
**功能：** 获取指定用户的头像文件路径
**参数：** userId - 用户ID
**返回：** String - 头像文件路径

```java
String avatarPath = sdk.getUserAvatarPath("user001");

if (avatarPath != null && !avatarPath.isEmpty()) {
    Log.i("KYC", "用户头像路径: " + avatarPath);
    
    // 加载头像图片
    Bitmap avatar = BitmapFactory.decodeFile(avatarPath);
    if (avatar != null) {
        imageView.setImageBitmap(avatar);
    }
} else {
    Log.w("KYC", "用户头像不存在");
}
```

### getUserName(String userId)
**功能：** 获取指定用户的姓名
**参数：** userId - 用户ID
**返回：** String - 用户姓名

```java
String userName = sdk.getUserName("user001");

if (userName != null && !userName.isEmpty()) {
    Log.i("KYC", "用户姓名: " + userName);
    textView.setText(userName);
} else {
    Log.w("KYC", "用户姓名为空或用户不存在");
}
```

### updateUserName(String userId, String newName)
**功能：** 更新指定用户的姓名
**参数：** 
- userId - 用户ID
- newName - 新的姓名
**返回：** boolean - true更新成功，false更新失败

```java
boolean updated = sdk.updateUserName("user001", "张三");

if (updated) {
    Log.i("KYC", "用户姓名更新成功");
} else {
    Log.e("KYC", "用户姓名更新失败");
}
```

### loadUserAvatar(String userId)
**功能：** 加载指定用户的头像图片
**参数：** userId - 用户ID
**返回：** Bitmap - 头像图片，失败返回null

```java
Bitmap avatar = sdk.loadUserAvatar("user001");

if (avatar != null) {
    Log.i("KYC", "用户头像加载成功");
    imageView.setImageBitmap(avatar);
} else {
    Log.w("KYC", "用户头像加载失败或不存在");
    imageView.setImageResource(R.drawable.default_avatar);
}
```

### deleteUser(String userId)
**功能：** 删除指定用户及其所有数据
**参数：** userId - 用户ID
**返回：** boolean - true删除成功，false删除失败

```java
boolean deleted = sdk.deleteUser("user001");

if (deleted) {
    Log.i("KYC", "用户删除成功");
    refreshUserList();
} else {
    Log.e("KYC", "用户删除失败");
}
```

## 🔔 回调机制

数据库操作支持回调监听：

```java
sdk.setFaceRecognizeCallback(new KYCFaceSDK.FaceRecognizeCallback() {
    @Override
    public void onRegisterResult(boolean success, String message) {
        if (success) {
            Log.i("KYC", "用户注册成功: " + message);
        } else {
            Log.e("KYC", "用户注册失败: " + message);
        }
    }
    
    @Override
    public void onRecognizeResult(boolean success, String userId, float confidence) {
        if (success) {
            String userName = sdk.getUserName(userId);
            Log.i("KYC", "识别成功 - 用户: " + userName + ", 置信度: " + confidence);
        } else {
            Log.i("KYC", "识别失败");
        }
    }
});
```

## ⚠️ 注意事项

- 注册和识别功能需要先加载AI模型
- 建议定期调用saveFaceDatabase()保存数据
- 删除用户操作不可恢复，请谨慎使用
- 用户ID由系统自动生成，具有唯一性
