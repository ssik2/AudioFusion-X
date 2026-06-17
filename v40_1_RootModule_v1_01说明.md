# AudioFusion V40.1 Root Module v1.01 升级说明

## 核心变化

Root 模块从旧 v1 系列升级为 v1.01。模块不再是单纯等待配置的旧脚本，也不再脱离 App 独立乱改系统音频链路。

## 模块必须依靠 App

v1.01 的设计是 App 驱动型 Root Engine：

- App 写入 `/data/adb/audiofusion/config.json`
- App 开启 Root Engine，即 `audioEngine = 2`
- App 开启全局增强，即 `globalEnabled = true`
- 模块检测 App 包 `com.audiofusion` 存在
- 模块确认 Speaker Only 策略存在

只有这些条件全部满足，模块才激活完整音效处理链。

## 完整链路

模块负责：

- systemless 媒体链路准备
- deep-buffer/offload 绕过，避免媒体流绕开 v38/v40 已成功挂载的 App AudioEffect/DynamicsProcessing 链
- DP / MBC / Limiter / Session / Root Active 状态输出
- Root WatchDog
- 配置变更自动 reload
- App 卸载、App 关闭、切换非 Root Engine 时自动 release

App 负责：

- EQ 参数
- 频响/目标曲线
- Adaptive Bass Engine
- Dynamic Loudness Engine
- Smart Limiter 参数
- Speaker Only 与媒体范围控制

## 不回退原则

本次没有回退 v38/v40 已经能正常挂载系统并处理音效的 Basic / DynamicsProcessing 链。Root 模块只增强媒体链路和验证恢复，不替换掉 App 内已生效的处理链。

## 版本标识

- module.prop: `version=v1.01`
- `versionCode=101`
- status.json: `moduleVersion=1.01`
- App 配置: `requiredModuleVersion=1.01`
