# 五轴光路网页

## V3 · G1净空改进三镜片（2026-10-30）

[当前V3](https://vforvan.github.io/galvo-validator/v3/)采用G1/G2错位布局，保留三片透镜及四振镜；G1旁采样最小净空13.679 mm。21配置、默认5，532目标几何/机械与21配置原生光线和POP经独立测试通过。只保留[最新版main](v3/assets/V3_main.zos)和[hole工程](v3/assets/V3_hole_POP.zos)。hole已从当前main延伸，保留6个孔道工况，前端结构/光线及孔道位置核对通过；[hole结构核对](v3/assets/hole_structure_verification.json)、[独立测试总结](v3/assets/validation/deepseek_summary.md)、[版本说明](v3/assets/README.md)。孔道POP尚未重新计算。V1/V2资源保留。

- [保留V2对照](https://vforvan.github.io/galvo-validator/v2/)：Ø100 µm、深1000 µm圆孔的五轴光路、旋入路径、固定工件及孔道通光检查。目标控件工作域Ø2.5 mm，音圈机械行程±3.5 mm。
- [冻结V1](https://vforvan.github.io/galvo-validator/galvo-validator.html)：原整机光路与同心圆扫描演示。

## 保留V2资源

- [主顺序工程](v2/round100/V2_main.zos)与[配置说明](v2/round100/README.md)。
- [非序列孔道工程](v2/round100/nsc/V2_hole_check.zos)与[6个工况说明](v2/round100/nsc/README.md)。
- [主工程配置Excel](v2/round100/V2配置点位与像面.xlsx)。
- [非序列位姿Excel](v2/round100/nsc/V2非序列6配置位姿.xlsx)。
- [precSYS基础参数对照](v2/round100/comparewithprecsys.xlsx)。
- [发布版本与验证范围](v2/source.json)。

旧版V2方孔页面、V2a演示及过期发布目录已移除。完整建模、构建和数值检查代码位于[工程仓库](https://github.com/Vforvan/5AIX-GALVO/tree/optics-v2-structure)。网页按当前工程的`simulation/v2/export_site.py`生成。

网页和已有NSC代表工况用于静态光学评估，不代表全工作域、连续动态或材料加工验收。
