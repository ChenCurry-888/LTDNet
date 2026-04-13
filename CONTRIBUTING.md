# LTDNet 贡献指南与Git使用规范

## 目录
1. [Git仓库设置](#1-git仓库设置)
2. [分支策略](#2-分支策略)
3. [提交规范](#3-提交规范)
4. [日常开发流程](#4-日常开发流程)
5. [代码审查](#5-代码审查)
6. [版本发布](#6-版本发布)

---

## 1. Git仓库设置

### 1.1 初始化Git仓库（已配置）

本项目已配置Git仓库，远程地址为：
```bash
origin  https://github.com/dyl96/LTDNet.git
```

### 1.2 配置Git用户信息

```bash
# 配置全局用户名和邮箱
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# 配置编辑器（可选）
git config --global core.editor vim

# 配置换行符处理（Windows用户）
git config --global core.autocrlf true

# 配置换行符处理（Linux/Mac用户）
git config --global core.autocrlf input
```

### 1.3 配置Git别名（推荐）

```bash
# 简化常用命令
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

---

## 2. 分支策略

本项目采用 **Git Flow** 简化版分支策略：

### 2.1 分支说明

| 分支 | 说明 | 保护级别 |
|------|------|----------|
| `main` | 主分支，稳定版本 | 受保护 |
| `develop` | 开发分支，日常开发 | 受保护 |
| `feature/*` | 功能分支 | 临时分支 |
| `bugfix/*` | 修复分支 | 临时分支 |
| `hotfix/*` | 紧急修复分支 | 临时分支 |
| `release/*` | 发布分支 | 临时分支 |

### 2.2 分支创建命令

```bash
# 创建并切换到开发分支
git checkout -b develop origin/main

# 创建功能分支（从develop分支）
git checkout -b feature/add-new-model develop

# 创建修复分支
git checkout -b bugfix/fix-memory-leak develop

# 创建紧急修复分支（从main分支）
git checkout -b hotfix/critical-bug-fix main
```

### 2.3 分支合并流程

```bash
# 功能开发完成后，合并到develop
git checkout develop
git merge --no-ff feature/add-new-model
git branch -d feature/add-new-model

# 发布版本时，合并到main
git checkout main
git merge --no-ff develop

# 推送标签
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin v1.0.0
```

---

## 3. 提交规范

### 3.1 提交信息格式

采用 **Conventional Commits** 规范：

```
<type>(<scope>): <subject>

<body>

<footer>
```

### 3.2 提交类型（Type）

| 类型 | 说明 | 示例 |
|------|------|------|
| `feat` | 新功能 | feat(model): add RepViT backbone |
| `fix` | 修复bug | fix(train): resolve memory leak in dataloader |
| `docs` | 文档更新 | docs(readme): update installation guide |
| `style` | 代码格式 | style: fix indentation in loss.py |
| `refactor` | 重构代码 | refactor(neck): simplify FPN structure |
| `perf` | 性能优化 | perf(inference): optimize NMS speed |
| `test` | 测试相关 | test: add unit tests for anchor generator |
| `chore` | 构建/工具 | chore: update requirements.txt |
| `ci` | CI/CD相关 | ci: add GitHub Actions workflow |

### 3.3 作用域（Scope）

可选，用于说明修改范围：

- `model` - 模型相关
- `data` - 数据处理
- `train` - 训练相关
- `test` - 测试相关
- `config` - 配置文件
- `docs` - 文档
- `tool` - 工具脚本

### 3.4 提交示例

```bash
# 简单提交
git commit -m "feat(model): add LTDNet backbone implementation"

# 详细提交
git commit -m "feat(model): add attention mechanism to FPN

- Implement channel attention module
- Add spatial attention for small objects
- Update config files accordingly

Closes #123"

# 修复提交
git commit -m "fix(train): resolve CUDA out of memory error

- Reduce default batch size from 8 to 4
- Add gradient checkpointing option
- Update documentation"
```

---

## 4. 日常开发流程

### 4.1 开始新功能开发

```bash
# 1. 更新本地develop分支
git checkout develop
git pull origin develop

# 2. 创建功能分支
git checkout -b feature/your-feature-name

# 3. 进行开发工作...

# 4. 提交更改
git add .
git commit -m "feat(scope): your commit message"

# 5. 推送到远程
git push origin feature/your-feature-name

# 6. 创建Pull Request（通过GitHub/GitLab界面）
```

### 4.2 保持分支同步

```bash
# 在功能分支上同步最新develop代码
git checkout develop
git pull origin develop
git checkout feature/your-feature
git rebase develop

# 或者使用merge
git merge develop
```

### 4.3 常用Git命令速查

```bash
# 查看状态
git status

# 查看修改内容
git diff

# 查看提交历史
git log --oneline -10

# 查看分支图
git log --graph --oneline --all

# 撤销工作区修改
git checkout -- <file>

# 撤销暂存区文件
git reset HEAD <file>

# 修改最后一次提交
git commit --amend

# 储藏当前修改
git stash

# 恢复储藏的修改
git stash pop

# 查看远程仓库
git remote -v

# 获取远程更新
git fetch origin

# 拉取并合并
git pull origin main

# 推送本地分支
git push origin <branch-name>

# 删除远程分支
git push origin --delete <branch-name>

# 清理已删除的远程分支引用
git remote prune origin
```

---

## 5. 代码审查

### 5.1 提交前检查清单

- [ ] 代码可以正常运行
- [ ] 添加了必要的注释
- [ ] 更新了相关文档
- [ ] 通过了本地测试
- [ ] 没有遗留调试代码
- [ ] 提交信息符合规范

### 5.2 Pull Request模板

创建PR时请包含以下信息：

```markdown
## 描述
简要描述本次修改的内容

## 修改类型
- [ ] Bug修复
- [ ] 新功能
- [ ] 性能优化
- [ ] 文档更新
- [ ] 代码重构

## 测试
- [ ] 通过了单元测试
- [ ] 通过了集成测试
- [ ] 手动测试通过

## 相关Issue
Fixes #(issue编号)
```

---

## 6. 版本发布

### 6.1 版本号规范

采用 [语义化版本](https://semver.org/lang/zh-CN/)（SemVer）：

```
主版本号.次版本号.修订号
```

- **主版本号**：不兼容的API修改
- **次版本号**：向下兼容的功能新增
- **修订号**：向下兼容的问题修复

### 6.2 发布流程

```bash
# 1. 创建发布分支
git checkout -b release/v1.0.0 develop

# 2. 更新版本号（修改mmdet/version.py）
# 修改版本号并提交
git add mmdet/version.py
git commit -m "chore(release): bump version to 1.0.0"

# 3. 合并到main分支
git checkout main
git merge --no-ff release/v1.0.0

# 4. 打标签
git tag -a v1.0.0 -m "Release version 1.0.0"

# 5. 合并回develop分支
git checkout develop
git merge --no-ff release/v1.0.0

# 6. 删除发布分支
git branch -d release/v1.0.0

# 7. 推送到远程
git push origin main
git push origin develop
git push origin v1.0.0
```

---

## 7. 常见问题解决

### 7.1 解决合并冲突

```bash
# 当rebase或merge出现冲突时
# 1. 查看冲突文件
git status

# 2. 编辑冲突文件，解决冲突
# 标记为已解决
git add <resolved-file>

# 3. 继续rebase
git rebase --continue

# 或者放弃rebase
git rebase --abort
```

### 7.2 撤销错误提交

```bash
# 撤销最后一次提交（保留修改）
git reset --soft HEAD~1

# 撤销最后一次提交（丢弃修改）
git reset --hard HEAD~1

# 撤销已推送到远程的提交（谨慎使用）
git revert <commit-hash>
git push origin <branch>
```

### 7.3 大文件处理

本项目已配置 `.gitignore` 排除大文件，如需追踪特定大文件，请使用 Git LFS：

```bash
# 安装Git LFS
git lfs install

# 追踪大文件
git lfs track "*.pth"
git lfs track "*.pth.tar"
git lfs track "checkpoint-*"

# 提交.gitattributes
git add .gitattributes
git commit -m "chore: setup Git LFS for model checkpoints"
```

---

## 8. 附录

### 8.1 项目目录结构速览

```
LTDNet/
├── 📄 项目文档 (README.md, CONTRIBUTING.md)
├── 🔧 依赖配置 (requirements/, setup.py)
├── 🧠 核心代码 (mmdet/)
│   ├── apis/         # API接口
│   ├── core/         # 核心功能
│   ├── datasets/     # 数据集处理
│   ├── models/       # 模型定义
│   └── utils/        # 工具函数
├── ⚙️ 配置文件 (configs/, configs_ltdnet/)
├── 🛠️ 工具脚本 (tools/)
└── 🧪 测试代码 (tests/)
```

### 8.2 相关链接

- [Git官方文档](https://git-scm.com/doc)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Semantic Versioning](https://semver.org/)
- [Git Flow工作流](https://nvie.com/posts/a-successful-git-branching-model/)

---

**最后更新**：2024年

如有问题，请通过GitHub Issues联系维护团队。