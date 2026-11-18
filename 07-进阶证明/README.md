# 用 Atelier B Provers 查看更多已完成证明

第一次上课可直接导入普通工程。准备逐项查看证明时，可以在另一个工作区导入本目录中的进阶工程 ZIP。它包含同样的 5 套正式模型和上下文，并附有 Rodin 证明树。有些树调用 Atelier B Provers，需要安装这个插件才能重放其中的规则。

1. 启动 Rodin 3.10，选择 **Help → Install New Software…**。在 **Work with** 下拉框选择 **Atelier B Provers**。若列表中没有该项，用 **Add…** 添加官方[更新站点](https://www.atelierb.eu/update_site/atelierb_provers/)。
2. 选择对应功能项，完成安装向导并重启。可在 **About Rodin Platform → Installation Details** 查看已装的插件。本次核验使用 Atelier B Provers 2.4.1。
3. 选 **File → Switch Workspace → Other…**，使用新的空目录。进阶版与普通版采用相同工程名，分开工作区可以并排保留两套练习记录。
4. 选择 **File → Import… → General → Existing Projects into Workspace → Select archive file**，选本目录的 `Rodin-Projects-AtelierB.zip`，勾选工程后完成导入。
5. 导入本包后，五套正式工程合计 486 项义务均有已完成证明。打开 **L2_Bridge → M0_Capacity → Proof Obligations**，该机的 7 项义务均有已完成证明。双击 `ML_out/inv0_2/INV`，从 Proof Tree 查看旧状态、进桥守卫和动作后的上界如何联系。机器与上下文的逐项结果见[构建与证明记录](../06-复习与核验/Rodin验证记录.md)。

ZIP 包内保存的是课程模型与证明。插件由 Rodin 的更新向导从其发布方安装。
