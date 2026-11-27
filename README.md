# 五轴光路网页

> 说明：全部 Zemax 原生工程文件（`.zos` / `.zmx` / `.ZAR` / `.ZDA`）仅保存在个人主机，本公开站点已下架并移除全部 Zemax 下载入口与工程文件。

## V5.3 · 一体全包覆五轴深孔扫描头（当前最新版，2026-11-27）

- [当前 V5.3 在线交互网页](https://vforvan.github.io/galvo-validator/v5/)：129 部件全包覆单箱体总装（主箱体 **328 × 294 × 188 mm**、总包络 **362 × 326 × 221.9 mm**，零外露电机尾座）、单一底板基准 `6×M4` 圆柱抱箍夹紧、`Extra-Galvo-14-X` + 曲柄连杆 + HIWIN `MGN9` 导轨调焦模组（五轴全数字旋转振镜），默认开启整机外形尺寸标注并支持一键 1:1 叠图对比 SCANLAB `precSYS 515`。
- 核心复核记录：L1 `SLSQ-15-50N`、L2 `SLSQ-25-90P` 与定制 `F60` 非球面物镜，四镜总光程 **163 mm**；24 配置 256² Strehl **≥ 0.99232**；1024² `M²=1.2` POP 最大长轴 **13.809 µm**（椭圆度 ≤ 1.0252）；`Ø100 µm × 1 mm` 深孔 13 工况孔底功率 **94.37%–100.00%**（`≤ 2.5°` 全 ≥ 97.49%）；600 次 CNC 公差蒙特卡洛补偿后 P90 Strehl **0.9843**；路线 A Q18 逆解 **285 项 / 286 拍（1.43 µs @ 200 MHz）**，`0 LSB` 偏置。

## 历史版本参考

- [V5.2 · 五轴深孔扫描头（音圈归档版）](https://vforvan.github.io/galvo-validator/v5.2/)：106 部件音圈调焦占位归档版（2026-11-27）。
- [V5.1 · 三层正交五轴扫描头（M1 制造版）](https://vforvan.github.io/galvo-validator/v5.1/)：117 部件五件分解箱体与 `G3` 分体夹座演示（2026-11-26）。
- [V4 · 目录透镜与扫描镜光路](https://vforvan.github.io/galvo-validator/v4/)：[V4 说明](v4/assets/README.md)、[V3/V4 镜片选型对照](v4/assets/V3_V4_LENS_COMPARISON.md)、[配置表](v4/assets/configurations.csv)、[版本元数据](v4/source.json)。
- [V3 · G1 净空改进三镜片](https://vforvan.github.io/galvo-validator/v3/)：[独立测试总结](v3/assets/validation/deepseek_summary.md)、[版本说明](v3/assets/README.md)。
- [V2 · Ø100 µm 深孔六镜片对照](https://vforvan.github.io/galvo-validator/v2/)：[配置说明](v2/round100/README.md)、[版本范围](v2/source.json)。
- [V1 · 冻结参考](https://vforvan.github.io/galvo-validator/galvo-validator.html)：原整机光路与同心圆扫描演示。

完整源码与工程记录位于私有仓库 [5AIX-GALVO](https://github.com/Vforvan/5AIX-GALVO)。网页和已有代表工况用于静态光学与控制评估，不替代实际硬件装调与工艺加工验收。
