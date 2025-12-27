# 检查部署问题的步骤

## 当前问题

在隐身模式下访问 `https://nelson-cheung.github.io/` 仍然显示 README.md 而不是网站内容。

## 诊断步骤

### 1. 检查 GitHub Pages 设置

**必须执行以下步骤：**

1. 进入 GitHub 仓库
2. 点击 **Settings**（设置）
3. 在左侧菜单点击 **Pages**
4. **检查 "Build and deployment" 部分**：
   
   **错误配置（会导致显示 README.md）：**
   ```
   Source: Deploy from a branch
   Branch: main (或其他分支)
   Folder: / (root)
   ```
   
   **正确配置（应该这样设置）：**
   ```
   Source: GitHub Actions  ← 必须选择这个！
   ```
   
5. 如果当前是 "Deploy from a branch"，请：
   - 点击下拉菜单
   - 选择 **GitHub Actions**
   - 点击 **Save** 保存

### 2. 检查 Actions 运行状态

1. 进入仓库的 **Actions** 标签页
2. 查看左侧是否有 "Deploy Hugo site to Pages" workflow
3. 点击这个 workflow
4. 查看最近的运行记录：
   - 如果有运行记录，点击最新的运行
   - 查看是否成功（绿色 ✅）或失败（红色 ❌）
   - 如果失败，查看错误信息

### 3. 手动触发部署

如果 workflow 没有运行：

1. 进入 **Actions** 标签页
2. 在左侧选择 "Deploy Hugo site to Pages"
3. 点击右侧的 **Run workflow** 按钮
4. 选择分支（通常是 `main`）
5. 点击绿色的 **Run workflow** 按钮
6. 等待运行完成

### 4. 验证部署结果

部署成功后：

1. 等待 1-2 分钟
2. 再次访问 `https://nelson-cheung.github.io/`
3. 应该看到你的网站（头像、姓名、社交图标等）

### 5. 如果仍然显示 README.md

#### 检查 1：查看 Actions 日志

1. 进入 **Actions** 标签页
2. 点击最新的 workflow 运行
3. 展开 "Deploy to GitHub Pages" 步骤
4. 查看是否有错误信息

#### 检查 2：验证 workflow 文件

确保 `.github/workflows/hugo.yml` 文件：
- 存在于仓库中
- 已提交并推送到 GitHub
- 格式正确（YAML 语法）

#### 检查 3：检查仓库设置

1. Settings → Pages
2. 确认 **Source** 是 **GitHub Actions**
3. 如果看到 "Your site is live at..." 但 URL 指向的网站显示 README，说明部署源设置错误

### 6. 临时解决方案：使用 gh-pages 分支

如果 GitHub Actions 仍然有问题，可以临时使用分支部署：

1. Settings → Pages
2. Source: 选择 **Deploy from a branch**
3. Branch: 选择 `gh-pages`
4. Folder: `/ (root)`
5. 然后手动将 `public` 目录的内容推送到 `gh-pages` 分支

```bash
# 构建
hugo --minify

# 部署到 gh-pages 分支
cd public
git init
git add .
git commit -m "Deploy to GitHub Pages"
git branch -M gh-pages
git remote add origin https://github.com/nelson-cheung/nelson-cheung.github.io.git
git push -u origin gh-pages --force
```

## 常见问题

### Q: 为什么显示 README.md？

**A:** 因为 GitHub Pages 的 Source 设置为 "Deploy from a branch"，GitHub 会显示仓库根目录的内容（包括 README.md），而不是 GitHub Actions 部署的网站。

### Q: 如何确认 GitHub Actions 是否成功部署？

**A:** 
1. Actions 标签页显示绿色 ✅
2. Settings → Pages 显示 "Your site is live at..."
3. 访问网站应该看到 Hugo 生成的内容，而不是 README.md

### Q: 部署成功后多久能看到更新？

**A:** 通常 1-2 分钟，有时可能需要 5-10 分钟。

## 最终检查清单

- [ ] Settings → Pages → Source = **GitHub Actions**
- [ ] Actions 标签页有 workflow 运行记录
- [ ] 最新的 workflow 运行显示成功（绿色 ✅）
- [ ] 等待了至少 2 分钟
- [ ] 使用隐身模式访问仍然显示 README.md（说明 Source 设置错误）

**如果所有步骤都正确但仍然显示 README.md，最可能的原因是 GitHub Pages 的 Source 仍然设置为 "Deploy from a branch" 而不是 "GitHub Actions"。**

