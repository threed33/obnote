下面是用 Git 连接并设置 GitHub 仓库的常用流程。

## 1. 一次性配置（首次使用）

```bash
git config --global user.name "你的用户名"
git config --global user.email "你的GitHub邮箱"
```

## 2. 配置认证（二选一）

**方式 A：SSH（推荐）**

```bash
ssh-keygen -t ed25519 -C "你的GitHub邮箱"
cat ~/.ssh/id_ed25519.pub     # 复制输出内容
```

然后到 GitHub：Settings → SSH and GPG keys → New SSH key，粘贴公钥。测试：

```bash
ssh -T git@github.com
```

**方式 B：HTTPS + 个人访问令牌（PAT）**

在 GitHub 的 Settings → Developer settings → Personal access tokens 生成令牌，之后推送时用令牌代替密码。也可以安装 GitHub CLI 来简化登录：

```bash
gh auth login
```

## 3. 场景一：把本地项目推送到新的 GitHub 仓库

先在 GitHub 网页上新建一个空仓库，然后在本地项目目录执行：

```bash
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:用户名/仓库名.git   # SSH
# 或 https://github.com/用户名/仓库名.git                  # HTTPS
git push -u origin main
```

## 4. 场景二：克隆已有的 GitHub 仓库

```bash
git clone git@github.com:用户名/仓库名.git
```

## 5. 常用的远程仓库管理命令

```bash
git remote -v                                   # 查看远程地址
git remote set-url origin <新地址>               # 修改远程地址（如 HTTPS 改 SSH）
git remote remove origin                        # 删除远程
git push                                        # 推送
git pull                                        # 拉取
```

## 常见问题

- **推送被拒绝（rejected）**：远程有本地没有的提交，先 `git pull --rebase origin main` 再推送。
- **默认分支名不一致**：用 `git branch` 查看本地分支名，与远程保持一致。
- **Permission denied (publickey)**：公钥没添加到 GitHub，或 ssh-agent 没加载密钥（`ssh-add ~/.ssh/id_ed25519`）。

