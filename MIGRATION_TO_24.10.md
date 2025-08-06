# luci-app-easytier 迁移到 OpenWrt 24.10

本文档记录了从 OpenWrt 22.03 迁移到 24.10 的主要变更。

## 主要变更

### 1. 修复 LuCI API 兼容性问题
- **问题**: OpenWrt 24.10 中 `luci.util.pcdata()` 被弃用，替换为 `luci.xml.pcdata()`
- **解决方案**: 全局替换所有 `luci.util.pcdata` 为 `luci.xml.pcdata`
- **影响文件**:
  - `luasrc/model/cbi/easytier.lua` - 所有信息显示功能
  - `luasrc/view/easytier/other_dvalue.htm` - 值显示模板

### 2. 更新 Makefile
- **变更**: 移除 `+luci-compat` 依赖
- **原因**: OpenWrt 24.10 不再需要 luci-compat 包来提供兼容性支持
- **修改**:
  ```makefile
  # 之前
  LUCI_DEPENDS:=+kmod-tun +luci-compat
  
  # 现在
  LUCI_DEPENDS:=+kmod-tun
  ```

### 3. 更新文档
- 更新 README.md 中的编译说明，使用 24.10 的 SDK
- 删除不再需要的 pcdata 问题解决方案
- 更新版本说明从 22.03 到 24.10

## 测试验证

迁移完成后请验证以下功能：
1. 插件能正常安装和启动
2. Web 界面显示正常，无 JavaScript 错误
3. 所有信息显示功能正常工作（节点信息、路由信息等）
4. 日志查看功能正常
5. 配置文件上传功能正常

## 兼容性说明

此版本专为 OpenWrt 24.10 设计，不再兼容 22.03。如需同时支持两个版本，建议：
1. 创建独立的分支
2. 使用条件编译来处理 API 差异
3. 在 Makefile 中根据版本选择合适的依赖

## 已知问题

无已知问题。所有在 22.03 中工作的功能在 24.10 中都应正常工作。

## 迁移日期

2025年8月6日
