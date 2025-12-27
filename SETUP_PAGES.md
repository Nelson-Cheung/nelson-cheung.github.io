# 解决 "Get Pages site failed" 错误

## 问题说明

如果你看到这个错误：
```
Error: Get Pages site failed. Please verify that the repository has Pages enabled and configured to build using GitHub Actions
```

这说明 GitHub Pages 还没有在仓库设置中启用。

## 解决步骤

### 第一步：启用 GitHub Pages

1. 进入你的 GitHub 仓库页面
2. 点击顶部的 **Settings**（设置）标签
3. 在左侧菜单中，找到并点击 **Pages**
4. 在 **Build and deployment** 部分：
   - **Source**: 选择 **GitHub Actions**（重要：必须选择这个）
   - **Branch**: 不需要设置（使用 GitHub Actions 时自动处理）
5. 点击页面底部的 **Save** 保存设置

### 第二步：验证设置

启用后，你应该看到：
- 页面顶部显示 "Your site is live at https://..."
- **Build and deployment** 部分显示 "GitHub Actions" 作为源

### 第三步：触发部署

1. 如果 workflow 文件已经存在，可以：
   - 推送到 `main` 分支（会自动触发）
   - 或者在 **Actions** 标签页手动运行 workflow

2. 进入 **Actions** 标签页，查看 workflow 运行状态
3. 等待部署完成（通常需要 1-2 分钟）

### 第四步：访问网站

部署完成后，你的网站将在以下地址可用：
- 用户/组织页面：`https://nelson-cheung.github.io/`
- 项目页面：`https://nelson-cheung.github.io/仓库名/`

## 如果仍然失败

### 检查权限

确保你的 GitHub 账户有仓库的管理权限：
1. 进入仓库 **Settings** → **Collaborators and teams**
2. 确认你的账户有 **Admin** 或 **Maintain** 权限

### 检查仓库类型

- 如果是私有仓库，需要 GitHub Pro 或更高版本才能使用 GitHub Pages
- 公开仓库可以免费使用 GitHub Pages

### 检查 workflow 文件

确保 `.github/workflows/hugo.yml` 文件存在且格式正确：
- 文件路径必须正确：`.github/workflows/hugo.yml`
- YAML 语法必须正确（注意缩进）

### 查看详细错误信息

1. 进入 **Actions** 标签页
2. 点击失败的 workflow 运行
3. 查看详细的错误日志
4. 根据错误信息进行相应的修复

## 备选方案：手动部署到 gh-pages 分支

如果 GitHub Actions 仍然有问题，可以使用手动部署：

1. 在仓库 **Settings** → **Pages** 中，选择 **Deploy from a branch**
2. 选择 **gh-pages** 分支和 **/ (root)** 目录
3. 然后运行：

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
git push -u origin gh-pages --force
```

