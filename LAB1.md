# 实验 1 记录：第 <组号> 组

现场检查时将打开本文件，按其中的链接核对。证据一律以链接或文本形式给出，不要只贴截图。将 `<填写>` 替换为本组的内容。main 受保护之后，修改本文件也须通过 PR。

## 1. 基本信息

| 项 | 内容 |
|---|---|
| 小组 | 第 <组号> 组 |
| 仓库地址 | <https://github.com/…> |
| 代码载体 | 统一示例 / 自己的项目（<项目名与语言>） |
| runner 与语言版本 | `ubuntu-24.04`，<Go 或其他语言的版本> |
| 合并方式 | <squash / merge commit / rebase> |

## 2. 任务 0：本地基线

| 问题 | 观察结果 |
|---|---|
| 第一次 `make package` 得到的 BUILD_INFO（粘贴） | `name=enroll-conflict`<br>`version=1.0.0`<br>`build_no=1`<br>`run_id=local`<br>`commit=7b63b41c13f061439997b0e6e4eaa2af922a7aa1`<br>`go=go1.26.9` |
| 修改一行、不提交，再次打包：BUILD_INFO 是否变化？二进制文件的 sha256 是否变化？（贴出两次的 sha256） | BUILD_INFO **没有变化**（`diff` 输出“BUILD_INFO 没有变化”）。二进制 sha256 **变化**：第一次为 `f23447d406c42af7dd35e23c2d1b2f15484cfb709cc9b4bb1262643694b8528e`；第二次为 `92e3e88728c3beb58788b99be3bf1e5a39a1d58efdadb42a16f8fc0300319c8b`。 |
| 两次 `go version -m` 输出中的 vcs 行有何不同？ | `vcs.revision` 和 `vcs.time` 相同（均为 HEAD 的 `7b63b41c…` 和 `2026-10-12T16:26:07Z`）；**`vcs.modified` 由 `false` 变为 `true`**。 |
| 仅凭 BUILD_INFO，能否回答该包是什么、在哪台机器上、何时、用哪份代码构建？哪一项信息会出错？ | **是什么**：能，`name=enroll-conflict`、`version=1.0.0`、`build_no=1` 都在。**在哪台机器上**：不能，BUILD_INFO 没有主机名、IP 或架构信息。**何时**：不能，没有构建时间戳；`run_id=local` 是手工填的值，不是真实运行 ID。**用哪份代码**：名义上有 `commit=7b63b41c…`，但它是 Git HEAD 的提交号，不包含未提交修改；第二次构建源码已变而 BUILD_INFO 完全没变，说明该字段不能代表实际构建内容。会出错的是“用哪份代码构建”这一项，其次是 `run_id` 无法追溯真实构建。 |
## 3. 证据

| 项 | 链接或文本 |
|---|---|
| 规则集里启用的规则 | <列出规则名、审批人数和必需检查的名称> |
| 组长直接 `git push origin main` 被拒的终端输出 | <粘贴文本> |
| 第一轮：失败的 PR 运行 | <链接> |
| 第一轮：修复后通过的 PR 运行 | <链接> |
| 第一轮：合入 main 后的运行（含制品） | <链接> |
| 第二轮：找到的缺陷，以及现有测试为何无法发现它 | <一两句话> |
| 第二轮：仅含缺陷时仍然通过的运行 | <链接> |
| 第二轮：补测试后失败的运行 | <链接> |
| 第二轮：撤销缺陷后通过的运行 | <链接> |
| 未合入分支上的 tag 被 release gate 拒绝的运行 | <链接> |
| Release v1.0.0 | [v1.0.0](https://github.com/workonemptymirror/ops-lab1-10/releases/tag/v1.0.0) |
| Release v1.0.1 | [v1.0.1](https://github.com/workonemptymirror/ops-lab1-10/releases/tag/v1.0.1) |

演练关联记录：第一轮修复 PR 为 [#5](https://github.com/workonemptymirror/ops-lab1-10/pull/5)，第二轮 PR 为 [#6](https://github.com/workonemptymirror/ops-lab1-10/pull/6)。已核对第二轮存在 [ci #12 失败运行](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39740432508/job/119696068823)和 [ci #13 成功运行](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39741319996/job/119698950427)，两次运行均关联 PR #6，不能用作第一轮 PR #5 的失败与通过证据。ci #12 具体失败步骤及其是否对应“补测试后失败”，仍需日志确认；第二轮“仅含缺陷时仍通过”的运行也待补。

## 4. 追溯与运行

对 v1.0.0 和 v1.0.1 各运行一次 `scripts/trace.sh`，粘贴输出，并填写下表。

| 项 | v1.0.0 | v1.0.1 |
|---|---|---|
| 版本号 | `1.0.0` | `1.0.1` |
| 构建号 | `1` | `3` |
| 提交号（SHA） | `2256e1691516fcf08ad693c139b972808c72a837` | `6a9d75cfcad7fd34153d98ad44fb041bd31d3c92` |
| 引入该提交的 PR | [#6：feat: 移除时段合法性检查](https://github.com/workonemptymirror/ops-lab1-10/pull/6)，PR 页面记录的合并提交为 `2256e1691516fcf08ad693c139b972808c72a837` | [#8：docs/version-and-rollback](https://github.com/workonemptymirror/ops-lab1-10/pull/8)，本地提交历史与 trace 输出均显示该提交为 PR #8 的 merge commit |
| 构建它的 Actions 运行（`trace.sh` 给出的链接） | [release #1，run_id=39761260459](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39761260459) | [release #3，run_id=39776115322](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39776115322) |
| 制品 sha256（发布当时，取自 Release 里的 .sha256） | `fb8d54ad4a42a754ca2b161792efd8070b9dadc8913745995adb95ea1afb1c9c` | `be10a6fe3eb0972b62b6b4cef138ee5c176dd6641966f5d0e39670f9d6f0b718` |

仅凭构建号能否唯一确定是哪次运行？v1.0.0 的构建号在 `ci` 和 `release` 的运行列表中各对应哪次运行？

不能。`github.run_number` 按工作流分别计数，v1.0.0 的构建号 `1` 在 `release` 中对应 [release #1](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39761260459)，在 `ci` 中则对应 [ci #1](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39562902328)（PR #1 的 CI 运行）。这两次运行的 ID 分别为 `39761260459` 和 `39562902328`，不是同一次运行。制品中的 `run_id=39761260459` 可以定位真正构建 v1.0.0 的 release 运行，不能将同号的 ci 运行当作该制品的构建来源。

发布 v1.0.1 之后重新下载 v1.0.0 的附件，计算得到的 sha256：`fb8d54ad4a42a754ca2b161792efd8070b9dadc8913745995adb95ea1afb1c9c`。与上表中发布当时的值是否一致？一致，与 v1.0.0 首次发布时记录完全相同，说明两次观测的附件内容一致；此比较不证明两个观测时刻之间从未发生过临时替换。

本次下载后实际计算的 SHA-256 如下。两个值分别与对应版本首次发布记录一致；两次 trace 输出也均显示附件与对应校验文件匹配（`OK`）。

```text
fb8d54ad4a42a754ca2b161792efd8070b9dadc8913745995adb95ea1afb1c9c  enroll-conflict_1.0.0_linux_amd64.tar.gz
be10a6fe3eb0972b62b6b4cef138ee5c176dd6641966f5d0e39670f9d6f0b718  enroll-conflict_1.0.1_linux_amd64.tar.gz
```

`trace.sh` 输出（v1.0.0）：

```text
== 1. 校验和
enroll-conflict_1.0.0_linux_amd64.tar.gz: OK
== 2. 制品里的 BUILD_INFO
name=enroll-conflict
version=1.0.0
build_no=1
run_id=39761260459
commit=2256e1691516fcf08ad693c139b972808c72a837
go=go1.26.9
== 3. 提交 2256e1691516fcf08ad693c139b972808c72a837
作者: shannonpurnomo <purnomo.shannon@gmail.com>
时间: 2026-10-22T15:37:41+08:00
标题: feat: 优化冲突检测逻辑
主干: 该提交在 origin/main 的历史上
指向它的 tag: v1.0.0
版本号: 1.0.0    构建号: 1
提交页面（含关联的 PR）: https://github.com/workonemptymirror/ops-lab1-10/commit/2256e1691516fcf08ad693c139b972808c72a837
构建它的运行: https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39761260459
```

`trace.sh` 输出（v1.0.1）：

```text
== 1. 校验和
enroll-conflict_1.0.1_linux_amd64.tar.gz: OK
== 2. 制品里的 BUILD_INFO
name=enroll-conflict
version=1.0.1
build_no=3
run_id=39776115322
commit=6a9d75cfcad7fd34153d98ad44fb041bd31d3c92
go=go1.26.9
== 3. 提交 6a9d75cfcad7fd34153d98ad44fb041bd31d3c92
作者: TOOTHACHE <toothache@sjtu.edu.cn>
时间: 2026-10-22T20:01:21+08:00
标题: Merge pull request #8 from workonemptymirror/docs/version-and-rollback
主干: 该提交在 origin/main 的历史上
指向它的 tag: v1.0.1
版本号: 1.0.1    构建号: 3
提交页面（含关联的 PR）: https://github.com/workonemptymirror/ops-lab1-10/commit/6a9d75cfcad7fd34153d98ad44fb041bd31d3c92
构建它的运行: https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39776115322
```

任务 6 第 4 步，两次 `/version` 的输出：

```text
运行 v1.0.1 时：
{"version":"1.0.1","commit":"6a9d75cfcad7fd34153d98ad44fb041bd31d3c92","build_no":"3","go":"go1.26.9"}

换回 v1.0.0 后：
{"version":"1.0.0","commit":"2256e1691516fcf08ad693c139b972808c72a837","build_no":"1","go":"go1.26.9"}
```

## 5. 数据

时间统一为北京时间（UTC+8）。PR 创建与合并时间、提交时间、Release 发布时间和 CI 时长取自 GitHub 公开页面；提交到发布的间隔按“对应 Release 发布时间减去 PR 第一个提交的提交时间（committedDate）”计算，不使用 PR 创建时间代替首次提交时间。

第一轮 gate 演练的时间线：

| 事件 | 时间 | 链接 |
|---|---|---|
| 发起 PR（最初失败分支） | 2026-10-22 00:42:20 | [PR #3](https://github.com/workonemptymirror/ops-lab1-10/pull/3) |
| 第一次 `ci` 失败（运行结束） | 待补精确结束时间；页面显示触发时间为 2026-10-22 00:42:24、运行时长 12 秒，此处不将触发时间误作结束时间 | [ci #7](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39637166239) |
| 发起第一轮修复 PR | 2026-10-22 11:29:53 | [PR #5](https://github.com/workonemptymirror/ops-lab1-10/pull/5) |
| 推送修复提交 | 待核对实际推送时间；PR #5 的提交列表中存在 `fix: 修复 weeksOverlap 逻辑`（`db7b279…`），提交时间不能直接替代实际推送时间 | [PR #5 提交列表](https://github.com/workonemptymirror/ops-lab1-10/pull/5/commits) |
| `ci` 通过（运行结束） | 本次采集未找到可确认属于第一轮修复后、指向 main 的 PR 成功运行，待补证据 | [PR #4](https://github.com/workonemptymirror/ops-lab1-10/pull/4)、[PR #5](https://github.com/workonemptymirror/ops-lab1-10/pull/5) |
| 合入 main | 本次采集未确认第一轮修复通过 PR 合入 main 的记录；不能用 PR #5 的合并时间代替 | [PR #4](https://github.com/workonemptymirror/ops-lab1-10/pull/4)、[PR #5](https://github.com/workonemptymirror/ops-lab1-10/pull/5) |

记录核对：PR #3、#4 在本次采集时仍为 Open；PR #5 虽显示 Merged，但目标分支是 `fitur/gate-round-1-fix`，不是 `main`，合并时间为 2026-10-22 14:04:29。因此上述记录尚不足以证明第一轮已完成“失败 → 修复通过 → 经 PR 合入 main”的全过程，也不足以精确计算人工修改时间和 CI 等待时间。该部分应在补齐实际运行与合并记录后完善，不将第二轮 PR #6 的成功结果挪作第一轮证据。

提交到发布的间隔，记录三个合入 main 的 PR：

| PR | 第一个提交的时间 | 合入时间 | 对应 Release 的发布时间 | 提交到发布的间隔 |
|---|---|---|---|---|
| [#1](https://github.com/workonemptymirror/ops-lab1-10/pull/1) | [2026-10-21 15:25:18](https://github.com/workonemptymirror/ops-lab1-10/commit/8335d115dba85460c5dbeddcce7aaf035bab92cb) | 2026-10-21 15:41:36 | [v1.0.0：2026-10-22 18:12:03](https://github.com/workonemptymirror/ops-lab1-10/releases/tag/v1.0.0) | 26 小时 46 分 45 秒 |
| [#2](https://github.com/workonemptymirror/ops-lab1-10/pull/2) | [2026-10-21 15:53:32](https://github.com/workonemptymirror/ops-lab1-10/commit/630c1137e77a464a4d1814b11b150bc0e52bf926) | 2026-10-21 16:06:53 | [v1.0.0：2026-10-22 18:12:03](https://github.com/workonemptymirror/ops-lab1-10/releases/tag/v1.0.0) | 26 小时 18 分 31 秒 |
| [#8](https://github.com/workonemptymirror/ops-lab1-10/pull/8) | [2026-10-22 19:45:58](https://github.com/workonemptymirror/ops-lab1-10/commit/1a7686d28355e00b439966d6e3c11020336c0986) | 2026-10-22 20:01:21 | [v1.0.1：2026-10-22 20:10:07](https://github.com/workonemptymirror/ops-lab1-10/releases/tag/v1.0.1) | 0 小时 24 分 9 秒 |

上表三个 PR 各只有一个提交；已通过 Git 提交历史确认其合并提交包含在对应 Release 的提交历史中。PR #1、#2 对应首次包含它们的 v1.0.0，PR #8 对应 v1.0.1。间隔包含修改、评审、等待和人为安排发布时间等因素，不能将全部间隔归因于 CI。

`ci` 工作流的运行时长（从 Actions 页面读取）：以下列出本次采集时工作流列表可见的全部 19 次运行（ci #1～#19），包括成功和失败运行；后续如有新增运行需继续补充。

| 运行 | 事件（PR / push） | 时长 | 备注 |
|---|---|---|---|
| [ci #1](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39562902328) | PR | 41s | 通过；PR #1 |
| [ci #2](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39563778346) | push | 49s | 通过；main |
| [ci #3](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39566529841) | PR | 15s | 通过；PR #2 |
| [ci #4](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39566844303) | push | 22s | 通过；main |
| [ci #5](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39575114517) | push | 21s | 通过；main |
| [ci #6](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39575693125) | push | 19s | 通过；main |
| [ci #7](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39637166239) | PR | 12s | 失败；PR #3 |
| [ci #8](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39716609805) | PR | 12s | 失败；PR #4 |
| [ci #9](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39731706345) | PR | 11s | 失败；PR #4 |
| [ci #10](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39734892565) | PR | 10s | 失败；PR #6 |
| [ci #11](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39735238868) | PR | 8s | 失败；PR #6 |
| [ci #12](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39740432508) | PR | 17s | 失败；PR #6 |
| [ci #13](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39741319996) | PR | 20s | 通过；PR #6 |
| [ci #14](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39741812593) | push | 23s | 通过；main |
| [ci #15](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39762334382) | PR | 17s | 通过；PR #7 |
| [ci #16](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39774035939) | PR | 16s | 通过；PR #8 |
| [ci #17](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39775019456) | push | 19s | 通过；main |
| [ci #18](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39776588588) | PR | 11s | 通过；PR #7 |
| [ci #19](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39777111418) | PR | 15s | 通过；PR #7 |

这 19 次运行的时长范围为 8～49 秒，中位数为 17 秒，平均约 18.8 秒。失败运行可能在格式检查等较早步骤退出，PR 不打包而 main push 包含打包，因此该汇总仅描述本次观测，不能直接解释为测试耗时或 gate 带来的纯等待成本；样本量小，共享 runner 负载和排队也可能影响反馈时间。

## 6. 分析问题

每题先列出所用数据（可引用上面的表），再用一段话（4～8 句）写出结论，并说明结论的成立条件。

**问题 1：gate 的收益与成本。** 两轮演练中，gate 拦住了什么，没有拦住什么？用第 5 节的时间线计算：第一轮因失败多花了多少时间，其中等待 `ci` 的时间和人工修改的时间各占多少？一次 `ci` 大约需要多久，在“提交到发布的间隔”中占多大比例？第二轮补充测试之后，还有哪类问题是测试本身无法发现的？

<填写>

**问题 2：PR 中的构建与 main 上的构建。** 在 PR 触发的运行日志中，`github.sha` 与 `pull_request.head.sha` 是否相同？它们能否在 main 的历史中找到？答案与合并方式有关，请写明本组使用的方式。合并之后，main 上这次改动的提交号是哪个？据此说明，为什么待发布的制品只能从 main 上的提交或 tag 构建。

<填写>

**问题 3：可重复构建。** 在本地同一个提交上按下表构建，记录哈希值。④、⑤ 需临时去掉 Makefile 中 tar 的 `--mtime=@0`，完成后用 `git checkout -- Makefile` 恢复，不要提交。

本地实验环境：`go version go1.26.9 linux/amd64`；五次构建均使用提交 `6a9d75cfcad7fd34153d98ad44fb041bd31d3c92`，开始时 `git status --short` 无输出，工作区干净。按本次实验步骤，固定 `VERSION=1.0.0`、`RUN_ID=local-q3`、`COMMIT=6a9d75cfcad7fd34153d98ad44fb041bd31d3c92`，①、②、④、⑤使用 `BUILD_NO=100`，③改为 `BUILD_NO=101`；④、⑤临时移除 tar 的 `--mtime=@0`，其间不切换提交。

| 构建 | 条件 | 二进制 sha256 | tar.gz sha256 |
|---|---|---|---|
| ① | `make package`，`VERSION`、`BUILD_NO`、`RUN_ID`、`COMMIT` 均指定固定值 | `f548df6ff68ff429a26138740bd996e3fe8e49469b9b03e64a89f3526b6c40c4` | `c8b43078e70cba3296db14e7a3f9e32b8885300864a58ce14fb305368da3f13c` |
| ② | 与 ① 完全相同，再构建一次 | `f548df6ff68ff429a26138740bd996e3fe8e49469b9b03e64a89f3526b6c40c4` | `c8b43078e70cba3296db14e7a3f9e32b8885300864a58ce14fb305368da3f13c` |
| ③ | 仅将 `BUILD_NO` 换成另一个值 | `848eb92e7969cd8f4cb79ffd007a6762725b5d9a9753361053855b72fcf30e1c` | `5a1ad81d669dbb1b4087028e4e29840c7b13497b6bab52d11ac9fc6fd5f82a1f` |
| ④ | 去掉 `--mtime=@0`，其余同 ① | `64d49f810447fa47e13531f090d3c55e6f9a6f9f3359067125a57222ef4496cd` | `5d3ab1b0b85e92d2a972649cb78f9113feabfa803f714943c8316217c24cfddb` |
| ⑤ | 同 ④，间隔几秒后再构建一次 | `64d49f810447fa47e13531f090d3c55e6f9a6f9f3359067125a57222ef4496cd` | `4cd2b5bd02cddd880ad1598cd5105a251453269fddc4d684378256ed5e7ce6cc` |
| Actions | [ci #17](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39775019456)，同一次 main push 运行重跑；第二次尝试为 `run_attempt=2` | 重跑前：`936ec1216e614618e5ed0e0bcf35fff1cbc867a837fd604a89a20244f20c9ad0`<br>重跑后：`936ec1216e614618e5ed0e0bcf35fff1cbc867a837fd604a89a20244f20c9ad0` | 重跑前：`52ab0a0a03bd8957c17d8695746b1297614a253eb24fd1c3658cc2ebc1c68b9a`<br>重跑后：`52ab0a0a03bd8957c17d8695746b1297614a253eb24fd1c3658cc2ebc1c68b9a` |

哪些相同、哪些不同，原因分别是什么？④ 的二进制文件与 ① 是否相同？如果不同，请结合任务 0 中看到的 vcs 信息分析。据此说明制品中应当包含什么、不应包含什么。另外，`release` 在同一个提交上再次编译，而没有直接发布 `ci` 产出的主干制品：为什么这样做？这与 L-05 第 11 页的“一次构建、多处部署”是否冲突，代价是什么？

Actions 重跑记录：运行 [ci #17](https://github.com/workonemptymirror/ops-lab1-10/actions/runs/39775019456) 的事件为 `push`，ref 为 `refs/heads/main`，提交为 `6a9d75cfcad7fd34153d98ad44fb041bd31d3c92`。重跑前后均使用 `VERSION=0.0.0-main`、`BUILD_NO=17`、`RUN_ID=39775019456`；原始 BUILD_INFO 中 Go 为 `go1.26.9`，重跑日志显示 `run_number=17`、`run_id=39775019456`、`run_attempt=2`。本次为 push 事件，`head_sha` 为空，不代表提交缺失；构建使用的是 `sha`。上表记录的是二进制和压缩包本身的哈希，没有将 `.sha256` 校验文件自身的哈希误作压缩包哈希。

所用数据：上表①～⑤及 Actions 两次尝试的二进制与压缩包哈希，以及①、④的 `go version -m` 输出。两次 vcs 信息如下：

```text
①：
build   vcs=git
build   vcs.revision=6a9d75cfcad7fd34153d98ad44fb041bd31d3c92
build   vcs.time=2026-10-22T12:01:21Z
build   vcs.modified=false

④：
build   vcs=git
build   vcs.revision=6a9d75cfcad7fd34153d98ad44fb041bd31d3c92
build   vcs.time=2026-10-22T12:01:21Z
build   vcs.modified=true
```

①与②的二进制及压缩包哈希分别相同，说明在本次相同工具链、提交、干净工作区、构建参数与固定归档元数据的条件下，重复构建得到了相同字节内容，但两次观测不能推广为任意环境下均可复现。③只改变 `BUILD_NO`，两种哈希均发生变化，因为该参数既通过链接参数写入二进制，也写入 BUILD_INFO，因此提交相同并不等于构建输入完全相同。④恢复①的构建参数后，二进制仍与①不同，而 vcs 输出显示 revision、time 相同、`vcs.modified` 从 false 变为 true，说明未提交的 Makefile 修改改变了 Go 写入的工作区状态，不能把④与①的全部差异仅归因于 tar 时间。④与⑤的二进制哈希相同、压缩包哈希不同，结合两次均移除固定时间参数且间隔后打包的条件，说明归档文件的修改时间元数据也会影响压缩包内容，即使其中的可执行文件字节未变。Actions 的 ci #17 重跑后 `run_attempt=2`，`run_number` 与 `run_id` 保持不变，二进制和压缩包哈希分别与重跑前完全相同，为本次工作流在相同提交和参数下的可重复构建提供了实际证据，但不保证未来 runner 或 Actions 更新后仍有同样结果。制品应包含稳定且必要的版本、构建号、提交号、运行 ID 和工具链信息，正式发布应使用受控的干净工作区，并规范归档顺序、属主和时间，避免嵌入无必要的当前时间、随机值或主机路径等不稳定信息。当前 release 工作流重新编译，是为了把 tag 对应的正式版本写入二进制，而 ci 主干制品的版本是 `0.0.0-main`，所以二者虽然可能来自同一提交，实际构建参数与运行标识并不相同。这种在发布阶段再次构建的安排偏离了严格的“一次构建、多处部署”，代价是增加构建耗时和验证成本，且不能直接将主干制品的哈希视为正式 Release 的哈希；若要严格遵循该原则，应在最终版本构建完成后提升并部署同一份不可变制品。


**问题 4：发布与回退。** 假设 v1.0.1 发布后发现缺陷，应当如何处理？为什么不应删除 v1.0.1 的 tag、再用同一版本号重新发布？任务 6 中将服务从 v1.0.1 切换回 v1.0.0 时，使用的是哪个文件，是否需要重新构建？请用两次 `/version` 的输出说明。如果改为从源码重新构建 v1.0.0，得到的文件与原附件是否相同（参考问题 3）？最后，如果有人删除 v1.0.0 的附件，换上一个新文件和对应的新 `.sha256`，`trace.sh` 能否发现？本组在第 4 节记录的值能否发现？可以查阅 GitHub 的 Immutable releases 设置的用途。

所用数据：第 4 节两次 `/version` 输出中，v1.0.1 为提交 `6a9d75cfcad7fd34153d98ad44fb041bd31d3c92`、构建号 `3`，v1.0.0 为提交 `2256e1691516fcf08ad693c139b972808c72a837`、构建号 `1`，Go 版本均为 `go1.26.9`；本次下载的 v1.0.0 压缩包 SHA-256 为 `fb8d54ad4a42a754ca2b161792efd8070b9dadc8913745995adb95ea1afb1c9c`。

如果 v1.0.1 发布后发现缺陷，应先评估旧版本是否仍能兼容当前数据和运行环境，在可以安全回退时切换至已校验的 v1.0.0 制品，再通过 PR 修复并发布新的修订版本（例如 v1.0.2）。不应删除 v1.0.1 的 tag 并用同一版本号重新发布，因为这会使同一版本名称对应不同内容，破坏已有下载、校验记录和故障追溯。本次先停止 v1.0.1，再运行从 `enroll-conflict_1.0.0_linux_amd64.tar.gz` 解压得到的 `enroll-conflict`，直接使用旧版本 Release 制品，未重新编译。两次 `/version` 分别显示版本、提交和构建号由 `1.0.1 / 6a9d75c… / 3` 切换为 `1.0.0 / 2256e16… / 1`，与两个版本的 BUILD_INFO 对应，但该接口输出本身不能证明文件字节完全一致。若改为从源码重新构建 v1.0.0，不能仅凭提交相同就保证文件与原附件相同，还需核对工具链、构建参数、工作区状态和打包元数据；问题 3 中①与②在相同条件下哈希一致，但③改变构建号、④改变工作区状态后均产生不同二进制，说明重新编译并不自动等同于原 Release 附件。如果有人同时替换制品和对应 `.sha256`，并保留有效的 BUILD_INFO，现有 `trace.sh` 可能仍通过，因为脚本检查的是当前文件与当前校验文件的一致性，以及所声明提交是否存在并属于 main 历史，并未独立验证二进制确由该提交构建。第 4 节保存了首次发布时的哈希以及本次重新下载的哈希，两者一致；若后来换成内容不同的新文件，在原记录未被篡改的前提下，与该记录比较即可发现变化，即使新的配套校验文件也已被替换。[GitHub Immutable releases](https://docs.github.com/en/code-security/concepts/supply-chain-security/immutable-releases) 会锁定已发布版本的附件和关联 tag，限制附件修改与删除、tag 移动等操作，从发布端降低这类替换风险；本组是否启用该设置尚未核实。

## 7. 选做记录

| 选做 | 完成情况与证据 |
|---|---|
| O1 容器镜像 | <未做 / 链接、镜像摘要（digest）、运行容器的主机架构> |
| O2 缓存对反馈时间的影响 | <未做 / 预期、数据与结论> |
| O3 迁移到小组项目 | <未做 / 仓库链接> |

## 8. 成员贡献

每位成员一行，只填写 GitHub 用户名，无需填写姓名；行数应与小组人数一致。

| GitHub 用户名 | 发起并合入的 PR 编号 | 审批过的 PR 编号 |
|---|---|---|
| <用户名> | <编号> | <编号> |
