# V2 深孔统一网页 · 30 µm 方孔候选

[在线仿真](https://vforvan.github.io/galvo-validator/v2/)将完整三维光路、微孔剖切、分层填充、五轴逆解及角度×Z一致性合并到同一入口。旧 `/v2/deep-hole/` 自动进入该页；历史光路、原生文件与POP报告保留，并明确标记为上一版处方。

本轮只运行仓库的实曲面光线和高斯束计算，不启动Zemax、不改写任何 `.zos`。新候选不能使用旧POP结果作为其验证依据。

## 当前目标与修改

- 参考 precSYS 手册陶瓷实例：30×30 µm、深300 µm方孔，深宽比10:1、圆角R<4 µm、4×4阵列与10 µm壁厚。网页目前求解单孔的光路与轨迹；阵列的定位和逐孔工艺尚未建模。
- 新候选名义1/e²束腰直径约7 µm，入口直径2 mm、515 nm、M²=1.2均作为模型输入。它不是实测光斑。
- 只改变L1/L2球面曲率和L2轴向位置。四片物镜、四振镜统一58×40×2 mm外形、各中心、出光轴和381.08×310×345 mm整机外包络保持。
- 音圈机械限位±3.5 mm。3080组采样覆盖Z±0.15 mm、AOI−5°到+5°每1°、方位每45°及中心/四角附近。具体数值见 `verification.json`，15组另用1152条NumPy光线复核。
- 默认13焦层填充/轮廓轨迹在AOI=0°、5°分别逐点核对2925点，共5850点；位置逆解、镜面余量和行程检查通过，音圈约−0.203～+0.205 mm。见 `path_verification.json`，该结果不代表孔内送光或材料加工验收。
- 7 µm光斑的名义共焦长度约0.125 mm，短于300 µm孔深。默认25 µm层距，通过五轴逆解逐层重新对焦；不会把共焦长度当成整孔深度或音圈行程。

## 网页的计算含义

1. 光线逐面求球面交点、Snell折射和平面反射。五轴目标为XYZ与投影方向角αβ；Z采用512条等功率高斯采样光线的最小几何RMS焦面。
2. 光斑采用“束腰高斯核与目标平面实曲面点列卷积”的近似，离焦仅由光线传播计入一次。显示强度对每张图使用同一1 W归一化绝对色标、±20 µm视窗；D4σ由协方差计算。该非相干卷积不等同相干PSF或POP。
3. 蛇形填充与闭合轮廓使用名义半光斑内偏作为参考。半光斑与目标圆角的比较仅用于光学余量筛查，不代表烧蚀轮廓已标定。光斑变小不能单独证明R<4 µm或实际打穿。
4. 覆盖图假设恒定线速度和线能量，按路径长度积分，关光换行与换层不累计能量。它显示相对剂量，未设材料去除阈值，不能将色块边缘当成成孔边界。
5. 孔内通光采用预存在的圆角方形空孔和高斯外接圆轮廓，统计入口至焦面的保守余量。负余量表示发生遮挡风险；此时自由传播光斑不代表经过截断后的孔底能量。
6. 点击“一致性”计算7个焦层×11个AOI，每点重新对焦，可点击格子应用到三维。更改孔形参数或光路会清除旧矩阵。CSV逐点求五轴，含开/关光标记；没有加入硬件速度、加速度和控制器时序。

## 复现

```powershell
python simulation/deep_hole_v2/web_square/optimize.py
python simulation/deep_hole_v2/web_square/validate.py
node analysis/optics_demo/build_square.cjs
node analysis/optics_demo/test_square.cjs
python simulation/deep_hole_v2/web_square/export_site.py tmp/galvo-validator-web-v2
```

优化与验证不调用ZOS API。处方保存在本目录 `design.json`，与上一版 `../design.json` 独立。当前相空间/近轴假设、真实光束M²、相干衍射、装调误差及材料工艺须在后续阶段复核。

来源：[SCANLAB precSYS 02/2026 手册](https://www.scanlab.de/sites/default/files/2026-03/SCANLAB-precSYS-EN.pdf)，[Newport 高斯束尺寸与Rayleigh长度](https://www.newport.com/n/gaussian-beam-optics)。
