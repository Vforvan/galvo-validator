# 五轴光路网页

> 说明：全部 Zemax 原生工程文件（`.zos` / `.zmx` / `.ZAR` / `.ZDA`）仅保存在个人主机，本公开站点已下架并移除全部 Zemax 下载入口与工程文件。

## V4 · 目录透镜与扫描镜光路（当前最新版，2026-11-18）

- [当前 V4 在线交互网页](https://vforvan.github.io/galvo-validator/v4/)：三片标准目录透镜（EKSMA 114-1118E + OptoSigma SLSQ-25-120P 反置 + EKSMA 117-5075）配合 G2/G3 等光程布局与 Wavelength SCM 扫描镜。
- 核心复核记录：21 配置 256² Strehl **0.9814–1.0000**；1024² POP D4σ **16.42–24.52 µm**（轴比 ≤1.0633）；Ø100 µm 深孔 13 工况孔底功率 **99.34%–100.00%**；PMAC Prog 30 在线逆解实采重建 3D RMS **0.133 µm**；FPGA Q18 241 项 / 242 周期。
- 网页附带非工程参数文档：[V4 说明](v4/assets/README.md)、[V3/V4 镜片选型对照](v4/assets/V3_V4_LENS_COMPARISON.md)、[配置表](v4/assets/configurations.csv)、[版本元数据](v4/source.json)。

## 历史版本参考

- [V3 · G1 净空改进三镜片](https://vforvan.github.io/galvo-validator/v3/)：[独立测试总结](v3/assets/validation/deepseek_summary.md)、[版本说明](v3/assets/README.md)。
- [V2 · Ø100 µm 深孔六镜片对照](https://vforvan.github.io/galvo-validator/v2/)：[配置说明](v2/round100/README.md)、[版本范围](v2/source.json)。
- [V1 · 冻结参考](https://vforvan.github.io/galvo-validator/galvo-validator.html)：原整机光路与同心圆扫描演示。

完整源码与工程记录位于私有仓库 [5AIX-GALVO](https://github.com/Vforvan/5AIX-GALVO)。网页和已有代表工况用于静态光学与控制评估，不替代实际硬件装调与工艺加工验收。
