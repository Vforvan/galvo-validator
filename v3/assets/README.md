# V3：Round3 三镜片水平像面基线

2026-10-30将用户指定的 `V2_round3_3lens_horizontal.zos` 独立保存为V3。本轮是版本冻结和网页同步，没有重新优化光学参数，也没有采用已弃用的一致性方案。后续由主代理直接完成，不再分派给子代理。

| 版本 | 光学结构 | 网页 |
|---|---|---|
| V3，当前基线 | L1/L2＋单片双面非球面F75，共3片透镜；4面振镜，G4兼最终向下折转；水平像面 | [V3网页](https://vforvan.github.io/galvo-validator/v3/) |
| V2，保留对照 | L1/L2＋四片F75物镜，共6片透镜；4面振镜＋独立折转镜 | [V2网页](https://vforvan.github.io/galvo-validator/v2/) |

## 工程

- [V3_main.zos](V3_main.zos)：21配置、默认5、45个顺序面，全部水平像面。曲率、Conic、A4/A6、镜间距和电机配置继承Round3；保存时更新版本标题。
- [V3_hole_POP.zos](V3_hole_POP.zos)：Round3处方派生的六配置孔道POP工程，默认1；孔径100 µm、最大深度1 mm，沿孔轴21个虚拟圆光阑近似吸收孔壁。光阑不是物理镜片。
- [configurations.csv](configurations.csv)：21配置的目标和五轴电机指令。
- [prescription.json](prescription.json)：冻结的18个处方参数及实际表面定义；[design.json](design.json)为网页使用的独立V3数据。
- [release.json](release.json)：来源和哈希、配置数量、重新打开后的水平像面检查。

主工程分析设置：[点列图](V3_spot.CFG)、[POP](V3_pop.CFG)、[PSF](V3_psf.CFG)。孔道工程使用 [V3_hole_POP_settings.CFG](V3_hole_POP_settings.CFG)。加载相应工程后，从分析窗口读取对应CFG，切换配置后刷新。

输入515 nm、原高斯1/e²直径2 mm、几何EPD 3 mm；F75标称有效焦距75 mm，中心厚度13.194127 mm。净口径直径L1/L2/F75分别为10.17/22.86/46 mm。工作域Ø2.5 mm、Z±1 mm、合成AOI≤7.5°；音圈机械范围±3.5 mm。

## G3/G4电机安装示意修正

2026-10-30发现旧电机安装表仍继承V2固定坐标，已按V3镜心及真实转轴重定位：G3电机中心为(184,192.03,14.5) mm，在新镜心下方、轴端朝+Z；G4电机中心为(336.08,226.53,49.4) mm，在镜面+Y一侧、轴端朝−Y。轴段按实际镜片外缘连接，电机机体到本镜片边缘的轴向间隙分别为4.4和4 mm。

这是电机占位和轴段显示修正，未改V3光学表面、镜心、数学转轴或五轴指令；G1/G2安装记录保留原状。当前Ø16×21 mm电机外形仍为示意，不代表完整机壳、连接件或供应商CAD已验收。核对记录见`motor_mount_verification.json`。

## 网页与物理光学的边界

V3网页对真实球面、Conic及A4/A6曲面求交并按Snell定律折射，追迹四面振镜，不保留独立固定折转镜。焦点Z采用512条等功率几何光线在水平面上的最小RMS位置；与源工程相同。

网页实时光束轮廓及孔道包络使用近轴高斯估计，假设M²=1.2，**不是实时POP**。原生配置选择器另显示既有Round3 POP记录（M²=1、总功率1 W），只对应原姿态；手动改动姿态会清除该历史结果显示。

历史全域POP汇总、孔道光学验证仍属于既有Round3处方结果，不因改名V3成为新实验。本轮新增检查是网页真实曲面追迹与独立Python参考光线的对照、21个原生配置目标/电机对应、101点下钻路径，以及V2回归检查，见 [网页核对记录](web_verification.json)。网页不判断材料加工或硬件动态。

## 构建

在仓库根目录设置`GALVO_PYTHON`后执行：

```powershell
$env:GALVO_PYTHON='C:/Users/27699/AppData/Local/Programs/Python/Python312/python.exe'
npm run build:v3 --prefix analysis/optics_demo
npm run test:v3 --prefix analysis/optics_demo
```

离线页面为 `output/galvo-validator-v3-round3.html`；也可直接打开此目录的`index.html`。发布脚本`export_site.py`只新增/更新网页仓库的`v3/`资源和版本导航，不删除V2。原始版本保存脚本`promote_round3.py`用于追溯，正常构建不重复运行它。
