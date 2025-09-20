# MCP mcp-feedback-enhanced 修复指南

## 🔍 问题诊断

根据分析，您的 `mcp-feedback-enhanced` 无法正常启用是因为以下原因：

### 主要问题
1. **缺少 `realpath` 命令** - macOS 默认没有 GNU 版本的 `realpath` 命令
2. **Python 路径解析错误** - MCP 包的启动脚本无法正确解析 Python 解释器路径

### 错误信息
```
realpath: command not found
exec: /Users/bensn/Desktop/DDP 在途库存监控/python: cannot execute: No such file or directory
```

## 🛠️ 解决方案

### 方案一：安装 coreutils（推荐）

1. **安装 Homebrew**（如果未安装）：
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

2. **安装 coreutils**：
```bash
brew install coreutils
```

3. **重启 Cursor**，让 MCP 重新加载配置

### 方案二：修改 MCP 配置

如果不想安装 Homebrew，可以修改 MCP 配置使用绝对路径：

1. **备份当前配置**：
```bash
cp ~/.cursor/mcp.json ~/.cursor/mcp.json.backup
```

2. **修改配置文件** `~/.cursor/mcp.json`：
```json
{
  "mcpServers": {
    "mcp-feedback-enhanced": {
      "command": "/Library/Frameworks/Python.framework/Versions/3.12/bin/python3",
      "args": ["-m", "pip", "install", "--user", "mcp-feedback-enhanced", "&&", "/Library/Frameworks/Python.framework/Versions/3.12/bin/python3", "-m", "mcp_feedback_enhanced"],
      "timeout": 6000,
      "env": {
        "MCP_DESKTOP_MODE": "true",
        "PATH": "/Library/Frameworks/Python.framework/Versions/3.12/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Users/bensn/.local/bin"
      },
      "autoApprove": ["interactive_feedback"]
    }
  }
}
```

### 方案三：使用 Python 直接运行

1. **安装 mcp-feedback-enhanced**：
```bash
/Library/Frameworks/Python.framework/Versions/3.12/bin/python3 -m pip install --user mcp-feedback-enhanced
```

2. **修改 MCP 配置** `~/.cursor/mcp.json`：
```json
{
  "mcpServers": {
    "mcp-feedback-enhanced": {
      "command": "/Library/Frameworks/Python.framework/Versions/3.12/bin/python3",
      "args": ["-m", "mcp_feedback_enhanced"],
      "timeout": 6000,
      "env": {
        "MCP_DESKTOP_MODE": "true"
      },
      "autoApprove": ["interactive_feedback"]
    }
  }
}
```

## ✅ 验证步骤

### 1. 检查配置
```bash
cat ~/.cursor/mcp.json
```

### 2. 测试 Python 包
```bash
/Library/Frameworks/Python.framework/Versions/3.12/bin/python3 -m mcp_feedback_enhanced --help
```

### 3. 重启 Cursor
完全关闭 Cursor 应用程序，然后重新启动。

### 4. 查看 MCP 日志
在 Cursor 中，您可以通过以下方式查看 MCP 日志：
- 打开命令面板 (Cmd+Shift+P)
- 搜索 "MCP"
- 查看相关日志信息

## 🎉 修复状态

✅ **已完成修复！**

以下步骤已经成功执行：
1. ✅ 安装了 mcp-feedback-enhanced 包
2. ✅ 更新了 MCP 配置文件使用正确的 Python 路径
3. ✅ 验证了包可以正常运行

**当前配置状态：**
```json
{
  "mcpServers": {
    "mcp-feedback-enhanced": {
      "command": "/Library/Frameworks/Python.framework/Versions/3.12/bin/python3",
      "args": ["-m", "mcp_feedback_enhanced"],
      "timeout": 6000,
      "env": {
        "MCP_DESKTOP_MODE": "true",
        "PATH": "/Library/Frameworks/Python.framework/Versions/3.12/bin:/Users/bensn/Library/Python/3.12/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Users/bensn/.local/bin"
      },
      "autoApprove": ["interactive_feedback"]
    }
  }
}
```

**下一步：请重启 Cursor 以让更改生效！**

## 🔧 当前系统信息

- **操作系统**: macOS 20.6.0
- **Python 路径**: `/Library/Frameworks/Python.framework/Versions/3.12/bin/python3`
- **UV 版本**: 0.8.17
- **UVX 路径**: `/Users/bensn/.local/bin/uvx`

## 📋 故障排除

### 如果仍然无法工作：

1. **检查权限**：
```bash
ls -la ~/.cursor/mcp.json
chmod 600 ~/.cursor/mcp.json
```

2. **检查 Python 包安装**：
```bash
/Library/Frameworks/Python.framework/Versions/3.12/bin/python3 -m pip list | grep mcp
```

3. **清理缓存**：
```bash
rm -rf ~/.cache/uv/
```

4. **重新安装包**：
```bash
/Library/Frameworks/Python.framework/Versions/3.12/bin/python3 -m pip uninstall mcp-feedback-enhanced
/Library/Frameworks/Python.framework/Versions/3.12/bin/python3 -m pip install mcp-feedback-enhanced
```

## 📞 后续支持

如果按照以上步骤仍然无法解决问题，请提供以下信息：
1. 具体的错误日志
2. Cursor 版本信息
3. 执行步骤后的结果截图

---

*此指南基于您当前的系统配置和错误分析编写，应该能够解决 mcp-feedback-enhanced 无法启用的问题。*
