# 外部环境与地图清单

本目录将保存可审计的文本清单，不保存任何地图、bag、PCD、CSV 或运行日志。

每次正式部署至少记录：

1. Ubuntu、ROS 2、GPU 驱动、CUDA、PCL、Eigen、OpenMP 的精确版本；
2. 使用的 `map_dir`、BBS `target_clouds` 目录、ICP `map_pcd` 的来源和 SHA-256；
3. RTK 杆臂、`imu_front` 对齐 JSON 与地图坐标系的适用范围；
4. 验证日期、验证 bag（仅标识与外部存储位置）和结论。

组件源码版本不在这里重复记录，以 Stack 根提交中的 submodule Gitlink 为准。
