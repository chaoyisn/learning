# Git 常用命令速查表 🍿

> 配套学习仓库，按课程顺序整理。核心心智模型：

```
工作区 🏠 ──git add──▶ 暂存区 🛒 ──git commit──▶ 仓库 📦 ──git push──▶ 远程 ☁️
```

---

## 0. 基础

```bash
git init                          # 把当前文件夹变成 Git 仓库（一生一次）
git status                        # 查看状态：什么改了、什么在购物车里（最常用，迷茫时先敲它）
git status -s                     # 精简版状态输出
git help 命令名                    # 查看某命令的官方说明书，如 git help add
```

## 1. 存档流程（第 1 课）

```bash
git add 文件名                     # 把该文件的改动放进购物车（暂存区）
git add .                         # 把所有改动的文件一次性放进购物车（. = 全部）
git commit -m "说明"               # 结账：把购物车内容存档，说明写清这次改了什么
git commit -a -m "说明"            # 自动 add 已跟踪文件 + 存档，一步到位（不含新文件！）
git log                           # 查看完整存档历史（作者、日期、编号）
git log --oneline                 # 精简版：一行一个存档，最新在上
git log --oneline --graph --all   # 带分叉合流图 + 显示所有分支（看全貌神器）
git show                          # 查看最近一次存档的详细内容
```

## 2. 查看 diff —— 三扇窗（第 2 课）

```bash
git diff                          # 窗①：工作区 vs 购物车（还没 add 的改动）
git diff --staged                 # 窗②：购物车 vs 最新存档（已 add 未 commit 的改动）
git diff 文件名                    # 只看指定文件的改动
git diff 存档1 存档2               # 窗③：任意两个存档之间的差异
git diff HEAD~1 HEAD              # 上一个存档 vs 最新存档（HEAD~1 = 往前数一个）
```

## 3. 撤销 —— 两颗药（第 2 课）

```bash
git restore --staged 文件名        # 药①：把误加的文件从购物车取出（改动保留在工作区）
git restore 文件名                 # 药②：丢弃工作区未存档的改动，恢复成存档时的样子（不可反悔！）
```

## 4. 文件管理（第 3 课）

```bash
git rm 文件名                      # 删除文件 + 把删除动作放入购物车（二合一）
git rm --cached 文件名             # 只停止跟踪（从仓库开除），文件保留在本地——密码误提交时用
git mv 旧名 新名                   # 改名/移动 + 自动放入购物车（Git 能识别"这是改名"）
```

`.gitignore` 文件（无视清单，本身要提交）：

```text
*.log          # 忽略所有 .log 文件
password.txt   # 忽略指定文件
temp/          # 忽略整个文件夹
node_modules/  # 典型：依赖包，绝不进仓库
```

⚠️ 无视清单只拦"新来的"，已被跟踪的文件写进去也无效。

## 5. 分支（第 4 课）

```bash
git branch                       # 列出本地分支，* 标出当前所在
git branch -v                    # 列出分支 + 各自最新存档
git branch 分支名                  # 只创建分支，人不过去（⚠️ 经典坑）
git switch -c 分支名               # 创建 + 立刻切过去 ⭐ 最常用
git switch 分支名                  # 切换到已存在的分支（切换前工作区要干净！）
git branch -d 分支名               # 删除已合并的分支（撕便利贴，成果已在主线）
git branch -D 分支名               # 强制删除（未合并的也删，慎用）
git branch -m 旧名 新名            # 改分支名（如 master → main）
```

## 6. 合并与冲突（第 5 课）

```bash
git merge 分支名                   # 把指定分支合入"我当前所在"的分支（先 switch 到接受方！）
git merge --abort                 # 冲突了想反悔：一键回到合并前
```

冲突标记（出现在文件里，人工裁决后 add + commit）：

```text
<<<<<<< HEAD      ← 我这边（当前分支）的版本
我的改动
=======
对方的改动         ← 对方分支的版本
>>>>>>> 分支名
```

> 裁决 = 把文件改成最终想要的样子 + 删掉所有标记行 + `git add` + `git commit`

## 7. 远程仓库（第 6 课）

```bash
git clone 网址                     # 把云端仓库完整复制到本地（含全部历史和分支）
git clone 网址 目录名               # 复制到指定文件夹名
git remote add origin 网址         # 登记远程地址，代号叫 origin（约定俗成，可任意改名）
git remote -v                     # 查看已登记的远程（通讯录）
git remote set-url origin 新网址   # 改地址，代号不变
git remote rename 旧名 新名        # 改代号（如 origin → github，纯标签操作）
git remote remove origin          # 删除这条远程登记
git push -u origin 分支名          # 首次推送该分支并建立配对（-u = 记住，以后只敲 git push）
git push                          # 日常推送（地址和分支都记住了）
git push origin --delete 分支名    # 删除云端上的分支
git pull                          # 拉取远程新存档并合并进本地（= fetch + merge）
git fetch                         # 只下载不合并（更新 origin/main 影子，先看看再决定）
```

## 8. 名词速查

| 写法 | 含义 |
|---|---|
| `HEAD` | 📌 你当前所在的分支/存档 |
| `HEAD~1` | 比 HEAD 往前数一个存档（`HEAD~3` = 往前数三个） |
| `b1a25cb` | 存档编号（SHA 哈希），可复制粘贴使用 |
| `origin` | 远程仓库的代号（存在 .git/config 里） |
| `origin/main` | 远程 main 在你本地的"影子"（上次同步时的样子） |

## 9. 团队日常循环（第 7 课）

```bash
git switch main                   # ① 开工：回到主线
git pull                          # ② 同步：拿走同事昨晚合并的新东西
git switch -c 我的任务名            # ③ 开今天的平行宇宙
#                                  ④ 干活：编辑 → add → commit → 循环
git push -u origin 我的任务名       # ⑤ 分支推上云（备份 + 准备申请合流）
#                                  ⑥ 网页发 Pull Request → 审查 → 合并
git switch main && git pull       # ⑦ 合并后：拿到含我成果的新 main
git branch -d 我的任务名            # ⑧ 清理，开下一轮
```

## 10. 预告：后悔药大礼包（第 8 课，待学）

```bash
git commit --amend                # 修补最近一次存档（改说明/补漏加文件）
git reset --soft HEAD~1           # 撤销最近一次存档，改动退回购物车
git reset --mixed HEAD~1          # 撤销存档，改动退回工作区（默认）
git reset --hard HEAD~1           # 撤销存档，改动也丢（⚠️ 危险但威力大）
git reflog                        # 黑匣子：记录 HEAD 的每一步移动，几乎能找回一切
git stash                         # 把手头改动暂存进抽屉（切分支前不想提交时用）
git stash pop                     # 从抽屉取出刚才暂存的改动
```
