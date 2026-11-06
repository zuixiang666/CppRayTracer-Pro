# CppRayTracer-Pro

这是一个基于 C++ 从零构建的进阶版光线追踪（Ray Tracing）渲染引擎。

本项目是在学习图形学基础理论后，基于经典开源教程 **ssloy/tinyraytracer** 的工程版本进行的深度二次开发。相较于早期的单文件基础版，本项目引入了现代 C++ 工程结构（CMake 构建），并使用第三方库（`stb_image`）实现了更加复杂的物理渲染效果。

## ✨ 核心技术特性

- [x] **现代工程化架构**：脱离单文件开发，使用 CMake 进行项目构建，引入 `stb_image` 第三方图像库读取真实环境贴图（JPG）。
- [x] **环境贴图（Environment Mapping）**：手写基于球面经纬度的三维射线到二维 UV 坐标的映射算法，将真实风景照片无缝包裹至场景外部，并修复了坐标系导致的图像倒置（V轴翻转）问题。
- [x] **光线折射（Refraction）**：引入斯涅尔定律（Snell's Law）计算光线的折射方向，并扩展了 `Vec4` 材质系统存储折射率，实现了逼真的玻璃/水滴材质。
- [x] **递归光线追踪（Recursive Ray Tracing）**：支持多重镜面反射与折射的递归计算，并设有最大递归深度（Depth）防止栈溢出。
- [x] **Phong 光照模型**：支持多光源渲染，实现了漫反射（Diffuse）与镜面高光（Specular）计算。
- [x] **硬阴影（Hard Shadows）**：利用阴影射线（Shadow Rays）进行遮挡检测，并通过“法线偏移（Shadow Offset）”技术解决阴影痤疮（Shadow Acne）伪影。

## 💡 开发踩坑与心得

在复现与二次开发的过程中，解决了以下几个关键问题：

1. **环境贴图颠倒 Bug**：由于 `stb_image` 的像素读取顺序（自上而下）与数学公式的映射顺序（自下而上）相冲突，导致天空和地面倒置。通过在计算 `v` 坐标时引入 `1.0f - v` 翻转修复。
2. **C++ 编译链配置**：深入理解了 CMake 构建系统的 `CMakeLists.txt` 配置，以及如何处理现代 CMake 对旧版 `cmake_minimum_required` 的兼容性问题。
3. **射线起点偏移（Shadow/Refraction Offset）**：在计算阴影和折射时，为避免浮点数精度导致射线与自身表面相交产生黑色噪点，在法线方向引入了极小量 `1e-3` 的偏移。

## 🙏 致谢

- 本项目核心图形学基础框架参考自经典开源教程：[ssloy/tinyraytracer](https://github.com/ssloy/tinyraytracer)。
- 感谢原作者在计算机图形学教育领域的杰出贡献。

## 🛠️ 构建与运行

本项目使用 CMake 管理构建，确保你已安装 C++ 编译器（如 MinGW/g++）以及 CMake。运行成功后，当前目录下会生成 out.jpg 或 out.ppm 图像文件。

```bash
# 1. 创建构建目录并进入
mkdir build
cd build

# 2. 生成构建文件 (Windows + MinGW 环境)
cmake -G "MinGW Makefiles" ..

# 3. 编译
cmake --build .

# 4. 运行程序
./tinyraytracer.exe


