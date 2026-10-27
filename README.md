# 五轴光路交互仿真与振镜分辨率预算

## 网页版本

- [V1.0 在线仿真](https://vforvan.github.io/galvo-validator/galvo-validator.html)：现有五轴仿真与同心圆示例。
- [V2 三维在线仿真](https://vforvan.github.io/galvo-validator/v2/)：无 R1/R2、四镜联合布置、五轴调节、同心圆连续演示。
- [V2 点列与 POP 对比](https://vforvan.github.io/galvo-validator/v2/review.html)：同尺度像质与原生验证记录。
- [V2 深孔光学候选](https://vforvan.github.io/galvo-validator/v2/deep-hole/)：音圈±3.5 mm、Ø0.1 mm × 深1 mm的独立处方与孔内通道。
- [V2 网页分支](https://github.com/Vforvan/galvo-validator/tree/optics-v2-structure)。

V2为研究候选；部分入瞳环和原始范围边界仍待优化，详见页面中的验证范围。

## 同心圆整组扫描

- 直径 D0.1～D1 mm，间隔 0.1 mm；圆周每隔 1°取点并闭合。
- AOI 从 −5°到 +5°，每隔 1°；每个 AOI 连续演示 10 圈，共 110 圈。
- 页面支持整组播放、进度定位和逐点五轴 CSV 导出，保留原单圈与孔加工示例。
- [五轴耦合运动学与同心圆方程](KINEMATICS.md)：四振镜机械角＋音圈位移映射到 x、y、z、α、β。
- [Z=0 整组数值验证报告](concentric-verification.json)：39,710 个圆周点逐点复核。

网页提供当前光学模型的位置逆解。换圈连接运动、时间律及实际电机动态约束尚未规划。
