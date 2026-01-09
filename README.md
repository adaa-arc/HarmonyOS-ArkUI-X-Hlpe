# HarmonyOS-ArkUI-X-Hlpe(ArkUI-X C++14 / HarmonyOS 6.0.1)
解决一些新建的鸿蒙ArkUI-X项目一运行就报错的一些方法

要在新项目中通过编译并运行，请直接修改以下 **4 个文件**。

### 1. 解决依赖下载报错
**新建文件**：在项目根目录新建 `.npmrc`
**内容**：
```ini
registry=https://repo.huaweicloud.com/repository/npm/
@ohos:registry=https://repo.harmonyos.com/npm/
```
> **作用**：分离镜像源，普通包走华为云，@ohos 包走鸿蒙官方源。

---

### 2. 解决构建报错 (nodesEvaluatedInternal)
**修改文件**：`hvigor/hvigor-config.json5`
**修改内容**：将插件版本改为 `4.21.2`
```json5
"dependencies": {
    "@ohos/hvigor-ohos-arkui-x-plugin": "4.21.2"  // 原为 4.22.2 或 rc 版本
}
```

---

### 3. 解决 SDK 版本警告
**修改文件**：`build-profile.json5` (根目录下)
**修改内容**：在 products 的 default 配置中补充 sdk 版本
```json5
"products": [
  {
    "name": "default",
    // ...
    "compileSdkVersion": "6.0.1(21)",  // 新增这一行
    "compatibleSdkVersion": "6.0.1(21)",
    "targetSdkVersion": "6.0.1(21)",
    // ...
  }
]
```

---

### 4. 解决安装报错 (install parse native so failed)
**修改文件**：`entry/build-profile.json5` (entry 模块下)
**修改内容**：在 externalNativeOptions 中增加 `abiFilters`
```json5
"externalNativeOptions": {
    "path": "./src/main/cpp/CMakeLists.txt",
    "arguments": "",
    "cppFlags": "--std=c++14",
    // 新增以下配置，支持真机(arm)和模拟器(x86)
    "abiFilters": [
        "arm64-v8a",
        "x86_64"
    ]
}
```

---

### 最后一步
修改完以上内容后：
1. 点击 DevEco Studio 顶部的 **File > Sync Project with Hvigor Files**。
2. 点击 **Run** 运行即可。
