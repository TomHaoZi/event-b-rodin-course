# Rodin 操作手册

第一次打开 Rodin，建议先导入单车道桥工程，观察一辆车怎样改变系统状态，再在一个空工程里重做同一层模型。本手册从安装开始，带你建立上下文、机器和事件，查看证明义务，修改错误，最后导出能在另一台电脑上打开的工程。示例使用第二讲的真实组件名，后续两讲也沿用这套操作。

## 1　安装并打开工作区

1. 从 Rodin 项目发布目录选择 **3.10**。Windows 选择文件名以 `win32.win32.x86_64.zip` 结尾的包，Linux 选择 `linux.gtk.x86_64.tar.gz`，macOS 在“关于本机”查看芯片，M 系列选择 `macosx.cocoa.aarch64`，Intel 选择 `macosx.cocoa.x86_64`。文件名带 `org.rodinp.dev` 的包供开发插件使用。[官方发布目录](https://sourceforge.net/projects/rodin-b-sharp/files/Core_Rodin_Platform/3.10/)
2. 准备和程序架构相同的 **64 位 Java 21**。在终端执行 `java -version`，应看到 21 或兼容的较高版本。完整解压 Rodin，再从所在目录启动 Windows 的 Rodin 可执行文件、Linux 的 `rodin` 或 macOS 的 `Rodin.app`。遇到版本、架构或 macOS 首次启动问题，核对发布说明里的对应段落。[Rodin 3.10 安装与兼容性说明](https://wiki.event-b.org/index.php/Rodin_Platform_3.10_Release_Notes)
3. 在启动时的 **Workspace** 对话框选一个可写空目录，如个人目录下的 `RodinWorkspace`。工作区保存项目及界面状态。把下载并解压的课程包放在另一个目录，保留一份原始练习材料。首次启动如果看到 Welcome，关闭该页或进入 Workbench。
4. 打开 **Window → Perspective → Open Perspective → Other… → Event-B**。某些版本的菜单直接写 **Window → Open Perspective**。左侧能看到 **Event-B Explorer**、双击组件后能看到编辑器，就可以开始。如需找回某个面板，用 **Window → Show View → Other…** 搜索视图名称。

**已有 Rodin 的同学**可在 **Help → About Rodin Platform**（macOS 也可在应用菜单找到 About）确认版本。将旧版工作区升级到 3.10 前先复制一份，3.10 打开过的工作区无法直接降回旧版本。[升级说明](https://wiki.event-b.org/index.php/Rodin_Platform_3.10_Release_Notes)

## 2　先打开随包工程

1. 解压课程资料包，找到 `03-Rodin工程/L2_Bridge`，其内有 `.project`、`.buc` 和 `.bum`。文件管理器可能隐藏以点开头的文件。
2. 在 Rodin 选 **File → Import… → General → Existing Projects into Workspace → Next**。点 **Select root directory → Browse…**，选择 `03-Rodin工程`。若只想导入一项，直接选择 `L2_Bridge`。
3. 在 **Projects** 列表勾选所需工程，并勾选 **Copy projects into workspace**（若可选），点 **Finish**。这样练习用的副本位于工作区，改坏后可以从资料包重新导入。
4. 确认 **Project → Build Automatically** 已勾选。等待右下角的构建任务结束。展开 **L2_Bridge**，找到 **C0_Capacity** 和 **M0_Capacity**。视图可隐藏扩展名，精化模型也可能以树形折叠在父模型下。
5. 双击 **C0_Capacity**，看常量 `d` 及其公理。再双击 **M0_Capacity**，沿 **SEES → C0_Capacity**、**VARIABLES → n**、**EVENTS → ML_out** 三处核对。若能看到 `n < d` 和 `n ≔ n + 1`，导入的就是手册的示例。

| 在界面里看到什么 | 用它完成什么 |
|---|---|
| Event-B Explorer | 找工程、上下文、机器、精化链及 Proof Obligations |
| 编辑器中的模型正文、结构树或 Edit 页 | 添加元素，编辑名称、公式及注释 |
| Rodin Problems 或 Problems | 定位语法、类型、引用及初始化问题；双击消息跳到源位置 |
| Symbols、Rodin Keyboard | 查找数学符号，把 ASCII 键入转换成 Rodin 数学字符 |
| Proving 透视图 | 显示当前假设、待证目标、证明树及可施用的推理规则 |
| ProB 事件和状态视图（选装） | 选择一个可触发事件，查看执行前后变量与轨迹 |

## 3　从空白工程重做桥的总量模型

这一层只问一个问题：在大陆一侧放行一辆车，会不会使系统内车辆超过容量？系统范围包含桥和岛。常量 `d` 给出上限，变量 `n` 统计当前总量。车辆在桥与岛之间移动时，总量不变。这正是第二讲为什么从两个大陆边界事件开始。

### 3.1　创建工程与上下文

1. 选 **File → New → Event-B Project**，工程名填 `MyFirstBridge`，点 **Finish**。若 New 菜单中未列出该项，走 **File → New → Other…**，搜索 `Event-B`。使用不同的工程名，可与随包答案并排对照。
2. 在左侧右击新工程，选 **New → Event-B Component**，名称填 `C0_Capacity`，类型选 **Context**。点 **Finish** 后，编辑器标题应为 CONTEXT。
3. 在默认的结构化正文编辑器中，点上下文名称旁的小箭头左侧，右击 **Add Child**，选择 **Constant**，将标识符改为 `d`。按同样方法加两个 **Axiom**。若使用表单编辑器，在 **Edit** 页展开 **Constants** 和 **Axioms**，用对应的添加按钮新增行。
4. 分别填写下表。**Label** 只填标签，**Predicate** 只填公式。两条公理的 **Theorem** 均不勾选。保存用 **Ctrl+S**，macOS 用 **⌘S**。

| 元素 | Label 或 Identifier | Predicate | 为什么需要 |
|---|---|---|---|
| Constant | d | — | 一次系统运行中容量固定 |
| Axiom | axm0_1 | `d ∈ ℕ` | 类型为含零的自然数 |
| Axiom | axm0_2 | `0 < d` | 排除没有任何通车能力的容量 |

如果短时间显示“常量没有类型”，先把 `d ∈ ℕ` 填完并保存。想取 `d = 2` 演算时，可以在动画中取一个实例；符号模型保留任意正容量。上下文向机器提供假设，Rodin 在证明机器性质时可使用这些假设。[新建项目和组件](https://stups.hhu-hosting.de/handbook/rodin/current/html/tut_project_setup.html)

### 3.2　建立机器与不变式

1. 对 `MyFirstBridge` 再建 **Event-B Component**，名称填 `M0_Capacity`，类型选 **Machine**。如果向导提供 **Sees Context**，选 `C0_Capacity`。也可在建好后通过机器下的 **Add Child → Sees Context** 或 Edit 页的 **SEES** 填入同名目标。
2. 在机器下添加一个 **Variable**，标识符 `n`。不要把 `n ∈ ℕ` 整行填在变量名里。接着添加 **Invariant**，第一条标签 `inv0_1`、谓词 `n ∈ ℕ`；第二条标签 `inv0_2`、谓词 `n ≤ d`。两行共同表达 `0 ≤ n ≤ d`。
3. 再添加一行 Invariant，标签 `thm0_dlf`、谓词 `n < d ∨ 0 < n`，将这一行的 **Theorem** 设为 true。前两行是要由事件保持的约束，这一行要求从已给条件推出一个结论。点击保存后，`d` 应已能解析。

### 3.3　填写三个事件

新机器通常已有 **INITIALISATION**。在这个事件内添加 Action，不要另建同名事件。然后在机器下加两个 **Event**，标签分别为 `ML_out`、`ML_in`，状态保持 **ordinary**。每个普通事件内各加一条 Guard 和一条 Action。课件中的 ML 指 mainland，`ML_out` 表示车离开大陆进入系统，所以计数增加。

| 事件 | Guard（Label 为 grd1） | Action（Label 为 act1） | 执行含义 |
|---|---|---|---|
| INITIALISATION | 留空 | `n ≔ 0` | 确定起点为空 |
| ML_out | `n < d` | `n ≔ n + 1` | 有空位时放进一辆车 |
| ML_in | `0 < n` | `n ≔ n − 1` | 系统内已有车辆时允许一辆返回大陆 |

逐项保存。守卫输入的是一个可判真假的谓词，动作输入的是赋值。选择结构树中的事件名右击 **Add Child** 可以分别找到 Guard 和 Action；正文的注释栏不会参与模型检查。公式编辑细节和两种编辑器示例见[官方建模教程](https://stups.hhu-hosting.de/handbook/rodin/current/html/tut_building_the_model.html)。

### 3.4　做一次纸上验收

取 `d = 2`。初始化后 `n = 0`，只有 `ML_out` 可执行；执行一次，得到 `n = 1`，此时两个事件均可执行；再执行一次 `ML_out`，得到 `n = 2`，只能 `ML_in`。对照工程时，守卫决定当前可走的边，动作决定走过去的状态。若 `n = 2` 时仍能进车，首先检查 `ML_out/grd1` 的不等号。

## 4　输入数学符号与参数

在公式输入框键入 ASCII，等待编辑器转换，或从 **Symbols** 点选符号。找不到视图时，到 **Window → Show View → Other…** 中搜索 **Rodin Keyboard**，在其中输入，复制转换后的结果。表格中的左右两列是键入形式和阅读结果，含有竖线的输入也请使用英文半角字符。[Rodin Keyboard 官方对照表](https://wiki.event-b.org/index.php/Rodin_Keyboard_User_Guide)

| 想表达的关系 | 输入 | 示例读法 |
|---|---|---|
| 自然数、正自然数、整数 | `NAT`、`NAT1`、`INT` | ℕ 包含零，ℕ1 从一开始，ℤ 包含负数 |
| 属于、包含于 | `:`、`<:` | `x ∈ S` 表示一个元素，`A ⊆ S` 表示一个子集 |
| 小于等于、不等于 | `<=`、`/=` | `n ≤ d` 和 `i ≠ j` |
| 且、或、蕴含 | `&`、`or`、`=>` | `a = 0 ∨ c = 0` 允许桥两边都为空 |
| 全称、存在、量词分隔 | `!`、`#`、`.` | `∀x·x ∈ A ⇒ x ∈ B` |
| 偏函数、全函数 | `+->`、`-->` | `f ∈ A → B` 对 A 中每项均有唯一结果 |
| 整数区间、乘法、减法 | `..`、`*`、`-` | `1‥n`、`2∗a+b`、`n−1` |
| 确定赋值、从集合中任选 | `:=`、`::` | `n ≔ n + 1`、`x :∈ S` |
| 映射对、关系覆盖 | <code>&#124;-&gt;</code>、`<+` | 覆盖时保留旧函数其余输入的像，只替换指定键 |
| 布尔集合和取值 | `BOOL`、`TRUE`、`FALSE` | `button ∈ BOOL`；守卫可以写 `button = TRUE` |

**参数的填法。** 在某事件下添加 **Parameter**（ANY）并命名 `x`，再加 **Guard** 写 `x ∈ A`。如需引用 `f(x)` 且 `f` 是偏函数，再写 `x ∈ dom(f)`。前者给参数定类型，后者保证函数应用有定义。一次事件选择一组使所有守卫为真的参数。动作右边均读取事件发生前的值，两条动作可以同时交换两个不同变量，同一个变量在一个事件中只赋值一次。

## 5　怎样定位并完成一项证明

### 5.1　从源元素找到目标

保存机器，等待自动构建。若未启用自动构建，选中工程后执行 **Project → Build Project**，或用 **Build All**。先解决 Problems 中阻止静态检查的错误。展开 **M0_Capacity → Proof Obligations**，筛选或展开到事件下的条目。双击一条 PO（proof obligation，证明义务）打开 **Proving** 透视图。简单目标可能被生成器直接化简，显示的数量还受版本、插件、自动证明配置影响。

例如名字 **ML_out/inv0_2/INV** 从左至右表示事件、不变式、义务类型。其数学问题是：假设旧状态符合 `n ∈ ℕ` 和 `n ≤ d`，又知道守卫 `n < d`，能否推出动作后的 `n + 1 ≤ d`？整数上相邻值至少差 1，因此答案是肯定的。另一个方向，`ML_in` 需要 `0 < n` 才能推出 `n − 1 ∈ ℕ`。这两条证明直接解释了为何守卫必须严格。

### 5.2　读证明树并处理未完成目标

1. 双击未完成项，先读 **Goal**，用一句中文重述它，再读 **Selected Hypotheses**。要找的条件可能保存在其他可用假设中。用 **Search Hypotheses** 搜变量名（如 `n`），找到相关不变式或守卫，加入选用假设。
2. 若 Goal 是合取，可按界面提供的合取分解规则拆成分支。若假设是析取，例如 `a = 0 ∨ c = 0`，可对该假设分情况证明，分别查看两个子目标。在 **Proof Control** 中应用界面可用的自动策略或理由器，观察 **Proof Tree** 是否还有开放叶子。
3. 对算术目标，选好变量类型、界和守卫再尝试自动策略。对函数目标，优先找定义域和函数类型。每步看 Goal 或 Selected Hypotheses 具体少了什么。走错分支时，在 Proof Tree 回到相应节点，用该节点提供的撤销或剪枝操作重新尝试。
4. 所有分支完成后保存证明。回到 Explorer 查看状态。**Reviewed** 表示人工审阅，**Discharged** 才表示该义务已证明。若一个合法状态能使目标为假，应修改模型，使证明对应真实需求。[Proving 视图与证明树](https://wiki.event-b.org/index.php/The_Proving_Perspective_%28Rodin_User_Manual%29)

### 5.3　故意造一次错

在个人练习工程把 `ML_out/grd1` 改成 `n ≤ d`，保存并构建。观察容量不变式的保持义务。取 `d = n = 2`，守卫为真，动作后 `n = 3`，目标 `3 ≤ 2` 为假。把守卫改回 `n < d`，保存再看状态。这次实验的错误类型是**语法正确而模型有反例**。与之相对，拼错 `d` 的名称会在静态检查阶段失败，二者需要到不同视图处理。

| PO 后缀 | 在检查什么 | 遇到该类目标先看哪里 |
|---|---|---|
| WD | 表达式在所用输入下有定义 | 偏函数定义域、除数、最值所在集合 |
| INV | 初始化建立、普通事件保持不变式 | 把动作右边代入不变式，寻找遗漏的守卫 |
| THM | 定理可以从之前可用的假设推出 | 定理放置顺序、需要的不变式、公理 |
| GRD | 具体事件使能时，抽象事件的守卫也成立 | 精化目标与连接不变式，消失的参数及其见证 |
| SIM、EQL | 具体动作模拟抽象动作，并保持需要保留的变量 | 抽象事件更新与具体事件的并行赋值 |
| FIS、WFIS | 非确定动作或见证存在可选择的值 | 给出一个具体值并验证约束 |
| NAT、VAR、FIN | 数值变式的非负性、变式的变化或集合变式的有限性 | 新事件是否严格减小变式，预期事件是否使它增加 |

Rodin 自动生成多类局部义务。若要证明非终止系统“总有一步可走”，通常要显式把非初始化事件的可使能条件析取起来作为定理；带参数事件要对参数作存在量化。本例的 `thm0_dlf` 就是最短的做法。搜索任务到达结束态后可以有意停止，它的进展条件应排除完成态。即使总有事件可走，某一指定请求也可能一直被推迟，响应性还需要讨论调度或公平性。[证明义务速查](https://stups.hhu-hosting.de/handbook/rodin/current/html/generated_proof_obligations.html)

## 6　在 Rodin 中做一次精化

1. 打开 `L2_Bridge/M0_Capacity` 与 `M1_Bridge` 并排阅读。若要亲手建立，回到个人工程，右击抽象机器，选择 **Refine**，新机器名填 `M1_Bridge`。也可新建 Machine 后在 **REFINES** 指定父机。**SEES** 指向本层使用的上下文。如果扩充常量或类型，右击旧上下文用 **Extend** 或新建 Context，在 **EXTENDS** 指向父上下文。
2. 对照第二讲讲义，设置变量 `a,b,c`。它们分别统计大陆往岛的桥上车辆、岛上车辆、岛往大陆的桥上车辆。本层用 `n = a + b + c` 连接前后层，用 `a = 0 ∨ c = 0` 限制桥上方向。替换掉的抽象变量 `n` 可在连接不变式中引用。实际维护的每个具体变量都需有初始化。
3. 双击具体事件，检查 **REFINES** 指向哪一个抽象事件。若勾选 **Extended**，同名或所指抽象事件的参数、守卫和动作会继承，在本层补新增内容。要改写抽象动作时，使用未扩展的显式精化事件，给足当前所需守卫和动作。若向导留下了操作旧变量的继承动作，应按设计重写。
4. 对本层新增且在上一层看不见的事件，抽象效果相当于 `skip`。如 `IL_in` 把一辆车从 `a` 转到 `b`，两项相加不变。需要证明其不会无限独占系统时，在机器的 **Variant** 写 `2∗a+b`，在有关事件的 **Convergence** 选 **convergent**。这一步车从 `a` 转到 `b` 后，变式减少 1。
5. 若具体事件消去抽象参数，在 **Witness** 下以消失的参数名作标签，谓词给出该参数与具体状态的关系。第一讲的序列查找把“任选命中位置”精化成“用当前索引作为命中位置”，是练习见证的实例。
6. 保存并构建。顺次检查 Problems、GRD/SIM、INV、VAR。把具体轨迹投影回抽象状态：对 `(a,b,c) = (1,0,0)` 执行上岛事件得到 `(0,1,0)`，两端都对应 `n = 1`。若连接式算不回去，就先检查事件归属、加减号和漏写的并行动作。[精化与建模记号](https://stups.hhu-hosting.de/handbook/rodin/current/html/modeling_notation.html)

## 7　用 ProB 观察事件和反例

1. 基础编辑、构建和证明使用 Rodin 自带功能。需要观察执行轨迹时，可在 **Help → Install New Software…** 的 **Work with** 中选择 ProB for Rodin 的稳定更新站点。若列表未预置，在 **Add…** 输入 ProB 官方教程给出的地址，选择匹配插件，完成向导后重启。插件安装取决于所用版本与操作系统。[ProB 官方安装与动画教程](https://prob.hhu.de/w/index.php?title=Tutorial_Rodin_First_Step)
2. 在 Explorer 右击 `M0_Capacity`，选 **Start Animation/Model Checking**（有的版本位于 ProB 子菜单）。如需设置常量，选择满足公理的一组值，尽量用小的正容量；若展示多个候选项，选 `d = 2`，也可按实际选值调整以下次数。执行 **INITIALISATION** 后，变量视图应显示 `n = 0`。
3. 双击可用的 `ML_out`，观察 `n` 增为 1，历史列表新增一步。执行到容量已满，检查 `ML_out` 是否从可执行列表消失。再执行 `ML_in`，观察它何时重新可用。可回退历史，探索同一状态的另一选择。
4. 模型检查时选中要检查的不变式违反或死锁，并记录常量、搜索上限和是否已遍历完毕。出现反例时，从初始状态沿事件历史逐步回放，记下最后正常状态、触发事件和第一个异常状态。有限实例的搜索结果适合找错；对任意容量和任意参数的结论，回到 Rodin 看符号证明。

## 8　保存、导出和重新导入

按 **Save All** 后，在 **File → Export… → General → Archive File** 勾选完整工程，检查包括 `.project`、机器 `.bum`、上下文 `.buc`。自己做过交互式证明且希望同学复用时，一并保留证明源 `.bpr`。静态检查结果 `.bcm/.bcc`、生成的证明义务 `.bpo`、状态缓存 `.bps` 可在目标工作区重新构建。工作区内部的 `.metadata` 带有个人路径和界面设置，无需打入课程工程。

交作业前可在 **File → Switch Workspace → Other…** 选一个新的空工作区，在导入向导改选 **Select archive file**，选择刚导出的工程 ZIP，核对工程列表、SEES/REFINES 关系及构建结果。本资料包也可解压后按第 2 节导入。若同时维护多个版本，给练习工程独立名称或分别使用工作区，避免同名项目互相遮挡。

## 9　按症状排查

| 症状 | 先查什么 | 如何恢复 |
|---|---|---|
| 导入列表为空或项目灰色 | 目标目录中是否有 `.project`；工作区是否已有同名项目 | 选到具体工程目录或使用新工作区，再点刷新 |
| 只有文件，找不到 Event-B 模型树 | 是否在通用文件视图；工程是否由 Event-B Project 向导创建 | 打开 Event-B Explorer；重新从含 `.project` 的完整目录导入 |
| Unknown identifier | 名称大小写、SEES、EXTENDS、REFINES 与参数作用域 | 从未解析的第一项追到它的声明；在相应父元素添加引用并保存 |
| Variable does not have a type | 是否只填变量名，漏掉类型不变式 | 加 `n ∈ ℕ` 或相应函数、集合类型；常量的类型放在公理中 |
| Variable is not initialized | INITIALISATION 是否为每个具体变量赋值 | 在该事件补 Action；精化时同时查看继承的初始化 |
| 键入显示方框或语法错 | 中文标点、公式栏位置、数学字体、符号是否已转换 | 用英文输入法与 Symbols 重输；字体问题参考官方发布说明 |
| 证明义务看不见或数量变化 | 构建是否结束、静态错误、只显示未完成项的过滤器 | 清除首条静态错误，保存，检查过滤器；需要时用 **Project → Clean…** 后重建 |
| 证明目标长期未完成 | 条件少了、定理实际为假，或自动策略没有找到推导 | 先代入边界状态找反例；再选假设、分情况并尝试证明 |
| 添加守卫后动画“卡住” | 各事件守卫是否同时为假；当前状态是否为设计的终止态 | 记录变量值，逐项算守卫。桥和压力机核对进展；查找核对是否成功结束 |
| 重开后找不到工程 | 启动时是否换了 Workspace | Switch Workspace 切回，或从备份目录重新导入 |

完成本手册后，打开第一讲银行模型，自己指出“静态假设”“当前状态”“一次操作”分别在哪个组件中。再进入桥和压力机，每读一层先写下一条可执行轨迹，再读一条能解释该轨迹为何安全的证明义务。
