# 五轴光路交互仿真与振镜分辨率预算

## 网页版本

- [V1.0 在线仿真](https://vforvan.github.io/galvo-validator/galvo-validator.html)。
- [V2 深孔三维统一仿真](https://vforvan.github.io/galvo-validator/v2/)：30×30 µm、深300 µm方孔，7 µm候选、分层填充、五轴逆解、角度×Z一致性。
当前候选仅完成网页光学检查，本轮未运行Zemax；加工能力仍待工艺验证。

## 同心圆整组扫描

- 直径 D0.1～D1 mm，间隔 0.1 mm；圆周每隔 1°取点并闭合。
- AOI 从 −5°到 +5°，每隔 1°；每个 AOI 连续演示 10 圈，共 110 圈。
- 页面支持整组播放、进度定位和逐点五轴 CSV 导出，保留原单圈与孔加工示例。
- [五轴耦合运动学与同心圆方程](KINEMATICS.md)：四振镜机械角＋音圈位移映射到 x、y、z、α、β。
- [Z=0 整组数值验证报告](concentric-verification.json)：39,710 个圆周点逐点复核。

网页提供当前光学模型的位置逆解。换圈连接运动、时间律及实际电机动态约束尚未规划。
