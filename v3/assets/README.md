# V3：G1/G2错位三镜片布局，独立测试通过

2026-10-30按用户授权修改本地布局。G1向入口移动20 mm，G2向入口移动10 mm、向−Y移动7.627608158 mm，扩大后续光束到G1的空间。三片透镜和四面振镜数量、透镜处方及位置保持，不增加镜片。

**当前工作工程：[V3_G1_clearance.zos](V3_G1_clearance.zos)，21配置、默认5。状态：独立测试通过。** DeepSeek运行20261030-140246-11eda5完成六阶段检查；用户重存后的ZOS另经21配置只读核对，光学结构和真实光线不变。

## 布局

| 振镜 | 当前镜心/mm | 外形u×v×厚/mm | 净口径u×v/mm |
|---|---|---|---|
| G1 | (160,92,49.4) | 30×25×2 | 28×23 |
| G2 | (170,34.372391841644,49.4) | 58×40×2 | 56×38 |
| G3 | (260,192.03,49.4) | 64×40×2 | 62×38 |
| G4 | (336.08,192.03,49.4) | 58×46×2 | 56×38 |

已同步G1/G2/G3零位法向、G2数学转轴、G1/G2电机与轴段占位、21配置五轴指令、原生工程与网页模型。G1/G3电机上置，G2在外侧，G4在+Y侧；机体仍按Ø38×75 mm、轴段按Ø10 mm预留。

固定入口为(0,92,49.4)沿+X，G3→G4标称沿+X，最终由G4向−Z出光，水平像面基准Z=−80 mm。主箱体仍止于X=300，G4及其电机在箱体外。名义入口到G3的分段几何长度保持400.026471174344 mm。

输入515 nm、原高斯1/e²直径2 mm，几何EPD 3 mm。工作域Ø2.5 mm、Z±1 mm、合成AOI≤7.5°，音圈机械范围±3.5 mm。原三片透镜仍为L1/L2和单片双面非球面F75，材料、曲率、非球面、厚度及净口径保持。

## 工程和数据

- [design.json](design.json)、[prescription.json](prescription.json)：当前工作模型及三片透镜处方。
- [cases.json](cases.json)、[configurations.csv](configurations.csv)：新布局21配置五轴指令。
- [reference_rays.json](reference_rays.json)：依据新模型生成的独立光线参考，21配置原生复核通过。
- [release.json](release.json)、[forward_publish_audit.json](forward_publish_audit.json)：当前文件来源与哈希。后者记录独立测试和重存文件核对；另见[saved_native_audit.json](saved_native_audit.json)。
- [本地网页](index.html)，或打开仓库`output/galvo-validator-v3-round3.html`。页面显示“G1净空改进布局 · 独立测试通过”，原生配置选择器显示本版21配置POP记录。
- [布局构建脚本](g1_clearance_revision/prepare.py)、[本轮结构说明](g1_clearance_revision/README.md)。

上一已接受纵向布局的数据、ZOS、旧POP及文档原样归档于[forward_reference](forward_reference/README.md)，来源哈希见其`archive_manifest.json`；原`V3_assembly_forward.zos`继续保留。更早Round3水平基线与孔道工程在[round3_reference](round3_reference/README.md)。V2保持独立，未改动其处方、工程或电机指令。

## 测试交接

本轮完整测试已由DeepSeek执行并通过，六阶段原始运行目录为`tests/results/20261030-140246-11eda5/`。测试按[交接文档](validation/deepseek_summary.md)，在仓库根目录执行：

```powershell
python tests/run.py --force
```

六阶段为版本一致性、532目标几何/机械、构建、网页引擎、浏览器和21配置原生光线/POP。报告、截图、测试副本及日志保存在根目录`tests/results/<运行编号>/`；总结使用[模板](validation/deepseek_summary.md)。失败时保留原始错误，不降门槛、不跳过或自行改布局。

专项门槛：G2→G3光束到G1旋转基片净空至少5 mm；电机/轴段保守光线净空至少3 mm。扩大轮廓应通过真实净口径，532目标保持可达。相同目标新POP的D4/D95相对归档已接受结果增加各不超过1%，窗口边缘功率比例不超过1e-4。

## 状态边界

独立实测G1旁最小采样净空13.678667 mm；电机/轴段最小胶囊净空3.087625 mm；G3长边通光余量1.158407 mm。真实镜架、法兰、机壳、线缆、公差、热漂移和连续扫掠仍未验收；需特别关注G3长边通光余量与G2电机到端壁空间。

网页实时高斯包络假设M²=1.2，不是实时POP。新布局原生POP已按DeepSeek的21配置实测记录更新，等效D4范围15.786737～17.500102 µm；旧布局M²=1相干POP仍单独存于根目录`tests/data/v3/pop_regression_reference.json`作回归比较。新孔道POP没有重做，不能继承任何旧结构孔道结论。

已根据本轮报告更新验收状态与页面POP。当前下载文件的SHA-256为`163d476c6d9facd75a6d99f1edf7ae24feb3856509287bfaa8271943068c752f`；与实测文件的3字节保存差异及21配置只读核对记录见`saved_native_audit.json`，未重复计算POP。
