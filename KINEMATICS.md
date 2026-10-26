# 五轴耦合运动学与 D0.1～D1 同心圆

[打开在线仿真](https://vforvan.github.io/galvo-validator/galvo-validator.html)

## 输入、输出及单位

五个执行器输入为

$$\mathbf u=(G_1,G_2,G_3,G_4,s)^T,$$

其中四个 $G_i$ 是镜面机械角（°），$s$ 是移动 L1 的音圈位移（mm）。第五轴是直线位移。工件侧输出为

$$\mathbf p=(x,y,z,\alpha,\beta)^T=F(\mathbf u).$$

$x,y$ 相对出光轴，$z$ 相对工作基准面，均为 mm；$\alpha,\beta$ 是方向投影角（°）。出射方向为

$$\mathbf d=\frac{(\tan\alpha,\tan\beta,-1)^T}{\sqrt{1+\tan^2\alpha+\tan^2\beta}}.$$

三角函数的自变量须先从度转换为弧度。总入射角大小为

$$A_{\rm abs}=\arctan\sqrt{\tan^2\alpha+\tan^2\beta}.$$

## 正向耦合模型 F

当前模型为 `all-real-lenses-clearance-515-v5-geometric-relay`。光路依次经过 L1、L2、G1～G4、R1、R2、固定折转镜和四片 f75 物镜。

振镜法线随机械角绕固定轴旋转：

$$\mathbf n_i=R(\mathbf a_i,G_i)\mathbf n_{i0}.$$

旋转采用 Rodrigues 公式：

$$R(\mathbf a,g)\mathbf v=\mathbf v\cos g+(\mathbf a\times\mathbf v)\sin g+\mathbf a(\mathbf a\cdot\mathbf v)(1-\cos g).$$

主光线与镜面平面的交点及反射方向为

$$t_i=\frac{(\mathbf c_i-\mathbf r_i)\cdot\mathbf n_i}{\mathbf d_i\cdot\mathbf n_i},\qquad
\mathbf r_i'=\mathbf r_i+t_i\mathbf d_i,$$

$$\mathbf d_i'=\mathbf d_i-2(\mathbf d_i\cdot\mathbf n_i)\mathbf n_i.$$

透镜表面按真实球面/平面求交，再按 Snell 定律折射。令单位法线指向入射介质，$c=-\mathbf n\cdot\mathbf d$，$\eta=n_{\rm in}/n_{\rm out}$，则

$$\mathbf d'=\eta\mathbf d+\left(\eta c-\sqrt{1-\eta^2(1-c^2)}\right)\mathbf n.$$

根号内为负、交点不在正向传播路径或超出有效孔径时，该追迹不成立。音圈移动 L1 的轴向位置，因而同时影响后续光线落点、方向及焦面；五个输入须联立处理。

Z 由 512 条等功率高斯采样光线的最小几何 RMS 焦面确定。在参考平面 $Z_0=-80$ mm 上，对每条光线的横向落点与斜率分别减去均值，记为 $\mathbf c_i$、$\mathbf k_i$，则

$$\Delta Z=-\frac{\sum_i\mathbf c_i\cdot\mathbf k_i}{\sum_i\mathbf k_i\cdot\mathbf k_i},\qquad Z_{\rm best}=Z_0+\Delta Z.$$

网页的 $x,y$ 取主光线在此焦面的交点；$z$ 为该焦面相对工作基准面的高度。方向投影角为 $\alpha=\arctan(d_x/(-d_z))$、$\beta=\arctan(d_y/(-d_z))$。以上逐面递推共同定义非线性函数 $F$。

## 位置逆解和局部速度运动学

给定目标 $\mathbf p^*$，求解

$$F(\mathbf u)-\mathbf p^*=\mathbf0.$$

定义完整的 5×5 耦合雅可比：

$$J_{ij}=\frac{\partial F_i}{\partial u_j},\qquad
J_{ij}\approx\frac{F_i(\mathbf u+h_j\mathbf e_j)-F_i(\mathbf u-h_j\mathbf e_j)}{2h_j}.$$

网页现有引擎的角度扰动为 $10^{-5}$°、位移扰动为 $10^{-5}$ mm。各行、列保留上述物理单位，数值求解时按量程归一化残差。迭代为

$$J(\mathbf u_k)\Delta\mathbf u=\mathbf p^*-F(\mathbf u_k),\qquad
\mathbf u_{k+1}=\mathbf u_k+\lambda\Delta\mathbf u.$$

$\lambda$ 通过缩步选择以降低误差。相邻圆周点可复用上一点的局部雅可比；每次均重新正算验证残差，未收敛时回退到现有完整逆解器。

局部速度及加速度关系为

$$\dot{\mathbf p}=J(\mathbf u)\dot{\mathbf u},\qquad
\ddot{\mathbf p}=J(\mathbf u)\ddot{\mathbf u}+\dot J(\mathbf u,\dot{\mathbf u})\dot{\mathbf u}.$$

当 $J$ 满秩时，可解线性方程得到对应的执行器速度。网页本轮实现的是位置轨迹与逆解；实际运行还需要给定进给速度，并结合电机限速、加速度、跃度、同步周期和标定结果规划时间律。

### 当前处方在中心的数值线性化

对目标 $(0,0,0,0,0)$ 求解得到 $\mathbf u_0\approx(0,0,0,0,0.04635714334)$，其中四镜单位为度，音圈单位为 mm。中心雅可比秩为 5。忽略约 $10^{-9}$ 量级的数值差分噪声后：

```text
δx = −1.26261267 δG2 − 1.26261266 δG4       [mm]
δy = −1.78560395 δG1 − 1.78560396 δG3       [mm]
δz = −1.06194108 δs                        [mm]
δα =  8.27120819 δG2 + 2.46465305 δG4       [°]
δβ = 12.87018853 δG1 + 8.56943153 δG3       [°]
```

$\delta\mathbf u=\mathbf u-\mathbf u_0$，$\delta\mathbf p=\mathbf p-\mathbf p_0$。这里呈现的分组结构来自中心对称位置；离轴、倾斜或改变焦面后必须重新计算完整 $J$。整组同心圆仍使用非线性逐面光线模型逆解，不能将中心线性式当作全行程精确方程。

## 同心圆与带符号 AOI

直径 $D_i=0.1i$ mm，$i=1,\ldots,10$；圆周方位角 $\theta_k=k$°，$k=0,\ldots,360$；带符号 AOI 为 $A_j=j$°，$j=-5,\ldots,5$。固定 $z=z_0$，目标为

$$x=\frac{D_i}{2}\cos\theta_k,\quad y=\frac{D_i}{2}\sin\theta_k,\quad z=z_0,$$

$$\alpha=\arctan(\tan A_j\cos\theta_k),\qquad
\beta=\arctan(\tan A_j\sin\theta_k).$$

正 AOI 的横向方向沿径向，负 AOI 沿反径向；实际入射角大小始终为 $|A_j|$。$\alpha,\beta$ 不能直接用 $A_j\cos\theta$、$A_j\sin\theta$ 替代，否则非零角度时总 AOI 会随方位变化。

执行顺序：AOI −5°下依次闭合 D0.1、D0.2、…、D1.0，随后 AOI 增加 1°并重复十圈，直到 +5°。共 110 圈、39,710 个圆周点，包含每圈首尾各一个重合位置。

每圈闭合点示意关光；下一圈起点作为关光转移终点单独标记。CSV 另含 109 条转移终点记录，总计 39,819 条数据。转移段的连续路径与时间律尚未生成。网页播放可抽帧；CSV 对每个 1°圆周点求解，包含目标、理想电机值、按当前量化设置应用的电机值、实际正算输出、残差和孔径/行程检查结果。

页面的正负 AOI、圆周步长和直径步长是三项独立参数。本轮固定为 AOI 1°、圆周 1°、直径 0.1 mm。验证报告中的整组数据使用 $z_0=0$；页面可锁定其他当前 Z 后重新导出，结果须按 CSV 的检查列判断。

## 代码与验证

- `concentric.js`：整数索引生成目标、相邻点延续逆解。
- `real_engine.js`：原有真实曲面正向模型、完整逆解器及雅可比。
- `concentric_ui.js`：逐圈切换、连续演示、全点 CSV 导出。
- `test_concentric.cjs`：整组逐点正算复核、行程与采样孔径检查，并在 Z=±1 mm 抽点与原逆解器比较。
- `test_concentric_browser.cjs`：播放、换圈、正负 AOI、进度定位、取消导出、原演示和移动端检查。

512 光线焦面、边界孔径及网页封装检查属于当前光学模型的数值检查；整组报告不等同于新增的 Zemax 原生逐点验证或实机运动验收。
