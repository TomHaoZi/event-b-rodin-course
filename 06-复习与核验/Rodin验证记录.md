# Rodin 工程构建与证明记录

本次使用 Rodin 3.10 正式版、Java 21、macOS Apple Silicon。从工程的 `.project`、`.bum`、`.buc` 建立新工作区，逐项核对静态检查文件和证明义务。基础工程 ZIP 另行解压，在空工作区读入随包的 `.bpr` 并全量重建。下表给出 2026 年 11 月 18 日该版本的记录。

## 如何读这份记录

**构建结果。** 5 个正式工程的 35 个组件及反例工程的 6 个组件全部通过 Rodin 静态检查，所有静态组件的 accurate 标记为 true。6 个工程均无静态错误或警告。反例工程特意保留可以推翻的性质。

**已消解。** 逐一对照 Rodin 生成的义务名称和证明状态，只计入已消解的义务。核心版使用 Rodin 自带证明工具，进阶版还使用 Atelier B Provers。两份工程的模型与上下文相同，保存的证明树不同。空白工作区只导入源文件、忽略已附证明，或另选自动证明组合时，显示的数量可能不同。

**读到 0 项时。** 生成器可直接化简平凡目标。0 项表示这个组件没有需要另外证明的生成项，不能据此断言每条自然语言需求都已形式化。有限状态搜索和独立符号计算的数字另见各讲末尾。

## 正式工程的证明汇总

| 工程 | 生成义务 | 核心版已消解 | 进阶版已消解 |
|---|---:|---:|---:|
| L1_Bank | 32 | 26 | 32 |
| L1_Search | 16 | 5 | 16 |
| L2_Bridge | 246 | 174 | 246 |
| L3_Patterns | 125 | 118 | 125 |
| L3_Press | 67 | 64 | 67 |
| **合计** | **486** | **387** | **486** |

## 到机器中定位

表内按“已消解 / 已生成”计数。打开组件下的 Proof Obligations，取消只显示某一种状态的过滤器，可逐项比对。

| 工程 | 组件 | 核心版 | 进阶版 |
|---|---|---:|---:|
| L1_Bank | `c0.buc` | 0 / 0 | 0 / 0 |
| L1_Bank | `c1.buc` | 0 / 0 | 0 / 0 |
| L1_Bank | `c2.buc` | 0 / 0 | 0 / 0 |
| L1_Bank | `m0.bum` | 9 / 13 | 13 / 13 |
| L1_Bank | `m1.bum` | 16 / 18 | 18 / 18 |
| L1_Bank | `m2.bum` | 1 / 1 | 1 / 1 |
| L1_Search | `ctx_0.buc` | 0 / 1 | 1 / 1 |
| L1_Search | `m_0a.bum` | 2 / 3 | 3 / 3 |
| L1_Search | `m_1a.bum` | 3 / 12 | 12 / 12 |
| L2_Bridge | `C0_Capacity.buc` | 0 / 0 | 0 / 0 |
| L2_Bridge | `C2_Lights.buc` | 0 / 0 | 0 / 0 |
| L2_Bridge | `C3_Sensors.buc` | 4 / 4 | 4 / 4 |
| L2_Bridge | `M0_Capacity.bum` | 2 / 7 | 7 / 7 |
| L2_Bridge | `M1_Bridge.bum` | 12 / 27 | 27 / 27 |
| L2_Bridge | `M2_Lights.bum` | 45 / 53 | 53 / 53 |
| L2_Bridge | `M3_Sensors.bum` | 111 / 155 | 155 / 155 |
| L3_Patterns | `Pair0.bum` | 38 / 40 | 40 / 40 |
| L3_Patterns | `Pair1.bum` | 20 / 20 | 20 / 20 |
| L3_Patterns | `Pair2.bum` | 31 / 31 | 31 / 31 |
| L3_Patterns | `PatternCtx.buc` | 0 / 0 | 0 / 0 |
| L3_Patterns | `StrongCount.bum` | 10 / 10 | 10 / 10 |
| L3_Patterns | `StrongSignals.bum` | 0 / 0 | 0 / 0 |
| L3_Patterns | `WeakCount.bum` | 13 / 18 | 18 / 18 |
| L3_Patterns | `WeakSignals.bum` | 6 / 6 | 6 / 6 |
| L3_Press | `ClutchCtx.buc` | 0 / 0 | 0 / 0 |
| L3_Press | `DoorCtx.buc` | 0 / 0 | 0 / 0 |
| L3_Press | `MotorCtx.buc` | 0 / 0 | 0 / 0 |
| L3_Press | `Press0.bum` | 0 / 0 | 0 / 0 |
| L3_Press | `Press1.bum` | 0 / 0 | 0 / 0 |
| L3_Press | `Press2.bum` | 0 / 0 | 0 / 0 |
| L3_Press | `Press3.bum` | 19 / 20 | 20 / 20 |
| L3_Press | `Press4.bum` | 0 / 0 | 0 / 0 |
| L3_Press | `Press5.bum` | 38 / 40 | 40 / 40 |
| L3_Press | `Press6.bum` | 7 / 7 | 7 / 7 |
| L3_Press | `Press7.bum` | 0 / 0 | 0 / 0 |

## 进阶版剩余目标

本版本的正式工程中，Rodin 生成的全部证明义务都已消解。

## 反例工程单独计数

反例机器用于练习定位不可证目标。下表记录基础 Rodin 从反例源文件直接构建后的状态；其中有的开放目标有明确反例，有的只是自动策略尚未完成。各自的反例、重放步骤和对应修复见[反例实验说明](../05-反例实验/L2_Bridge_Lab/README.md)。

| 组件 | 生成义务 | 已消解 |
|---|---:|---:|
| Lab0_Unguarded.bum | 6 | 2 |
| Lab1_TwoGreens.bum | 46 | 23 |
| Lab2_LastSpace.bum | 35 | 20 |
| Lab3_SwitchLoop.bum | 52 | 27 |
| Lab_Capacity.buc | 0 | 0 |
| Lab_Colors.buc | 0 | 0 |

## 复核时从哪一步开始

1. 在 Rodin 选择一个新工作区。用 **File → Import… → General → Existing Projects into Workspace** 导入工程包，等自动构建结束。
2. 先展开 L2_Bridge / M0_Capacity 的 Proof Obligations。读 ML_out/inv0_2/INV，把动作代入 n ≤ d。比较不同证明配置对同一个整数不等式所给的证明树。
3. 转到 M1_Bridge 或 M3_Sensors 的无死锁定理，查看各分支使用哪些守卫和不变式。若自动策略不能完成，结合第二讲的车数分类与传感器状态做分情况推理。
4. 实际修改模型时另开练习副本。一次修改可能使已有证明需重新检查，重新构建后以新的 Proof Obligations 状态为准。

Rodin 的验证对象是随包模型及其公理。课件截图中的证明数量、有限实例的状态总数与本表各有不同含义。例如 Press7 的一个具体实例可有 2,816 个可达状态，而它在当前精化层的生成义务数量可以是零。
