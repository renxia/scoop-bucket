# Scoop Bucket 项目指南

## 项目概述

这是一个为 [Scoop](https://scoop.sh) 打造的个人软件包仓库（bucket），用于在 Windows 上通过命令行安装软件。项目使用 PowerShell 脚本进行自动化管理，包含 CI/CD 流水线和 GitHub Actions 工作流。

### 项目结构

```
scoop-bucket/
├── bucket/              # 软件包清单文件（JSON 格式）
├── deprecated/          # 已弃用的软件包清单
├── bin/                 # 自动化脚本
├── .github/workflows/   # GitHub Actions 工作流
├── scripts/             # 其他脚本
├── Scoop-Bucket.Tests.ps1  # 测试脚本
└── app-name.json.template # 新软件包清单模板
```

## 核心组件

### 1. 软件包清单（bucket/）

每个软件包都有对应的 JSON 文件，例如：
- `7zip@21.03+zs.json` - 带 Zstandard 支持的 7-Zip
- `fusion360.json` - Fusion 360
- `notepad--.json` - Notepad--

清单文件包含版本、描述、下载地址、校验和、安装脚本等信息。

### 2. 自动化脚本（bin/）

- `auto-pr.ps1` - 自动生成 Pull Request
- `checkver.ps1` - 检查软件包版本更新
- `checkhashes.ps1` - 验证下载文件的校验和
- `checkurls.ps1` - 检查下载链接有效性
- `formatjson.ps1` - 格式化 JSON 文件
- `test.ps1` - 运行测试套件

### 3. CI/CD 流水线

- **CI 流水线** (`.github/workflows/ci.yml`): 每次推送或 PR 时运行测试
- **Excavator 流水线** (`.github/workflows/excavator.yml`): 每 8 小时自动检查并更新软件包版本

## 使用方法

### 安装软件包

1. 添加仓库：
   ```powershell
   scoop bucket add rsb https://github.com/renxia/scoop-bucket
   ```

2. 安装软件包：
   ```powershell
   scoop install 7zip
   ```

### 开发和维护

#### 添加新软件包

1. 复制 `app-name.json.template` 为 `软件名.json`
2. 填写必要信息：
   - `version`: 软件版本
   - `description`: 软件描述
   - `homepage`: 官方网站
   - `license`: 许可证信息
   - `architecture`: 不同架构的下载链接和校验和
   - `bin`: 可执行文件路径
3. 运行格式化：
   ```powershell
   .\bin\formatjson.ps1
   ```
4. 提交并推送

#### 更新软件包

1. 检查版本更新：
   ```powershell
   .\bin\checkver.ps1
   ```

2. 验证下载链接和校验和：
   ```powershell
   .\bin\checkurls.ps1
   .\bin\checkhashes.ps1
   ```

3. 提交更改

#### 运行测试

```powershell
.\bin\test.ps1
```

## 开发约定

### JSON 格式规范

- 使用 `.\bin\formatjson.ps1` 格式化所有 JSON 文件
- 确保所有必需字段都已填写
- 使用 SHA256 校验和

### 命名约定

- 软件包文件名：`软件名.json`
- 版本号：使用 `@` 符号分隔，如 `7zip@21.03+zs`
- 32 位架构：`32bit`
- 64 位架构：`64bit`
- ARM64 架构：`arm64`

### 版本管理

- 使用 `checkver` 和 `autoupdate` 字段自动检查更新
- 在 `checkver` 中指定版本检查 URL 和正则表达式
- 在 `autoupdate` 中指定自动更新的下载 URL

### 提交规范

- 提交信息应清晰描述更改内容
- 修复版本更新问题时，使用 `fix:` 前缀
- 添加新软件包时，使用 `add:` 前缀

## 常用命令

```powershell
# 格式化所有 JSON 文件
.\bin\formatjson.ps1

# 检查版本更新
.\bin\checkver.ps1

# 验证下载链接
.\bin\checkurls.ps1

# 验证校验和
.\bin\checkhashes.ps1

# 运行测试
.\bin\test.ps1

# 自动生成 PR
.\bin\auto-pr.ps1
```

## GitHub Actions

### CI 流水线

- **触发条件**：推送或 PR 到 main/master 分支
- **测试环境**：Windows Latest
- **测试内容**：PowerShell 和 Windows PowerShell

### Excavator 流水线

- **触发条件**：每 8 小时自动运行，或手动触发
- **功能**：自动检查软件包更新并创建 PR
- **配置**：通过 GitHub Actions 自动执行

## 注意事项

1. **大陆用户**：项目提供了镜像地址，推荐使用 `https://gh-proxy.org` 加速下载。
2. **使用镜像地址重试**：开发过程中设计获取 github api 失败时，应尝试以镜像地址为前缀重试。如：`curl https://gh-proxy.org/https://api.github.com/repos/iOfficeAI/AionUi/releases/latest`
3. **数据处理程序**：本地运行环境已安装 `Node.js` 和 `bun`。**所有涉及数据和文件处理的脚本程序，应优先选择使用它们。**
4. **校验和**：必须使用 SHA256 校验和，确保下载文件的完整性
5. **下载链接**：优先使用 GitHub Releases 或官方下载地址
6. **许可证**：正确填写许可证信息，包括标识符和 URL
7. **测试**：在提交前运行测试确保所有软件包清单有效

## 相关资源

- [Scoop 官方文档](https://scoop.sh/docs/)
- [App Manifests](https://github.com/ScoopInstaller/Scoop/wiki/App-Manifests)
- [Contributing Guide](https://github.com/ScoopInstaller/.github/blob/main/.github/CONTRIBUTING.md)
- [Scoop Bucket 模板](https://github.com/ScoopInstaller/Scoop-Bucket-Template)
