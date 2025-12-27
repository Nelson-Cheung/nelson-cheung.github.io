# 解决环境保护规则错误

## 错误信息

如果你看到这个错误：
```
Branch "main" is not allowed to deploy to github-pages due to environment protection rules.
```

这是因为 `github-pages` 环境有保护规则限制了部署分支。

## 解决方法

### 方法 1：移除环境保护规则（推荐）

1. 进入你的 GitHub 仓库
2. 点击 **Settings**（设置）
3. 在左侧菜单中找到 **Environments**
4. 点击 **github-pages** 环境
5. 在 **Deployment branches** 部分：
   - 确保选择了 **All branches**（所有分支）
   - 或者添加 `main` 分支到允许列表
6. 如果看到 **Required reviewers** 或其他保护规则，可以暂时移除（对于个人项目通常不需要）
7. 点击 **Save protection rules** 保存

### 方法 2：修改环境配置

如果方法 1 不可用，可以：

1. 进入 **Settings** → **Environments**
2. 如果 `github-pages` 环境不存在或配置有问题，删除它
3. 然后重新运行 workflow，GitHub 会自动创建环境
4. 或者手动创建环境，选择 **All branches**

### 方法 3：使用简化的 workflow（备选）

如果环境保护规则无法修改，可以使用以下简化版本：

```yaml
# 这个版本不需要环境配置
deploy:
  runs-on: ubuntu-latest
  needs: build
  steps:
    - name: Deploy to GitHub Pages
      uses: actions/deploy-pages@v4
```

但是通常建议使用方法 1 来正确配置环境。

## 注意事项

- **环境保护规则**通常用于团队项目，防止意外部署
- 对于个人项目，通常不需要这些保护规则
- 如果仓库是公开的，任何人都会看到环境名称，但只有有权限的人才能修改规则

## 验证修复

修复后：

1. 推送代码到 `main` 分支
2. 进入 **Actions** 标签页
3. 查看 workflow 运行状态
4. 如果成功，你应该看到部署完成的绿色标记
5. 网站将在几分钟内在 `https://nelson-cheung.github.io/` 可用

