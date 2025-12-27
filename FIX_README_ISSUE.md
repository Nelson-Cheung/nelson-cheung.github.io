# 解决显示 README.md 而不是网站内容的问题

## 问题说明

如果 GitHub Pages 显示的是 README.md 而不是你的 Hugo 网站，通常是因为：

1. **GitHub Pages 没有使用 GitHub Actions 部署**
2. **Workflow 部署失败**
3. **GitHub Pages 源设置错误**

## 解决步骤

### 第一步：检查 GitHub Pages 设置

1. 进入你的 GitHub 仓库
2. 点击 **Settings** → **Pages**
3. **重要**：在 **Build and deployment** 部分：
   - **Source** 必须选择 **GitHub Actions**（不是 "Deploy from a branch"）
4. 如果显示 "Your site is ready to be published"，说明还没有启用 GitHub Actions

### 第二步：检查 Workflow 运行状态

1. 进入仓库的 **Actions** 标签页
2. 查看最近的 workflow 运行：
   - ✅ 绿色 = 成功
   - ❌ 红色 = 失败（需要查看错误日志）
   - 🟡 黄色 = 运行中

### 第三步：如果 Workflow 失败

查看错误日志，常见问题：

1. **环境保护规则错误**：参考 `FIX_ENVIRONMENT.md`
2. **Hugo 构建错误**：检查配置文件语法
3. **权限问题**：确保有正确的权限

### 第四步：验证部署

部署成功后：

1. 等待 1-2 分钟让 GitHub Pages 更新
2. 访问 `https://nelson-cheung.github.io/`
3. 应该看到你的主页（头像、姓名、社交图标等），而不是 README.md

### 第五步：清除缓存

如果仍然显示 README.md：

1. 硬刷新浏览器（Ctrl+F5 或 Cmd+Shift+R）
2. 或者使用隐私模式访问
3. 检查是否访问了正确的 URL

## 常见原因

### 原因 1：GitHub Pages 源设置错误

**错误设置**：
- Source: Deploy from a branch
- Branch: main
- Folder: / (root)

**正确设置**：
- Source: **GitHub Actions**

### 原因 2：Workflow 没有运行

- 确保代码已推送到 `main` 分支
- 检查 `.github/workflows/hugo.yml` 文件是否存在
- 确保文件格式正确（YAML 语法）

### 原因 3：部署失败但 GitHub 显示默认 README

- 如果 GitHub Actions 部署失败，GitHub 可能会显示仓库的 README.md
- 检查 Actions 标签页中的错误信息

## 验证清单

- [ ] Settings → Pages → Source = **GitHub Actions**
- [ ] Actions 标签页显示 workflow 成功运行（绿色 ✅）
- [ ] 等待 1-2 分钟让部署生效
- [ ] 硬刷新浏览器清除缓存
- [ ] 访问正确的 URL（`https://nelson-cheung.github.io/`）

## 如果问题仍然存在

1. **检查 Actions 日志**：
   - 进入 Actions 标签页
   - 点击最新的 workflow 运行
   - 查看 "Build with Hugo" 和 "Deploy to GitHub Pages" 步骤的日志

2. **手动触发部署**：
   - 在 Actions 标签页
   - 点击 "Deploy Hugo site to Pages" workflow
   - 点击 "Run workflow" 按钮手动运行

3. **检查文件路径**：
   - 确保 `public/index.html` 文件存在
   - 确保 workflow 中的 `path: ./public` 正确

