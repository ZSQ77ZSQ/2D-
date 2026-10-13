# Git 使用手册

> **适用项目**：ZSQ（仓库名 `2D-`）
> **仓库地址**：https://github.com/ZSQ77ZSQ/2D-
> **本地目录**：`D:\My game\ZSQ`
> **写给**：ZSQ、0tmzz0，以及以后加入的任何伙伴
> **最后更新**：2026-10-13

---

## 📖 这份手册怎么用

**不要从头读到尾。** 按下面的索引，直接跳到你要的那一节。

| 我想…… | 看这里 |
|---|---|
| **五分钟上手**（只看一页就够） | [第 0 章](#第-0-章--五分钟上手) |
| 第一次在这台电脑上配置 | [第 1 章](#第-1-章--第一次使用前的准备) |
| 每天开工、收工怎么做 | [第 2 章](#第-2-章--每天的标准流程) |
| **要提交代码，具体敲什么** | [**第 3 章**](#第-3-章--各种提交情况一一对应) ⭐ |
| 想看看现在改了什么 | [第 4 章](#第-4-章--查看状态) |
| 两个人一起改怎么不出乱子 | [第 5 章](#第-5-章--两个人协作规则) |
| **两个人改了同一个文件（冲突）** | [5.6 节](#56-冲突是什么样子的) |
| 报错了、卡住了 | [第 6 章](#第-6-章--出错了怎么办) |
| 所有命令速查表 | [第 7 章](#第-7-章--命令总表) |
| 哪些事**绝对不能**做 | [第 8 章](#第-8-章--绝对不要做的事) |
| 名词看不懂 | [附录 B](#附录-b--名词解释) |
| **只想看一页纸（贴显示器边上）** | [附录 C](#附录-c--一页纸速查贴在显示器边上) |

---

# 第 0 章 · 五分钟上手

**如果你只想记住一件事，记住这个：**

```
开工先拉，收工就推。
```

## 每天开工（打开电脑后第一件事）

```powershell
cd "D:\My game\ZSQ"
git pull
```

## 每天收工（做完一块就做一次）

```powershell
git add -A
git commit -m "这次改了什么"
git push
```

## 就这五条命令

```powershell
git pull                          # ① 拉取别人的更新
git add -A                        # ② 把所有改动放进待提交清单
git commit -m "说明文字"           # ③ 打包存档
git push                          # ④ 上传到 GitHub
git status                        # ⑤ 随时查看当前状态
```

**其它都不用记。** 遇到具体问题再翻对应章节。

---

# 第 1 章 · 第一次使用前的准备

> 每台电脑**只需要做一次**。做过就跳过。

## 1.1 安装 Git

**下载**：https://git-scm.com/download/win

**安装**：一路点「下一步」就行，所有默认选项都不用改。

**验证装好了**：打开 PowerShell，敲：

```powershell
git --version
```

看到类似 `git version 2.55.0.windows.4` 就成功了。

---

## 1.2 告诉 Git 你是谁（必做）

**每提交一次代码，Git 都会记录"是谁提交的"。** 所以必须先设置。

```powershell
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

**例子**：

```powershell
git config --global user.name "ZSQ"
git config --global user.email "zhangsq1377@gmail.com"
```

> ⚠️ **邮箱最好和你 GitHub 账号绑定的邮箱一致**，否则 GitHub 上不显示你的头像，提交记录里也不是可点击的链接。
>
> 查 GitHub 绑定邮箱：https://github.com/settings/emails

**顺手做的一个小配置**（让中文文件名正常显示，不然会是乱码）：

```powershell
git config --global core.quotepath false
```

---

## 1.3 把仓库下载到电脑上（第一次）

**这台电脑上还没有这个项目**，用 `clone` 下载：

```powershell
cd "D:\My game"
git clone https://github.com/ZSQ77ZSQ/2D-.git
```

**下载完会多出一个文件夹**：`D:\My game\2D-`

> ⚠️ 注意：`clone` 下来的文件夹叫 `2D-`，是因为仓库名就叫 `2D-`。
> ZSQ 的电脑上文件夹叫 `ZSQ`，是因为他当初是用 Cocos Dashboard 建的项目。
> **文件夹名字不同不影响协作**，里面的内容是一样的。

**下载完之后**：

```powershell
cd "D:\My game\2D-"
git log --oneline
```

能看到提交历史就说明成功了。

---

## 1.4 ⚠️ 每次 push/pull 之前，代理必须开着

**这是本项目最重要的一条环境要求。**

GitHub 在国内直连不通，必须走代理。本项目用的是：

```
代理地址：127.0.0.1:7890    （Clash）
```

**如果 Clash 没开，你会看到：**

```
fatal: unable to access 'https://github.com/...': 
Failed to connect to github.com port 443 after 21000 ms
```

**看到这个报错 → 先检查 Clash 是不是没开。**

### 配置 Git 走代理（每台电脑做一次）

```powershell
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
```

### 以后不想用代理了，怎么取消

```powershell
git config --global --unset http.proxy
git config --global --unset https.proxy
```

### 查一下现在配了什么

```powershell
git config --global --list
```

---

## 1.5 权限：你需要被加为协作者

**光有账号不够，必须被仓库主人加为 Collaborator，否则 push 会被拒绝：**

```
remote: Permission to ZSQ77ZSQ/2D-.git denied to xxx.
fatal: ... The requested URL returned error: 403
```

**仓库主人操作**：Settings → Access → Add people → 输入对方用户名 → 权限选 **Write** → 确认 → 对方接受邮件邀请。

**如何确认自己有没有权限**：

```powershell
git push
```

能推上去就是有权限。

---

# 第 2 章 · 每天的标准流程

## 完整时间线

```
【打开电脑】
    │
    │  ①  git pull              ← 先拿别人的更新（必须在打开编辑器之前）
    ▼
【打开 Cocos Creator，干活】
    │
    │  ②  干活……干活……
    │
    │  ③  git add -A            ← 做完一块就提交一次，别攒一天
    │      git commit -m "..."
    │
    │  ④  git push              ← 推上去，别人才能拿到
    ▼
【关闭项目】
    │
    │  ⑤  git status            ← 确认显示 "nothing to commit"
    ▼
```

---

## 2.1 开工：先拉取

```powershell
cd "D:\My game\ZSQ"
git pull
```

**⚠️ 要在打开 Cocos Creator 之前做。**

原因：编辑器一打开就会锁住 `library/` 并可能自动改写场景文件，这时候拉取容易出问题。

### 你会看到三种结果

| 输出 | 含义 | 下一步 |
|---|---|---|
| `Already up to date.` | 对方没更新 | 直接干活 |
| `Updating xxx..yyy` + 一堆文件名 | 拉到了新东西 | 直接干活 |
| `error: Your local changes...` | 你有还没提交的改动 | 看 [6.2 节](#62-pull-时提示有本地改动) |

---

## 2.2 干活

**这一步不用管 Git。** 正常改文件就行。

---

## 2.3 收工：提交 + 推送

```powershell
git add -A
git commit -m "这次改了什么"
git push
```

**`-m` 后面的引号里写人话**，比如：

```
改了萝卜的生长时间从 60 秒到 90 秒
加了白菜的作物数据
修复离线收益算错的问题
把主界面的按钮调大了一点
```

> 💡 **为什么做完一小块就提交，不能攒一天？**
>
> 因为 Cocos 的**场景文件（`.scene`）是 JSON 格式**。两个人如果都改了同一个场景，Git 没办法自动合并，得人工解决冲突。
>
> **提交越频繁 = 每次改动越小 = 万一冲突了也容易解决。**

---

## 2.4 关闭项目前：确认干净

```powershell
git status
```

**看到这段就是干净的：**

```
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

**如果看到一片文件名**，说明还有没提交的改动 → 回到 2.3 节。

---

# 第 3 章 · 各种提交情况（一一对应）

> **这一章是全文最重要的。** 找到和你一样的情况，照着敲就行。

**统一前提**：所有命令都在项目目录里执行。

```powershell
cd "D:\My game\ZSQ"
```

---

## 情况 1 · 改了好几个文件，全部提交 ⭐ **最常用**

**【你的情况】** 改了 5 个文件、新增 2 个、删了 1 个，想一次性全提交

**【敲这些】**

```powershell
git add -A
git commit -m "这次改了什么"
git push
```

**【结果】** 所有变化（改的 / 新增的 / 删的）一起打包成一个存档并上传。

**【为什么用 `-A`】** `-A` = All，一次包揽三种变化：

| 你做的事 | `-A` 包含吗 |
|---|---|
| 修改了已有文件 | ✅ |
| 新增了文件 | ✅ |
| 删除了文件 | ✅ |

**【怎么确认成功】** 最后一行显示类似：

```
   5c6d122..352d52b  main -> main
```

---

## 情况 2 · 只提交**一个**文件

**【你的情况】** 我改了 6 个文件，但这次只想提交 `1.txt`

**【敲这些】**

```powershell
git add 1.txt
git commit -m "新增 1.txt"
git push
```

**【结果】**
- `1.txt` ✅ 提交并推送
- **其它 5 个文件原地不动**，还是"未提交"状态，下次再提交

**【文件在子目录里怎么写】**

```powershell
git add assets/1.txt
git add "assets/notes/1.txt"
```

---

## 情况 3 · 只提交**指定的几个**文件

**【你的情况】** 只想提交 `1.txt`、`2.txt`、`crops.ts` 这三个

**【方法 A：一次列出来】**

```powershell
git add 1.txt 2.txt assets/core/crops.ts
git commit -m "更新三个文件"
git push
```

**【方法 B：分几次 add，最后 commit 一次】**（效果完全一样）

```powershell
git add 1.txt
git add 2.txt
git add assets/core/crops.ts
git commit -m "更新三个文件"
git push
```

---

## 情况 4 · 只提交**某个文件夹**

**【你的情况】** 只想提交 `assets/core/` 里的改动

**【敲这些】**

```powershell
git add assets/core/
git commit -m "数值调整"
git push
```

**【也支持通配符】**

```powershell
git add "assets/**/*.ts"      # 所有 ts 文件
git add "*.md"                # 所有 md 文档
git add assets/core/crop*.ts  # 以 crop 开头的文件
```

---

## 情况 5 · 分成**几个独立的提交** ⭐ 推荐做法

**【你的情况】** 改了数值又改了界面，想分成两个提交（方便以后单独回退）

**【敲这些】**

```powershell
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

**【关键】** **`push` 只写一次**，本地所有未推送的提交会一起上去。

**【为什么推荐这样做】** 以后要回退时，可以只回退"数值调整"而不影响"界面"。而且别人拉取时冲突更好解决。

---

## 情况 6 · 新增了文件

**【你的情况】** 新建了几个文件，想提交

**【敲这些】**

```powershell
git add -A
git commit -m "新增 xxx 文件"
git push
```

**【说明】** 新增文件**不需要特殊命令**，`git add -A` 会自动识别。

**【⚠️ Cocos 项目特别注意】**

Cocos 会自动给每个资源生成一个同名的 `.meta` 文件：

```
assets/core/crops.ts          ← 你写的代码
assets/core/crops.ts.meta     ← Cocos 自动生成（必须一起提交！）
```

**`git add -A` 会自动带上 `.meta`，不用你操心。** 但**绝对不能只提交 `.ts` 不提交 `.meta`** —— 那样别人拉下来工程会报错。

---

## 情况 7 · **删除**一个文件

**【你的情况】** 想删掉 `old-crops.ts`

**【敲这些】**

```powershell
git rm assets/core/old-crops.ts
git commit -m "删掉废弃的作物表"
git push
```

**【或者：先手动删，再让 Git 记录】**

```powershell
Remove-Item assets/core/old-crops.ts
Remove-Item assets/core/old-crops.ts.meta     # meta 也要删
git add -A
git commit -m "删掉废弃的作物表"
git push
```

**【⚠️ 最关键的一点】**

**光在资源管理器里右键删除是不够的！**

| 位置 | 会怎样 |
|---|---|
| 你的电脑 | ✅ 没了 |
| **GitHub 上** | ❌ **还在** |
| **别人的电脑上** | ❌ **还在** |
| 以后别人 clone | ❌ **又会回来** |

**必须走完 `add` → `commit` → `push` 三步，才算真的删掉。**

---

## 情况 8 · **删除**一个文件夹

**【你的情况】** 想删掉 `TEST` 文件夹

**【敲这些】**

```powershell
git rm -r TEST
git commit -m "删掉 TEST 文件夹"
git push
```

**【参数解释】**

| 部分 | 含义 |
|---|---|
| `git rm` | 删除文件**并记录到待提交清单**（一步到位） |
| `-r` | **r**ecursive（递归），**删目录必须加**，不加会报错 |

**【或者：先手动删】**

```powershell
Remove-Item -Recurse -Force "D:\My game\ZSQ\TEST"
git add -A                  # ← 这里正好体现 -A 的用处：自动识别"删除了文件"
git commit -m "删掉 TEST 文件夹"
git push
```

---

## 情况 9 · **重命名**文件

**【你的情况】** 想把 `crops.ts` 改名成 `crop-data.ts`

**【推荐：用 git 命令】**

```powershell
git mv assets/core/crops.ts assets/core/crop-data.ts
git mv assets/core/crops.ts.meta assets/core/crop-data.ts.meta
git commit -m "作物表改名为 crop-data.ts"
git push
```

**【也可以：手动改名 + add】**

```powershell
git add -A
git commit -m "重命名文件"
git push
```

**【⚠️ Cocos 项目注意】** 资源文件改名时，**`.meta` 也要一起改名**，否则编辑器会报错丢失引用。

---

## 情况 10 · 提交时**漏了**一个文件

**【你的情况】** 刚 commit 完发现忘了 `economy.ts`

**【如果还没 push】**

```powershell
git add assets/core/economy.ts
git commit --amend --no-edit
git push
```

`--amend` = 把新文件**合并进上一次提交**；`--no-edit` = 说明文字保持不变。

**【如果已经 push 了】**

**再补一个提交就行**，不要折腾：

```powershell
git add assets/core/economy.ts
git commit -m "补上漏掉的 economy.ts"
git push
```

---

## 情况 11 · 提交说明**写错了**

**【你的情况】** 说明写成了"asdf"，实际是"修复存档 bug"

**【如果还没 push】**

```powershell
git commit --amend -m "修复存档 bug"
git push
```

**【如果已经 push 了】**

**算了，别改了。** 说明文字写错不影响任何功能，下次写对就行。

> 硬要改也可以（`git push --force`），但**有风险**，详见 [第 8 章](#第-8-章--绝对不要做的事)。

---

## 情况 12 · 提交了但**还没 push**，想撤销这次提交

**【你的情况】** 刚 commit 完，发现有东西提交错了

**【敲这个】**

```powershell
git reset --soft HEAD~1
```

**【结果】** 撤销最后一次提交，但**你的改动都还在**（回到"已改未提交"状态），可以重新整理后再提交。

**【想彻底不要这些改动了】**

```powershell
git reset --hard HEAD~1
```

> ⚠️ **`--hard` 会真的丢掉改动，不可恢复。** 慎用。

---

## 情况 13 · 丢弃**还没提交**的改动

**【你的情况】** 改乱了，想恢复到上次提交的状态

**【丢弃某个文件】**

```powershell
git restore assets/core/crops.ts
```

**【丢弃全部】**

```powershell
git restore .
```

> ⚠️ **危险**：改过的东西就真的没了。执行前先 `git status` 确认一下。

---

## 情况 14 · `add` 错了，想撤回

**【你的情况】** 把不想提交的文件 `add` 进去了

**【敲这个】**

```powershell
git restore --staged 不想提交的文件.ts
```

**【全部拿出来】**

```powershell
git restore --staged .
```

**【结果】** 文件从"待提交清单"里拿出来，**但改动还在**，只是这次不提交它。

---

## 情况 15 · 提交前想先看看改了什么

**【敲这些】**

```powershell
git status -s          # 简写：M=改了 ??=新文件 D=删了
git diff --stat        # 每个文件改了多少行
git diff               # 具体改了什么内容
```

**【输出长这样】**

```
 M assets/core/crops.ts       ← M = Modified，改了
?? assets/core/economy.ts     ← ?? = 新文件，Git 还不认识
 D assets/core/old.ts         ← D = Deleted，删了
```

---

# 第 4 章 · 查看状态

## 我想知道…… → 敲这个

| 我想知道 | 命令 |
|---|---|
| 现在有哪些改动 | `git status` |
| 简写版（一屏看完） | `git status -s` |
| 每个文件改了几行 | `git diff --stat` |
| 具体改了什么内容 | `git diff` |
| 某个文件改了什么 | `git diff 文件路径` |
| 提交历史 | `git log --oneline` |
| 图形化历史 | `git log --oneline --graph --all` |
| 我的仓库地址 | `git remote -v` |
| 我在哪个分支 | `git branch` |
| 本地比远程多了什么 | `git status -sb` |
| 最近一次提交改了什么 | `git show` |

---

## `git status -s` 的三个符号

| 符号 | 含义 |
|---|---|
| `M` | **M**odified，改过了 |
| `??` | 新文件，Git 还不认识它 |
| `D` | **D**eleted，删掉了 |
| `A` | **A**dded，已放进待提交清单 |
| `R` | **R**enamed，改名了 |

**位置也重要：**

```
 M assets/core/crops.ts
 ↑
 第一列 = 待提交区   第二列 = 工作区
```

- `M ` （M 在左边）= 已 `add`，等着 `commit`
- ` M`（M 在右边）= 改了但还没 `add`

---

## `git log --oneline` 长什么样

```
352d52b 删掉测试文件夹 TEST
5c6d122 Merge pull request #1 from ZSQ77ZSQ/add-my-files
32877ff test
1f0d48b 新增 Git 速查模板文档
82463d9 初始提交：工程骨架 + 协作文档
```

**左边 7 位是提交编号，右边是你写的说明。**

---

# 第 5 章 · 两个人协作规则

## 5.1 最重要的三条铁律

```
① 开工先 pull，收工就 push
② 做完一小块就提交，别攒一天
③ 不要两个人同时改同一个场景文件
```

---

## 5.2 分工原则：各改各的文件

**Cocos 项目最容易冲突的不是代码，是场景文件。**

### ✅ 安全的分工方式

| 人 | 负责 |
|---|---|
| A | `assets/core/`（数值逻辑） |
| B | `assets/scripts/`（界面脚本） |

**两人改的文件完全不重叠 → 永远不会冲突。**

### ❌ 危险的协作方式

```
两个人同时打开 main.scene 改
→ A 加了个按钮，B 加了个标签
→ B 先 push，A 再 push 时被拒绝
→ A pull 下来发现场景文件冲突
→ 场景是 JSON，没法自动合并，只能二选一
```

### 🔬 为什么场景文件这么特殊（技术原因）

**因为场景里的节点是用"数组下标"互相引用的。**

场景文件 `main.scene` 是个 JSON 数组，节点之间这样互相指向：

```json
[
  { "__id__": 0, "name": "Canvas",  "_children": [ {"__id__": 1} ] },   ← 第 0 个
  { "__id__": 1, "name": "Camera",  "_parent":   {"__id__": 0} }       ← 第 1 个
]
                                                              ↑
                                              这个数字 = 它在数组里的位置
```

**你加一个节点，后面所有节点的编号全都要挪一位。**

```
A 本地：  [0]Canvas  [1]Camera  [2]A加的按钮
B 本地：  [0]Canvas  [1]B加的标签  [2]Camera
Git 合并：把两边拼起来 → 编号全乱 → 引用全部指错 → 场景损坏
```

**这就是为什么 Git 合并 `.ts` 代码没问题，合并 `.scene` 会炸。**

### 万一必须改同一个场景

**约定好顺序**：A 改完 → push → 在群里说一声 → B 先 pull 再改。

**或者**：把改动拆开 —— **A 只改脚本（`.ts`），场景里的节点统一由 B 添加。**

> 💡 代码文件（`.ts`）随便改，Git 能自动合并。**要小心的是 `.scene` 和 `.prefab`。**

---

## 5.3 `.meta` 文件规则（新手最容易搞错的地方）

**Cocos 会给每个资源生成一个 `.meta` 文件：**

```
assets/scenes/main.scene
assets/scenes/main.scene.meta      ← 必须一起提交
assets/core/crops.ts
assets/core/crops.ts.meta          ← 必须一起提交
```

### 🔬 为什么这个文件这么重要

**`.meta` 里存着这个资源的 UUID（一串唯一编号）。**

关键点：**场景里引用一张图，引用的不是路径，而是那张图的 UUID。**

```
你看到的是：  Canvas 上有个 Sprite，用的是 farm.png
Cocos 实际存的是：Canvas 上有个 Sprite，用的是 3f8a2b1c-...-9e4d
                                              ↑ 这个 UUID 存在 farm.png.meta 里
```

**所以如果 `.meta` 丢了或变了：**

```
Cocos 重新生成一个 .meta → 里面是个新的 UUID
        ↓
场景里原来记的 3f8a2b1c... 找不到了
        ↓
所有引用这张图的地方全部变成"丢失资源"（红色感叹号）
```

### 三条铁律

| 规则 | 说明 |
|---|---|
| ✅ **永远和资源一起提交** | 用 `git add -A` 会自动带上，不用操心 |
| ❌ **绝不手动编辑** `.meta` | 里面的 uuid 是 Cocos 生成的，改了会丢引用 |
| ❌ **绝不只删资源不删 meta** | 会留下孤儿 meta，编辑器报错 |

**删除资源时，在 Cocos Creator 编辑器里删**（它会自动处理 `.meta`），不要用资源管理器直接删。

**万一 meta 冲突了**：**删掉 `.meta`，重启 Cocos Creator，它会重新生成一个。** 场景里该节点的引用可能会丢，需要手动重新挂一次。

---

## 5.4 不要提交的东西

**`.gitignore` 已经自动挡住了，不用管：**

| 目录 | 为什么不能提交 |
|---|---|
| `library/` | 引擎导入缓存，几万个文件，会自动重建 |
| `temp/` | 编译临时文件 |
| `build/` | 构建产物 |
| `profiles/` | 个人偏好设置 |
| `.creator/` | 编辑器本地配置 |

**判断标准**：**能靠 `assets/` 重新生成的东西，就不提交。**

---

## 5.5 Cocos Creator 版本必须完全一致

```
所有人都必须是 3.8.8
```

**版本不同会导致**：工程打不开、场景错乱、资源导入失败。

**检查方法**：Cocos Creator 标题栏 / 关于页面。

---

## 5.6 冲突是什么样子的

**冲突不会自己发生，只会发生在 `git pull` 的那一刻。**

```
你改了文件 A
队友也改了文件 A
队友先 push 上去了
你 pull 的时候 → Git 说：我不知道该听谁的
```

**报错长这样：**

```
CONFLICT (content): Merge conflict in assets/core/crops.ts
Automatic merge failed; fix conflicts and then commit the result.
```

**别慌。** 冲突不是错误，只是 Git 在问你"这两个版本你要哪个"。下面按文件类型分成三种情况。

---

## 5.7 情况 A：代码文件（`.ts`）冲突 —— 好解决 ⭐

**打开冲突的文件，会看到这样的标记：**

```typescript
<<<<<<< HEAD
export const RADISH_GROW_SEC = 60;      // ← 你的版本
=======
export const RADISH_GROW_SEC = 90;      // ← 队友的版本
>>>>>>> origin/main
```

**三步搞定：**

**第 1 步**：把文件改成你想要的样子 —— 删掉 `<<<<<<<`、`=======`、`>>>>>>>` 这三行标记，中间保留你想要的内容（可以两边都留、也可以改成第三种写法）。

```typescript
export const RADISH_GROW_SEC = 60;      // ← 改完就是这样，标记全没了
```

**第 2 步**：

```bash
git add -A
git commit -m "解决 crops.ts 的冲突"
git push
```

**第 3 步**：确认成功。

```bash
git status
```

看到 `nothing to commit, working tree clean` 就成了。

> 💡 **怎么少遇到冲突**：小步提交、勤 push。改一小块就推一次，冲突的窗口就小。

---

## 5.8 情况 B：场景 / 预制体（`.scene` `.prefab`）冲突 —— 很麻烦 🔴

**绝对不要手动去改冲突标记。**

原因见 5.2 节：场景文件里 `{"__id__": 数字}` 是数组下标，两边同时改，编号全部错位，**手动合并出来的文件 99% 是坏的**，而且很难查。

**正确做法：二选一，然后重做。**

```bash
# 保留「你自己」这一版的场景
git checkout --ours assets/scenes/main.scene

# 或者保留「队友」那一版
git checkout --theirs assets/scenes/main.scene
```

然后：

```bash
git add -A
git commit -m "解决 main.scene 冲突，保留我的版本"
git push
```

**被丢掉的那部分改动，要去 Cocos Creator 编辑器里手动再做一遍。**

> 🔴 **所以真正的解决方案不是"怎么修"，而是"别让它发生"** —— 回到 5.2 节：**约定好同一时间只有一个人改同一个场景。**

---

## 5.9 情况 C：改乱了，想全部推翻重来

**丢掉所有还没提交的修改：**

```bash
git reset --hard HEAD
```

**连新增的、还没被 Git 跟踪的文件也一起删掉：**

```bash
git clean -nd     # ⚠️ 先预览！会列出「将要删除」的文件，不会真删
git clean -fd     # 确认没问题了，再真删
```

> 🔴 **`git clean -fd` 删掉的文件不可恢复** —— 它们从来没进过 Git，GitHub 上也没有。
> **永远先跑 `git clean -nd` 看一眼。**

**只丢掉某几个文件的修改**（其它文件不动）：

```bash
git restore assets/core/crops.ts
```

---

## 5.10 大文件提醒（Git LFS）

**GitHub 的硬限制：**

| 单个文件大小 | 结果 |
|---|---|
| < 50 MB | ✅ 正常 |
| 50 ~ 100 MB | ⚠️ 会警告，但能推 |
| **> 100 MB** | ❌ **直接拒绝推送** |

2D 小游戏的美术资源一般不会超。**但如果以后要加背景音乐、大量高清图**，单个文件可能变大。

**真遇到了再用 Git LFS（大文件存储）：**

```bash
git lfs install
git lfs track "*.psd"
git lfs track "*.wav"
git add .gitattributes
```

> ⚠️ **一旦启用了 LFS，队友也必须装 `git lfs`**，否则他 clone 下来的是一堆"指针文件"而不是真资源，游戏会缺素材。
> **启用之前一定要先跟队友说一声。**

---

# 第 6 章 · 出错了怎么办

## 6.1 `git push` 被拒绝

**【报错】**

```
! [rejected]  main -> main (non-fast-forward)
error: failed to push some refs
```

**【原因】** 别人先 push 了，你本地落后了。

**【解决】**

```powershell
git pull --rebase
git push
```

**【绝对不要】** 用 `git push --force`（会把别人的工作抹掉）。

---

## 6.2 `pull` 时提示有本地改动

**【报错】**

```
error: Your local changes to the following files would be overwritten by merge:
        assets/core/crops.ts
Please commit your changes or stash them before you merge.
```

**【原因】** 你有还没提交的改动，pull 会覆盖它们。

**【方法 A：先提交，再拉】**

```powershell
git add -A
git commit -m "先保存一下当前进度"
git pull
```

**【方法 B：暂时收起来，拉完再放回】**

```powershell
git stash        # 把改动暂时收起来
git pull         # 拉取
git stash pop    # 把改动放回来
```

---

## 6.3 连不上 GitHub

**【报错】**

```
fatal: unable to access 'https://github.com/...': 
Failed to connect to github.com port 443 after 21000 ms
```

**【原因】** 99% 是 **Clash 没开**。

**【解决】**

1. 打开 Clash，确认它正在运行
2. 确认端口是 `7890`
3. 重试

**【还是不行就检查配置】**

```powershell
git config --global --list | Select-String proxy
```

**应该看到：**

```
http.proxy=http://127.0.0.1:7890
https.proxy=http://127.0.0.1:7890
```

**没有的话补上：**

```powershell
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
```

---

## 6.4 `403 Permission denied`

**【报错】**

```
remote: Permission to ZSQ77ZSQ/2D-.git denied to xxx.
fatal: ... The requested URL returned error: 403
```

**【原因】** 你还没被加为协作者。

**【解决】** 让仓库主人加你：Settings → Access → Add people → 权限选 **Write** → 你接受邮件邀请。

---

## 6.5 卡在 vim 编辑器里出不来

**【什么时候会遇到】** `git commit` 时忘了写 `-m`，Git 打开了一个编辑器。

**【屏幕上会是】** 一片黑，左边有很多 `~` 符号，左下角有文件名。

**【救命操作】**

```
① 按一下 Esc 键
② 输入  :wq
③ 按回车
```

就会保存并退出。

**想放弃这次提交**：把 `:wq` 换成 `:q!`。

---

## 6.6 提交了不该提交的东西

**【比如】** 不小心把 `library/` 提交上去了

**【解决】**

```powershell
git rm -r --cached library/
git commit -m "移除误提交的 library"
git push
```

**`--cached` 的意思**：只从 Git 里移除，**不删你硬盘上的文件**。

**然后确认 `.gitignore` 里有这一条**：

```powershell
Add-Content .gitignore "library/"
```

---

## 6.7 想回到之前的某个版本

**【先看历史，找到要回去的编号】**

```powershell
git log --oneline
```

**【想看看那个版本长什么样（不修改任何东西）】**

```powershell
git checkout 352d52b
```

看完回到最新：

```powershell
git checkout main
```

**【想撤销某次提交的改动（保留历史）】**

```powershell
git revert 352d52b
git push
```

> ⚠️ **不要用 `git reset --hard` 去撤销已经 push 的提交** —— 会造成本地和远程分叉，很麻烦。

---

## 6.8 万能求救

**如果你不知道怎么办了：**

```powershell
git status
```

**这不会修改任何东西，只会告诉你现在的状态。** 把输出发给懂的人（或者 AI）看，就能知道怎么处理。

**另外记住：只要还没 push，你怎么折腾都能恢复。** 慌的时候先别 push。

---

# 第 7 章 · 命令总表

## 日常五条

| 命令 | 作用 |
|---|---|
| `git pull` | 拉取别人的更新 |
| `git add -A` | 把所有改动放进待提交清单 |
| `git commit -m "说明"` | 打包存档 |
| `git push` | 上传到 GitHub |
| `git status` | 查看当前状态 |

## 查看

| 命令 | 作用 |
|---|---|
| `git status` | 完整状态 |
| `git status -s` | 简写状态 |
| `git diff` | 看具体改了什么 |
| `git diff --stat` | 看每个文件改了几行 |
| `git log --oneline` | 提交历史 |
| `git remote -v` | 远程地址 |
| `git branch -a` | 所有分支 |

## 提交

| 命令 | 作用 |
|---|---|
| `git add -A` | 全部改动 |
| `git add 文件` | 单个文件 |
| `git add 文件1 文件2` | 多个文件 |
| `git add 目录/` | 一个目录 |
| `git rm 文件` | 删除文件 |
| `git rm -r 目录` | 删除文件夹 |
| `git mv 旧名 新名` | 重命名 |
| `git commit -m "说明"` | 提交 |
| `git commit --amend --no-edit` | 补进上次提交 |
| `git commit --amend -m "新说明"` | 改上次的说明 |

## 撤销

| 命令 | 作用 | 危险度 |
|---|---|---|
| `git restore --staged 文件` | 从待提交清单拿出来 | 🟢 安全 |
| `git restore 文件` | 丢弃文件改动 | 🟡 改动会没 |
| `git stash` | 暂时收起改动 | 🟢 安全 |
| `git stash pop` | 把收起的改动放回 | 🟢 安全 |
| `git reset --soft HEAD~1` | 撤销上次提交（保留改动） | 🟢 安全 |
| `git reset --hard HEAD~1` | 撤销上次提交（**丢弃改动**） | 🔴 危险 |
| `git revert 编号` | 反向提交某次改动 | 🟢 安全 |

## 配置

| 命令 | 作用 |
|---|---|
| `git config --global user.name "名字"` | 设置名字 |
| `git config --global user.email "邮箱"` | 设置邮箱 |
| `git config --global http.proxy http://127.0.0.1:7890` | 设代理 |
| `git config --global core.quotepath false` | 中文文件名正常显示 |
| `git config --global --list` | 查看所有配置 |

---

# 第 8 章 · 绝对不要做的事

## 🔴 1. 不要用 `git push --force`

```powershell
git push --force            # ❌ 不要
git push -f                 # ❌ 不要（简写）
```

**它会强行用你本地的内容覆盖远程，别人的提交直接消失。**

**唯一例外**：你 100% 确定远程的东西是错的必须丢掉，**而且提前跟对方说了**。

**被拒绝时的正确做法**：

```powershell
git pull --rebase
git push
```

---

## 🔴 2. 不要提交 `library/` `temp/` `build/`

**它们体积巨大且能自动重建。** `.gitignore` 已经挡住了，**别用 `git add -f` 强行推**。

---

## 🔴 3. 不要手动改 `.meta` 文件

**里面的 uuid 是 Cocos 生成的。** 改了会丢资源引用，场景会变空白。

---

## 🔴 4. 不要两个人同时改同一个 `.scene`

**场景是 JSON，冲突了无法自动合并。** 约定好顺序，或者分工改不同的文件。

---

## 🔴 5. 不要在没 `pull` 的情况下开始改

**先 `git pull`，再动手。** 顺序反了就要处理冲突。

---

## 🔴 6. 不要攒一天再提交

**做完一小块就提交。** 攒得越多，冲突越难解。

---

## 🔴 7. 不要用中文引号

```powershell
git commit -m “说明”        # ❌ 中文引号，报错
git commit -m "说明"        # ✅ 英文引号
```

中文输入法下容易打错，注意是**直的** `"`。

---

# 附录 A · 本项目信息

| 项目 | 内容 |
|---|---|
| 仓库名 | `2D-` |
| 仓库地址 | https://github.com/ZSQ77ZSQ/2D- |
| 默认分支 | `main` |
| 可见性 | public（公开） |
| 本地目录（ZSQ） | `D:\My game\ZSQ` |
| Cocos Creator 版本 | **3.8.8（必须完全一致）** |
| 代理 | `127.0.0.1:7890`（Clash） |

## 仓库目录说明

```
2D-/
├── assets/                 ← 游戏资源（唯一的真相来源）
│   ├── core/               ← 纯 TypeScript 逻辑（作物表、经济系统）
│   ├── scenes/             ← 场景文件
│   └── scripts/            ← 挂在节点上的脚本
├── docs/                   ← 文档（本文件在这里）
│   ├── Git使用手册.md
│   └── GIT协作指南.md
├── settings/               ← 工程设置（提交，但一般不手动改）
├── .gitattributes          ← 换行符和二进制文件规则
├── .gitignore              ← 不提交名单
├── README.md               ← 项目说明
├── package.json            ← 工程标识
└── tsconfig.json           ← TypeScript 配置
```

**不提交的**（本地会有，GitHub 上没有）：`library/`、`temp/`、`build/`、`profiles/`、`.creator/`

---

# 附录 B · 名词解释

| 名词 | 英文 | 意思 |
|---|---|---|
| **仓库** | Repository / Repo | 存放代码的地方。本项目仓库是 `2D-` |
| **远程仓库** | Remote | GitHub 上的那一份 |
| **本地仓库** | Local | 你电脑上的那一份 |
| **克隆** | Clone | 第一次把远程仓库下载到本地 |
| **拉取** | Pull | 把别人的更新拿到本地 |
| **推送** | Push | 把自己的更新传到远程 |
| **提交** | Commit | 给当前状态拍一张快照（存档） |
| **暂存区** | Staging Area | "待提交清单"。`add` 是往里放，`commit` 是打包 |
| **分支** | Branch | 平行的开发线。本项目主线叫 `main` |
| **冲突** | Conflict | 两个人改了同一处，Git 不知道听谁的 |
| **工作区** | Working Tree | 你正在编辑的文件 |
| **HEAD** | — | 指向"当前所在的提交"，可以理解为"你现在在哪" |
| **origin** | — | 远程仓库的默认名字 |
| **`.meta` 文件** | — | Cocos 给每个资源生成的索引文件，必须和资源一起提交 |

---

# 附录 C · 一页纸速查（贴在显示器边上）

```powershell
# ══════════════ 开工 ══════════════
cd "D:\My game\ZSQ"
git pull

# ══════════════ 干活 ══════════════
#   （正常改文件，不用管 Git）

# ══════════════ 收工 ══════════════

# ── 全部提交 ──
git add -A
git commit -m "说明文字"
git push

# ── 只提交一个文件 ──
git add 1.txt
git commit -m "说明文字"
git push

# ── 只提交几个文件 ──
git add 1.txt 2.txt assets/core/crops.ts
git commit -m "说明文字"
git push

# ── 删除文件 / 文件夹 ──
git rm 文件.txt
git rm -r 文件夹
git commit -m "说明文字"
git push

# ══════════════ 出问题 ══════════════

# ── push 被拒绝 ──
git pull --rebase
git push

# ── 连不上 GitHub ──
#   → 检查 Clash 开没开

# ── 不知道怎么办 ──
git status
```

---

**有问题随时问。或者直接说"帮我提交，说明写 XXX"，让 AI 代劳。**
