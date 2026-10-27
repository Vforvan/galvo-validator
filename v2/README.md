# V2 无 R1/R2：四镜联合布置与光斑圆度候选

分支：`optics-v2-structure`。本目录是可复查的 V2 研究候选；V1.0 工程和标签作为对照保存。

当前音圈机械上限按用户新确认的±3.5 mm、总行程7 mm处理，见 [motion_limits.json](motion_limits.json)。本目录保留原光学处方及其历史验证记录；重新优化的Ø0.1 mm × 深1 mm候选见 [独立深孔工程](deep-hole/README.md) 和 [在线模型](https://vforvan.github.io/galvo-validator/v2/deep-hole/)。机械音圈行程不等同于工件焦点Z行程。

打开 [原生工程](5AXIS_no_relay_v2.zos) · [交互点列对比与光路](review.html) · [配置表](configurations.csv)。

## 具体修改

- 取消 R1/R2 四个折射面；光路为 L1→L2→G1→G2→G3→G4→MF→四片定制球面 f75 物镜。
- 四块镜片统一研究尺寸 **58×40×2 mm**，有效矩形口径 **56×38 mm**；镜间距和电机安装侧联合调整。
- G1/G2/G3/G4 中心依次为 **(240,92,49.4)、(240,142,49.4)、(184,142,49.4)、(184,192.03,49.4) mm**。G1→G2 的折转方向相对 V1 改为 +Y；旋转轴与五轴标定同步修改。
- L1/L2 为重新设计的球面研究处方；L1 基准 X25 mm，L2 X80.000000 mm，毛坯直径仍为12.7/25.4 mm。不能按原目录镜片直接采购。
- 物镜8个真实球面及内部间距重新优化，EFL74.820240 mm（近f75），首/末面 Z18.003445/−29.996555 mm，总长48.000000 mm和名义后焦距50.003445 mm保持。具体处方见 [design.json](design.json)。
- 出光轴 (336.08,192.03)、工作基准 Z−80 mm、整机估算外包络 **381.08×310×345 mm** 保持；全部新玻璃实体、镜面扫动包络和电机示意体放入既有壳体预留。

## 点列图与主旋切范围

主验收工况：Z=0，D0.1～D1 mm，AOI−5°～+5°，圆周1°取点、逐圈闭合。**39,710/39,710** 个点通过，光斑圆度取二维协方差主轴长短轴比，不以 X/Y 截面替代。

整体二阶矩接近圆，并不能保证每个入瞳采样环都接近圆形。本轮进一步优化了逐环各向异性和环质心偏移，同尺度点列的主要采样环已比上一候选更接近圆形；仍保留部分小尺度环的方向性残差，尚未宣布全域逐环近圆验收通过。上述通过记录指逆解、采样通光及相应统计，不能混用其含义。

| 指标 | 当前 V2 |
|---|---:|
| 512光线点列长短轴比最大值 | 1.049965 |
| 同心圆几何 RMS 半径最大值，512光线 | 0.262231 µm |
| 同心圆机械角绝对最大值 | 3.084188° |
| 同心圆采样有效口径余量最小值 | 3.562609 mm |
| 各入瞳环径向极差的全轨迹最大值 | 0.212280 µm |

所有环均计入逐环统计，包括半径接近零、长短轴比数值敏感的环。原始逐环长短轴比最大值 23.1257；对RMS至少0.1 µm的环分组后为 7.0176。这个分组只作诊断，不将被分组的小环视作自动通过；径向绝对差保留全部环。完整轨迹CSV含逐环指标，交互图可直接检查形态。

曲面优化同时约束圆度和RMS。为改善极小点列残差的相对各向异性，前级使用约0.25 µm的平衡旋转对称几何残差目标；这是实际处方的像差权衡，未通过显示拉伸或移动观察面修饰形状。最后按真实固定工作面复算，并用POP检验能量光斑。

交互图统一采用2048条等功率高斯入瞳光线、质心参考、相同坐标尺度；与原生抽查逐条一致。它是独立追迹生成的对比图，不是 Zemax 窗口截图；原生六边形密度200的统计数值可因采样方式而略有差异。

原生Standard Spot另以相同Gaussian2.25入瞳、六边形密度200、质心参考、离焦0复算：

| 工况 | V1原生RMS半径，µm | V2原生RMS半径，µm |
|---|---:|---:|
| center3 | 0.873535926 | 0.312248244 |
| circle5 | 0.850625154 | 0.301810986 |

原始文本见 [native_spot_comparison.json](native_spot_comparison.json) 中的文件名；可加载 [V2_spot.CFG](V2_spot.CFG) 复查原生窗口。原生标准点列与独立采样图分别记录输入/采样条件，不跨口径等同数字。

## 同条件 POP 能量光斑

同一515 nm相干基模高斯输入，束腰半径1 mm、输入功率归一1 W、工件法线投影。全部尺寸/能量由完整未裁剪网格积分；仓库ROI只用于查看，完整2048²/4096²网格保留在本地并可由脚本重算。该POP没有自动实现实际激光器M²=1.2。

| 工况 | V1 D4σ主轴/µm | V2 D4σ主轴/µm | D95 V1 / V2，µm | 椭圆率 V1 / V2 |
|---|---|---|---|---|
| center3 | 8.887×8.928 | 7.189×7.223 | 10.494 / 8.665 | 1.0046 / 1.0047 |
| edge3 | 8.843×8.900 | 7.185×7.237 | 10.463 / 8.695 | 1.0064 / 1.0072 |
| circle5_x | 8.802×8.921 | 7.156×7.230 | 10.481 / 8.673 | 1.0135 / 1.0103 |
| circle5_y | 8.802×8.921 | 7.155×7.230 | 10.481 / 8.673 | 1.0135 / 1.0104 |
| circle5_diag | 8.800×8.922 | 7.177×7.218 | 10.510 / 8.679 | 1.0138 / 1.0057 |
| center_zminus1 | 7.090×7.090 | 6.663×6.663 | 8.434 / 8.088 | 1.0000 / 1.0000 |

5°圆周X向工况另做4096²采样复核；详见 [完整指标](pop_comparison/metrics.json)。几何点列RMS、D4σ、D95与衍射Strehl采用各自定义。

## 完整范围回归与当前限制

原405点目标域及新增18点，共423点通过逆解和采样通光检查；独立2048光线最差几何RMS半径 **0.796320 µm**，V1同集合为 **1.143418 µm**。最大机械角 **5.297331°**，小于现有±5.5°建模假设；音圈范围 -1.052733～1.239626 mm，小于±2.5 mm。

**主旋切域已改善，但尚不能宣布整个原始域的全部像质指标均不劣于V1。** 在半径1.25 mm、Z±1 mm和AOI7.5°的扩展域，V2最大点列长短轴比 1.1829，V1为 1.0989；部分焦位分离和近轴辅助束腰指标仍需改进。交互图保留较差工况供直接查看，未将其从报告中移除。六个POP代表点也不能替代全域衍射最差值搜索。

原生工程含38面、6片真实玻璃、12个折射面，无Paraxial理想面。28/28个代表配置逐条验证主光线、512加权光线和32边界光线、保存重载；V1源文件SHA-256保持不变。详见 [native_verification.json](native_verification.json)。

## 封装与电机预留

四镜连续±5.5°扫动实体的镜间分离下界最小约 5.761 mm；镜片与示意电机实体分离下界最小约 4.400 mm。0个采样光束/电机或轴杆交叉事件；全部提出的硬件包络仍位于原壳体预留内。

58×40 mm是共同光学毛坯方案，尚不是厂家已确认的振镜产品。较大的镜片惯量、G2轴杆长度、带载速度/噪声、实际镜架与装调公差需要供应商CAD和驱动规格核验。尺寸保持结论适用于本目录列出的实体与预留模型。见 [mechanical_verification.json](mechanical_verification.json)。

## 在 Zemax 查看

配置1为中心直射。配置含义按本目录配置表使用，不能套用V1的30/86编号。

| V2配置 | x,y,z,α,β（mm/°） |
|---|---|
| 1 | 0, 0, 0, 0, 0 |
| 2 | 0, 0, -1, 0, 0 |
| 3 | 0, 0, 1, 0, 0 |
| 4 | 0, 0, 0, -7.5, 9.238e-16 |
| 5 | 0, 0, 0, 3, 0 |
| 6 | 0, 0, 0, 7.5, 0 |
| 7 | 0, 0, 0, -1.386e-15, -7.5 |
| 8 | 0, 0, 0, 4.619e-16, 7.5 |
| 9 | 0.5, 0, 0, -5, -0 |
| 10 | 0.5, 0, 0, 5, 0 |

点列图采用当前配置、波长1、视场1、Gaussian2.25入瞳、质心参考、离焦0，比较时保持图幅一致。POP使用本目录保存的分析设置；输入面1，像面37，相干基模高斯。完整原生文件：[5AXIS_no_relay_v2.zos](5AXIS_no_relay_v2.zos)。

## 工程源码与复现

[对应工程源码](https://github.com/Vforvan/5AIX-GALVO/tree/optics-v2-structure/simulation/no_relay_v2/)。以下命令在工程仓库中执行。

在本目录执行 `cmd /d /c build_fast.cmd`，使用配置的VS2022 MSVC构建C++20真实曲面追迹DLL。Python仅调用此计算库；无Qt界面依赖。随后在仓库根目录依次执行：

```text
python simulation/no_relay_v2/validate.py
python simulation/no_relay_v2/native_validate.py
python simulation/no_relay_v2/mechanical.py
python simulation/no_relay_v2/validate.py --circles
python simulation/no_relay_v2/compare_pop.py
python simulation/no_relay_v2/make_review.py
```

C++加速器与NumPy独立模型有24个状态交叉检查，几何焦面采用512等功率高斯光线；原生DLL另逐条核验。`optimize.py`、`optimize_objective.py`、`layout_search.py`为研究优化入口，运行会生成新候选并使旧校验失效；请在重新校验后再生成报告。同心圆CSV只含闭合圆周点，换圈连接段和时间律另行规划。

在线入口：[V1五轴仿真](https://vforvan.github.io/galvo-validator/galvo-validator.html) · [V2三维五轴仿真](https://vforvan.github.io/galvo-validator/v2/) · [V2点列与POP对比](https://vforvan.github.io/galvo-validator/v2/review.html)。网页发布仓库另有同名 `optics-v2-structure` 分支；GitHub Pages由main发布独立v2目录，V1入口保留。

V2 主页面支持三维旋转缩放、四振镜及音圈直调、目标五轴逆解、圆形扫描、D0.1～D1 整组连续演示和五轴 CSV 导出。三维几何与计算使用本目录的已验证处方，未沿用 V1 镜位。网页的近轴束腰、几何点列 RMS、对比页 POP 各自标注，不能混作实测光斑。

重新构建网页，在仓库根目录执行：

```text
node analysis/optics_demo/build_v2.cjs
node analysis/optics_demo/test_v2_engine.cjs
node analysis/optics_demo/test_v2_concentric.cjs
python simulation/no_relay_v2/export_review_site.py tmp/galvo-validator-web-v2
node simulation/no_relay_v2/test_review_site.cjs
```

独立网页输出 `output/galvo-validator-v2.html`；发布前测试默认使用本地 8733 端口，也可将站点根 URL 作为最后一条命令的参数。浏览器测试需要 Playwright 与 Edge。网页引擎的423组姿态和7组独立Python交叉核对结果见 `analysis/optics_demo/v2-engine-verification.json`；全39,710点网页复算见 `analysis/optics_demo/v2-concentric-verification.json`。
