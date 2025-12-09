**基础操作流程**：

1. ‌**初始化仓库**‌：在项目目录下运行 `git init` 创建本地仓库。
2. ‌**添加文件**‌：用 `git add <文件名>` 将修改的文件添加到暂存区。
3. ‌**提交更改**‌：用 `git commit -m "提交信息"` 将暂存区的更改提交到本地仓库。
4. ‌**查看状态**‌：随时用 `git status` 查看文件状态和待提交的修改。

‌**分支管理**‌：

- `git branch` 查看分支，`git branch <新分支名>` 创建分支。
- `git checkout <分支名>` 切换分支。

‌**远程协作**‌：

- `git clone <远程仓库地址>` 克隆远程仓库到本地。
- `git push` 将本地提交推送到远程仓库。

**实用技巧**‌：

- `git log` 查看提交历史。
- `git diff` 查看工作区与暂存区的差异。
- 多人协作时，先 `git pull` 拉取远程最新更改再提交，避免冲突。

1.**本地账号登录**：
‌**全局配置**‌（影响所有仓库）：
```bash
git config --global user.name "新用户名"
git config --global user.email "新邮箱"
```
**验证**‌：`git config --list` 查看配置。

‌**项目级配置**‌（仅影响当前仓库）：
```bash
git config user.name "新用户名"
git config user.email "新邮箱"
```

### 2. ‌**远程仓库账号切换**‌

- ‌**修改远程URL**‌：
    
    `git remote set-url origin 新仓库地址`
    
- ‌**清除本地凭证**‌：
    
    `git credential-manager delete --host=旧仓库主机名`
    
- ‌**重新认证**‌：`git push` 时输入新账号信息。

### **常见问题处理**‌

- ‌**用户名冲突**‌：使用 `--replace-all` 替换所有配置：
    
    `git config --global --replace-all user.name "新用户名"`
    
- ‌**密码错误**‌：`git config --global credential.helper store` 自动缓存凭证。