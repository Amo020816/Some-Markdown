
# Introduction
## What is Computer Graphics

计算机图形学涉猎的范畴：Animation, Simulation, Graphical Users Interface (GUI), Video Games, Movies, Virtual Reality, Typography. 

## Why study Computer Graphics

### Fundamental Intellectual Challenges

- 创造并与现实世界的交互
- 需要对物理世界各个方面的了解
-  新的计算方法、显示方法、技术

### Technical Challenges

- 矩阵的映射，曲线，曲面的数学知识
- 光线、着色的物理知识
- 在3D上对图形的表示和操作
- 动画和仿真

## Course Topics (Mainly 4 parts)

- Rasterization 光栅化
- Curves and Meshes
- Ray Tracing
- Animation / Simulation

### Rasterization 光珊化

- 什么是光珊化？ 
把三维空间的几何形体显示在屏幕上。

在游戏和实时型的计算机图形学应用上着重应用。
实时的定义：达到30FPS的叫实时，否则叫Offline。
### Curves and Meshes

- 如何表示一条光滑的曲线，如何表示曲面。
- 如何用简单的曲面，通过细分的方法得到更复杂的曲面。
- 当形状发生变化时这些面该如何变化。
- 如何保持住物体的拓扑结构。

### Ray Tracing
动画和电影中着重使用。(Offline Applications)

- Shoot rays from the camera through each pixel
	-  Calculate insertion and shading
	-  Continue to bounce the rays until they hit light sources.

### Animation / Simulation
- 关键帧动画
- Mass-spring system