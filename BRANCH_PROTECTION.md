# 分支保护规则说明 (Branch Protection Rules)

## 概述
本仓库已配置分支保护机制，确保代码质量和规范的协作流程。

## 保护规则

### 1. Main 分支保护
- **禁止直接推送到 main 分支**
- 所有更改必须通过 Pull Request (PR) 提交
- PR 必须通过所有 CI 检查才能合并

### 2. 必需的 CI 检查
在 PR 合并到 main 分支之前，必须通过以下检查：
- ✅ Ruff (代码质量检查)
- ✅ Black (代码格式化检查)
- ✅ isort (导入排序检查)
- ✅ Pytest (单元测试)

### 3. 代码审查要求
- 建议启用代码审查（Code Review）要求
- PR 需要至少一位审查者批准（在仓库设置中配置）

## 工作流程

### 正确的提交流程 ✅
```bash
# 1. 从 main 分支创建新分支
git checkout main
git pull origin main
git checkout -b fix/your-feature

# 2. 进行代码修改
# ... 修改文件 ...

# 3. 提交更改
git add .
git commit -m "fix: your change description"

# 4. 推送到远程分支
git push -u origin fix/your-feature

# 5. GitHub Actions 会自动创建 PR（如果启用了 auto-pr.yml）
# 或者手动在 GitHub 网站上创建 PR

# 6. 等待 CI 检查通过
# 7. 在 GitHub 网站上合并 PR
```

### 错误的提交流程 ❌
```bash
# ❌ 不要直接推送到 main 分支
git checkout main
git add .
git commit -m "some changes"
git push origin main  # 这将被拒绝或触发保护警告
```

## 启用严格保护（需要仓库管理员操作）

要完全阻止直接推送到 main 分支，需要在 GitHub 仓库设置中配置：

1. 进入仓库的 **Settings** → **Branches**
2. 添加分支保护规则（Branch protection rule）
3. 规则名称：`main`
4. 启用以下选项：
   - ✅ **Require a pull request before merging**
     - ✅ Require approvals (建议至少 1 个审查者)
   - ✅ **Require status checks to pass before merging**
     - ✅ Require branches to be up to date before merging
     - 添加必需的检查：
       - `grade (lint + format + tests)` (来自 grading-ci.yml)
       - `test-and-lint` (来自 ci.yml)
   - ✅ **Do not allow bypassing the above settings**
   - ⚠️ **Include administrators** (可选，但建议启用以确保规则一致性)

## 自动化特性

### 自动创建 PR
本仓库配置了 `auto-pr.yml` 工作流：
- 当您推送到非 main 分支时，会自动创建 PR
- PR 会包含标准的检查清单
- 节省手动创建 PR 的时间

### 自动运行检查
- 每次 PR 更新时，自动运行所有质量检查
- 检查结果显示在 PR 页面
- 只有全部通过才能合并

## 常见问题

### Q: 我不小心推送到了 main 分支怎么办？
A: 如果分支保护已正确配置，推送会被拒绝。如果推送成功了，说明需要在仓库设置中启用更严格的保护规则。

### Q: 如何查看 CI 检查结果？
A: 在 PR 页面的 "Checks" 标签中可以看到所有检查的详细结果。

### Q: 所有检查都通过了，但无法合并？
A: 检查是否启用了代码审查要求，可能需要其他人批准您的 PR。

## 参考资料
- [GitHub 分支保护文档](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
- [GitHub Actions 工作流文档](https://docs.github.com/en/actions/using-workflows)
