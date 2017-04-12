# AE 效果
 Lens Flare -> 镜头炫光
 CC Light Wipe -> 边缘发光的擦除效果
 CC Glass Wipe -> 类似于水面溶解般的擦除
 Displacement Map -> 进行像素的位移和变换
 CC Lens -> 镜头效果（鱼眼，在屏幕四周的一个放大效果
 Scatter -> 分散为粒子 -> 制作毛玻璃


# 通过查找并自己尝试的可能制作玻璃的效果：
 > 在调节层上使用 transform 对下层像素进行放大、移位
 > 加上 fast blur

 > 使用 Displacement Map ？

# 尝试得出的比较好的效果
 > 每个玻璃分两层做，第一层为玻璃主体，应用 `transform` 效果和 `fast blur` 效果
 > 第二层应用 `3d glasses - Balanced Red Green` 进行调节，颜色混合模式选择 `Add`，之后通过 `curves` 调节色彩恢复原色调（或者应用蒙版使其只在玻璃边缘有效果

 Threshold -> 起点
