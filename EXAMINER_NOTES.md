# 评审说明（供出题/阅卷人使用）

## 1. 题目设计意图
- `src/bad_style.py` 人为写得很差：导入在同一行、命名随意、短路写法、资源泄露风险、异常类型不规范等
- 候选人需要把它改成“更像生产代码”的样子
- 但测试锁定了对外行为：CLI 输出与 calc 的数值行为不能变

## 2. 推荐的优秀改法（非唯一）
- 把异常改为 `ValueError` 或自定义异常（注意 tests 里现在期望 raises Exception，可引导面试官接受改动并同步测试；但作为考试建议不要改 tests）
- 使用上下文管理器读文件
- 规范化命名（calc -> calculate_statistics 等）
- 增加类型标注
- 把 parse_numbers 的 split 逻辑简化
- 按 black 风格格式化
- 导入分组与排序

## 3. 出题上线流程（建议）
1. 将该仓库设置为模板仓库（GitHub Template Repository）
2. 考生从模板生成自己的考试仓库（或你们系统自动创建）
3. **配置仓库的分支保护规则**（重要）：
   - Settings → Branches → Add branch protection rule
   - Branch name pattern: `main`
   - 启用以下选项：
     - ✅ Require a pull request before merging
     - ✅ Require status checks to pass before merging
     - 添加必需检查：`grade (lint + format + tests)`
     - ✅ Do not allow bypassing the above settings
   - 详细配置请参考 `BRANCH_PROTECTION.md`
4. 要求考生在规定时间内提交 PR
5. CI 自动给出是否通过
6. 管理端收集 PR 链接与 CI 状态，进行最终评审

## 4. 自动化功能说明
- **自动 PR 创建**：`auto-pr.yml` 会在学生推送分支时自动创建 PR
- **自动 CI 检查**：`grading-ci.yml` 会在 PR 上自动运行所有检查
- **分支保护监控**：`main-protection.yml` 监控对 main 分支的操作
- **代码审查要求**：`.github/CODEOWNERS` 定义了代码审查者