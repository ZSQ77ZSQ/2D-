# Git 操作模板（照着抄）

> 所有命令都在项目目录里执行。
> 项目目录：`D:\My game\ZSQ`

---

## 0. 通用开头（每次开工先做这两行）

```powershell
cd "D:\My game\ZSQ"
git pull
```

拉完再干活。如果先改了文件再 pull，会报错。

---

## 场景 1 ⭐ 改了好几个文件，全部同步到仓库

**这是最常用的。** 不管改了多少个文件、新增了多少、删了多少，`-A` 全包。

```powershell
cd "D:\My game\ZSQ"

git add -A
git commit -m "这次改了什么（写人话）"
git push
```

**三行搞定。** 输出会像这样：

```
To https://github.com/ZSQ77ZSQ/2D-.git
   82463d9..a1b2c3d  main -> main
```

**提交前想确认一下改了什么：**

```powershell
git status -s
```

```
 M assets/core/crops.ts       ← M = 改了
?? assets/core/economy.ts     ← ?? = 新文件
 D assets/core/old.ts         ← D = 删了
```

---

## 场景 2 ⭐ 只提交**一个**文件（比如新增了 `1.txt`）

**只 add 那一个文件，其它改动原地不动。**

```powershell
cd "D:\My game\ZSQ"

git add 1.txt
git commit -m "新增 1.txt"
git push
```

**结果**：只有 `1.txt` 进了这次提交，你改的其它文件还是"未提交"状态，等下次再提交。

**如果 `1.txt` 在子目录里：**

```powershell
git add assets/1.txt
git add "assets/notes/1.txt"
```

---

## 场景 3 提交**指定的几个**文件

**方法 A：一次列出来**

```powershell
git add 1.txt 2.txt assets/core/crops.ts
git commit -m "更新 1.txt、2.txt、作物表"
git push
```

**方法 B：分几次 add，最后 commit 一次**（效果完全一样）

```powershell
git add 1.txt
git add 2.txt
git add assets/core/crops.ts
git commit -m "更新三个文件"
git push
```

---

## 场景 4 分成**几个独立的提交**

**推荐做法**：一批逻辑一个提交，方便以后单独回退。

```powershell
cd "D:\My game\ZSQ"

# 第一批
git add assets/core/
git commit -m "数值调整"

# 第二批
git add assets/scripts/ assets/scenes/
git commit -m "界面接上数据"

# 第三批
git add -A
git commit -m "文档和剩余改动"

# 最后统一推（3 个提交一起上传）
git push
```

> `push` 只写一次，本地所有未推送的提交会一起上去。

---

## 场景 5 提交**一个目录**下的全部改动

```powershell
git add assets/core/         # core 目录
git add assets/scripts/      # 脚本目录
git add docs/                # 文档目录
```

**用通配符：**

```powershell
git add "assets/**/*.ts"     # 所有 ts 文件
git add "*.md"               # 所有 md 文档
```

---

## 场景 6 新增文件 / 删除文件

**不用特殊处理**，`git add -A` 自动识别。

```powershell
# 新增
New-Item 1.txt -ItemType File
git add -A
git commit -m "新增 1.txt"

# 删除
Remove-Item 1.txt
git add -A
git commit -m "删掉 1.txt"
```

> ⚠️ **Cocos 项目注意**：删资源文件时，同名的 `.meta` 必须一起处理。
> `git add -A` 会自动带上，**不要手动只删资源不删 `.meta`**。

---

## 场景 7 `git push` 被拒绝怎么办

**报错长这样：**

```
! [rejected]  main -> main (non-fast-forward)
error: failed to push some refs
```

**原因**：朋友先推了，你本地落后了。

**解决**（先拉再推）：

```powershell
git pull --rebase
git push
```

---

## 场景 8 提交写错了 / 漏了文件

**漏了文件 —— 补进上一次提交：**

```powershell
git add 忘记的文件.ts
git commit --amend --no-edit
git push
```

**说明文字写错了 —— 改说明：**

```powershell
git commit --amend -m "正确的说明"
git push
```

**已经 push 了才发现漏文件 —— 再补一个提交就行：**

```powershell
git add 忘记的文件.ts
git commit -m "补上漏掉的文件"
git push
```

---

## 场景 9 add 错了想反悔

**把文件从"待提交区"拿出来**（改动不会丢）：

```powershell
git restore --staged 1.txt
```

**全部拿出来：**

```powershell
git restore --staged .
```

**撤销上一次 commit**（改动保留，回到"已改未提交"）：

```powershell
git reset --soft HEAD~1
```

---

## 场景 10 想丢弃还没提交的改动

```powershell
git restore 1.txt        # 丢弃 1.txt 的改动
git restore .            # 丢弃全部（⚠️ 危险，改了的东西就没了）
```

---

## 对照表：我想要 → 我敲什么

| 我想要 | 命令 |
|---|---|
| 拉取朋友的最新代码 | `git pull` |
| 看改了哪些文件 | `git status -s` |
| 看每个文件改了几行 | `git diff --stat` |
| 看具体改了什么内容 | `git diff` |
| **全部提交** | `git add -A` + `git commit -m "..."` + `git push` |
| **只提交一个文件** | `git add 1.txt` + `git commit -m "..."` + `git push` |
| 提交几个文件 | `git add a.txt b.txt c.ts` + commit + push |
| 提交一个目录 | `git add assets/core/` + commit + push |
| 分成几个提交 | 多次 `git add` + `git commit`，最后 `push` 一次 |
| 看提交历史 | `git log --oneline` |
| 看远程地址 | `git remote -v` |
| 被拒绝推不上去 | `git pull --rebase` 然后 `git push` |

---

## 复制即用模板

把 `说明文字` 换成你这次做了什么，就这两块：

```powershell
# ═══ 开工 ═══
cd "D:\My game\ZSQ"
git pull

# ═══ 收工 ═══
git add -A
git commit -m "说明文字"
git push
```

---

## ⚠️ 前置条件

**`git pull` / `git push` 之前，Clash 必须开着。**

GitHub 走本地代理 `127.0.0.1:7890`。Clash 关了会报：

```
fatal: unable to access 'https://github.com/...': Failed to connect
```

**看到这个报错 → 先检查 Clash 是不是没开。**

---

## 🔴 唯一需要警惕的命令

```powershell
git push --force
```

**别用。** 它会把远程的提交直接抹掉，朋友的工作可能就没了。

只有在**你确定远程的东西是错的、必须丢掉**的时候才用，而且提前跟朋友说一声。

日常永远用普通的 `git push`；被拒绝了就 `git pull --rebase` 再推。
