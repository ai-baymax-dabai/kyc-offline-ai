# 📋 License管理模块

## 🎯 模块概述
License管理模块负责SDK授权验证，确保合法使用。

## 📖 API概览

| 方法 | 功能 | 返回 |
|------|------|------|
| `initializeLicense(String key)` | 初始化License | int状态码 |
| `isLicenseValid()` | 检查License有效性 | boolean |
| `getLicenseStatus()` | 获取License状态 | int状态码 |
| `getLicenseStatusMessage()` | 获取状态描述 | String消息 |
| `getLicenseRemainingDays()` | 获取剩余天数 | int天数 |
| `setLicense(String key)` | 设置License | int状态码 |
| `clearLicense()` | 清除License | void |

## 🚀 快速使用

```java
// 1. 初始化
KYCFaceSDK sdk = new KYCFaceSDK();
int result = sdk.initializeLicense("your_license_key");

// 2. 检查状态
if (sdk.isLicenseValid()) {
    // 可以使用SDK功能
} else {
    Log.e("KYC", sdk.getLicenseStatusMessage());
}
```

## 📖 API详细说明

### initializeLicense(String licenseKey)
**功能：** 初始化License验证
**参数：** licenseKey - License密钥字符串  
**返回：** int - 状态码（0=有效，1=无效，2=过期，3=包名不匹配）

```java
int status = sdk.initializeLicense("your_license_key");
if (status == 0) {
    Log.i("KYC", "License验证成功");
} else {
    Log.e("KYC", "License验证失败，状态码: " + status);
}
```

### isLicenseValid()
**功能：** 快速检查License是否有效
**返回：** boolean - true有效，false无效

```java
if (sdk.isLicenseValid()) {
    // 继续使用SDK
} else {
    // 停止使用，显示错误
}
```

### getLicenseStatus()
**功能：** 获取详细License状态
**返回：** int - 状态码

```java
int status = sdk.getLicenseStatus();
switch (status) {
    case 0: // 有效
        break;
    case 1: // 无效
        break;
    case 2: // 过期
        break;
    case 3: // 包名不匹配
        break;
}
```

### getLicenseStatusMessage()
**功能：** 获取状态的中文描述
**返回：** String - 状态描述

```java
String message = sdk.getLicenseStatusMessage();
Toast.makeText(this, message, Toast.LENGTH_SHORT).show();
```

### getLicenseRemainingDays()
**功能：** 获取License剩余有效天数
**返回：** int - 剩余天数

```java
int days = sdk.getLicenseRemainingDays();
if (days <= 7 && days > 0) {
    showRenewalReminder(days);
}
```

### setLicense(String licenseKey)
**功能：** 设置新的License（同initializeLicense）
**参数：** licenseKey - License密钥字符串
**返回：** int - 状态码

```java
int result = sdk.setLicense("new_license_key");
```

### clearLicense()
**功能：** 清除当前License
**返回：** void

```java
sdk.clearLicense(); // 清除License，SDK将无法使用
```

## ⚠️ 状态码说明

| 状态码 | 含义 | 解决方案 |
|--------|------|----------|
| 0 | License有效 | 正常使用 |
| 1 | License无效 | 检查License格式 |
| 2 | License过期 | 联系商务续费 |
| 3 | 包名不匹配 | 确认License包名 |
