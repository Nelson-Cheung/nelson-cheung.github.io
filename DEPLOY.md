# GitHub Pages 部署指南

## 部署步骤

### 1. 准备 GitHub 仓库

确保你的代码已经推送到 GitHub 仓库，仓库名应该是 `nelson-cheung.github.io`（如果使用用户/组织页面）或任意名称（如果使用项目页面）。

### 2. 配置 GitHub Pages（重要！）

**必须先手动启用 GitHub Pages，然后 workflow 才能工作：**

1. 进入你的 GitHub 仓库
2. 点击 **Settings**（设置）
3. 在左侧菜单中找到 **Pages**
4. 在 **Build and deployment** 部分：
   - **Source**: 选择 **GitHub Actions**（重要：必须选择这个选项）
5. 点击 **Save** 保存设置

**注意：**
- 如果看到 "Your site is ready to be published" 的提示，说明 Pages 还没有启用
- 必须选择 **GitHub Actions** 作为源，而不是 branch 部署
- 如果仓库名不是 `nelson-cheung.github.io`，网站地址会是 `https://nelson-cheung.github.io/仓库名/`

### 3. 使用 GitHub Actions（推荐）

我们已经创建了 `.github/workflows/hugo.yml` 文件，它会自动：
- 在每次推送到 `main` 分支时构建和部署
- 使用 Hugo 0.152.2 版本
- 自动优化和压缩资源

**使用方法：**
1. 确保代码已推送到 GitHub
2. 在仓库设置中启用 GitHub Actions
3. 推送到 `main` 分支后，GitHub Actions 会自动构建和部署

### 4. 手动部署（备选方案）

如果你想手动部署，可以运行：

```bash
# 构建站点
hugo --minify

# 将 public 目录推送到 gh-pages 分支
cd public
git init
git add .
git commit -m "Deploy to GitHub Pages"
git branch -M gh-pages
git remote add origin https://github.com/nelson-cheung/nelson-cheung.github.io.git
git push -u origin gh-pages
```

### 5. 检查 baseURL 配置

确保 `config/_default/config.yaml` 中的 `baseURL` 正确：

```yaml
baseURL: 'https://nelson-cheung.github.io/'
```

如果是项目页面（不是用户/组织页面），baseURL 应该是：
```yaml
baseURL: 'https://nelson-cheung.github.io/仓库名/'
```

### 6. 访问你的网站

部署完成后，你的网站将在以下地址可用：
- 用户/组织页面：`https://nelson-cheung.github.io/`
- 项目页面：`https://nelson-cheung.github.io/仓库名/`

## 故障排除

### 如果 GitHub Actions 失败

1. 检查 Actions 标签页中的错误信息
2. 确保 Hugo 版本兼容
3. 检查配置文件是否有语法错误

### 如果网站显示 404

1. 确认 baseURL 配置正确
2. 检查 GitHub Pages 设置中的源分支
3. 等待几分钟让 DNS 更新

### 如果样式或资源加载失败

1. 检查 `baseURL` 是否正确（必须以 `/` 结尾）
2. 确保所有资源路径使用相对路径或正确的 baseURL

## 本地测试

在部署前，可以在本地测试：

```bash
# 使用生产环境配置构建
hugo --minify

# 或者使用服务器预览
hugo server
```

