# V3：纵向装配三镜片水平像面

2026-10-30按用户“用这版更新网页V3”，把已通过21配置原生复核的纵向装配候选`assembly_forward_trial/V3_assembly_forward.zos`发布为V3当前基线。进出光关系沿用V1/V2：入口和末端下折前都沿+X，G4兼最终向下折转，像面水平。三片透镜仍沿用Round3处方，曲率、Conic、A4/A6和厚度未重新优化；本轮改的是振镜位置、镜片外形和电机安装方向。原水平入口基线`V3_main.zos`与孔道工程已归档到[round3_reference](round3_reference/README.md)，不再作为当前工程。

| 版本 | 光学结构 | 网页 |
|---|---|---|
| V3，当前基线 | L1/L2＋单片双面非球面F75，共3片透镜；4面振镜，G4兼最终向下折转；水平像面 | [V3网页](https://vforvan.github.io/galvo-validator/v3/) |
| V2，保留对照 | L1/L2＋四片F75物镜，共6片透镜；4面振镜＋独立折转镜 | [V2网页](https://vforvan.github.io/galvo-validator/v2/) |

## 工程

- [V3_assembly_forward.zos](V3_assembly_forward.zos)：当前主工程，21配置、默认5、45个顺序面，全部水平像面。源候选保存后曾被再次保存；发布前重新打开核对21配置的曲率、材料、Conic/A4/A6、净口径和逐面光线，均与计算记录一致，再从只读核对生成默认配置5的下载副本。记录见[forward_publish_audit.json](forward_publish_audit.json)。
- [V3_main.zos](round3_reference/V3_main.zos)：历史基线，Round3三镜片水平入口布局，21配置、默认5；不是当前网页布局。
- [V3_hole_POP.zos](round3_reference/V3_hole_POP.zos)：历史孔道POP工程，6配置、默认1；**没有**随纵向布局重做，不能用来宣称新布局已完成孔道验证。
- [configurations.csv](configurations.csv)：21配置的目标和五轴电机指令。
- [prescription.json](prescription.json)：Round3处方参数、实际表面行和本轮镜片外形；[design.json](design.json)为网页使用的独立V3数据；[release.json](release.json)记录来源、哈希和工程数量。
- 原生分析设置：[点列图](V3_spot.CFG)、[POP](V3_pop.CFG)，均由新布局候选导出。本轮未重跑PSF，旧版[V3_psf.CFG](round3_reference/V3_psf.CFG)只作历史设置。

输入515 nm、原高斯1/e²直径2 mm、几何EPD 3 mm；F75标称有效焦距75 mm，中心厚度13.194127 mm。净口径直径L1/L2/F75分别为10.17/22.86/46 mm。工作域Ø2.5 mm、Z±1 mm、合成AOI≤7.5°；音圈机械范围±3.5 mm。

## 接口与内部布局

固定接口：输入基准点(0,92,49.4) mm、沿+X；G3→G4仍沿+X；出光轴线X=336.08、Y=192.03 mm；标称出光−Z，像面基准Z=−80 mm。没有改成顶部竖直进光。

| 振镜 | 镜心/mm | 数学正转轴 | 电机安装侧 | 外形u×v×厚/mm | 光学净口径/mm |
|---|---|---|---|---|---:|
| G1 | (180,92,49.4) | (0,0,1) | 上方 | 30×25×2 | 28×23 |
| G2 | (180,42,49.4) | (0.970153,−0.242495,0) | +轴，朝外侧 | 58×40×2 | 56×38 |
| G3 | (260,192.03,49.4) | (0,0,1) | 上方 | 64×40×2 | 62×38 |
| G4 | (336.08,192.03,49.4) | (0,1,0) | +Y侧 | 58×46×2 | 56×38 |

G1的58 mm长边会挡住G2→G3光束，因此按真实足迹缩小为30×25 mm；G3需要更长的镜面，预留64×40 mm，这是本轮的负载代价，后续需连同转子惯量选型评估。G4机械v向增至46 mm，但光学净窗口仍为56×38 mm，与原生工程净口径一致。

电机按Ø38×75 mm机体、Ø10 mm轴段预留。G1/G3电机在上方，G2在外侧，G4在+Y侧；本版修正后的转轴与镜心对齐，安装中心分别为G3 (260,192.03,124.9) mm、G4 (336.08,270.53,49.4) mm。机体到本镜片边缘的轴向间隙为15.5/18/18/18 mm。镜架、法兰、轴承接口、线缆、机壳壁厚和连续扫掠体尚未完成真实CAD验收。

## 核对范围

- 532个几何目标覆盖Ø2.5 mm、Z±1 mm、合成AOI≤7.5°，重新求解后全部可达，最大归一化残差2.36×10⁻¹⁰。四振镜最大绝对指令1.413°/1.227°/1.972°/1.649°，音圈最大3.301 mm，仍在±3.5 mm内。
- 每个目标追迹输入中心和半径1.5/2.5 mm轮廓。示意机体/轴段胶囊包络到采样光线的最小间隙3.112 mm；采样光线与非预期镜片盒相交0次。该检查不是连续扫掠或真实CAD碰撞检查。
- 保存并重新打开工程后，21配置原生光线与独立Python/C++求交一致，最大位置差小于4×10⁻¹² mm；曲面、材料、净口径和像面朝向逐项复核。
- 21配置原生POP（515 nm、M²=1、1 W、1024²）：等效D4σ直径15.787～17.500 µm，原布局为15.758～17.677 µm；同目标最大增加0.2632%，D95最大增加0.3171%。配置1/16/18/19的2048²核对采样差小于0.009%。POP只覆盖21配置，不是532点全域物理光斑验收。
- 网页检查：21配置光线与独立参考一致，101点旋入路径通过，浏览器实际打开后完成三维渲染、配置9加载、五轴输入应用、下载哈希和移动端布局检查，见[web_verification.json](web_verification.json)、[browser_verification.json](browser_verification.json)。浏览截图保存在仓库`output/v3_forward_20261030/`，不随网页发布。

## 网页与物理光学的边界

V3网页对真实球面、Conic及A4/A6曲面求交并按Snell定律折射，追迹四面振镜，不保留独立固定折转镜。焦点Z采用512条等功率几何光线在水平面上的最小RMS位置，与源工程一致。

网页实时光束轮廓及孔道包络使用近轴高斯估计，假设M²=1.2，**不是实时POP**。原生配置选择器另显示本布局既有的21配置POP记录（M²=1、总功率1 W），只对应所选配置姿态；手动改动姿态会清除该历史结果显示。新版孔道POP尚未重做，网页的Ø100 µm孔检查仍是近轴高斯包络示例。

历史全域POP汇总和旧孔道验证属于Round3原水平布局，不因改名V3而成为新布局结论。网页不判断材料烧蚀、排屑、制造公差或硬件动态。

## 构建与发布

在仓库根目录执行：

```powershell
$env:GALVO_PYTHON='C:/Users/27699/AppData/Local/Programs/Python/Python312/python.exe'
npm run build:v3 --prefix analysis/optics_demo
npm run test:v3 --prefix analysis/optics_demo
npm run test:v3:browser --prefix analysis/optics_demo
```

浏览器检查需要Playwright和本机Edge，路径与`test_concentric_browser.cjs`相同；不启动Zemax。离线页面为`output/galvo-validator-v3-round3.html`（文件名沿用历史路径，内容已是纵向装配布局），也可直接打开本目录的`index.html`。

发布脚本：

```powershell
python simulation/v3/export_site.py <网页仓库目录>
```

脚本会重建网页仓库的`v3/assets/`，保留归档，只新增/更新V3入口和版本导航，不删除V2。原始候选保存脚本`promote_round3.py`和`promote_forward_web.py`用于追溯，正常构建不重复运行。
