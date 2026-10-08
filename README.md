# RobotDog Localization Stack

该仓库是机器狗 ROS 2 定位系统的**源码锁定仓库**，用于将两个独立组件固定为一套可复现的
定位链路：

```text
3D-BBS -> ICP handoff -> FAST-LOCALIZATION-ROS2
```

## 组件与职责

| 路径 | 组件 | 职责 |
| --- | --- | --- |
| `components/3d_bbs` | 3D-BBS 与 ICP handoff | 全局粗搜索、ICP 精配准、向 FAST 发布精配准候选位姿 |
| `components/fast_localization` | FAST-LOCALIZATION-ROS2 | 候选时间一致性与两次确认、全局状态注入、连续 LiDAR-IMU 定位、状态与诊断输出 |

本仓库不复制组件源码；两个目录都是 Git submodule。根仓库提交中的 Gitlink 是组件版本的
**唯一真相源**。不要以分支名代替版本号，也不要在其他 YAML 或文档重复维护组件 commit。

查看当前固定版本：

```bash
git submodule status
```

## 克隆

```bash
git clone https://github.com/WeJCh/RobotDog-Localization-Stack.git
cd RobotDog-Localization-Stack

git submodule update --init \
  components/fast_localization \
  components/3d_bbs
```

不要使用 `--recursive`：3D-BBS 内部的 `thirdparty/Eigen` 不属于本套系统的源码依赖；已验证
环境使用系统 `libeigen3-dev`。

网络受限且确认 Git 支持浅子模块时，可以将最后一条替换为：

```bash
git submodule update --init --depth 1 \
  components/fast_localization \
  components/3d_bbs
```

如果浅克隆无法取得 Stack 固定的 Gitlink 提交，应改回不带 `--depth` 的标准命令，而不是切换
到其他组件版本。

## 不进入 Git 的内容

以下内容必须作为外部部署/实验数据管理，不能提交到 Stack：

- FAST-LIVO2 地图目录、BBS target PCD 与 ICP global-map PCD；
- ROS 2 bag、RTK 原始记录、PCD、CSV、节点日志和评估产物；
- ROS 2 `build/`、`install/`、`log/` 目录；
- 服务器绝对路径、密钥、账号和代理配置。

## 下一步：环境与地图清单

`manifests/` 只记录外部数据和部署环境的**版本、来源、哈希与适用范围**，不保存数据本体。
建立新部署环境或切换地图时，应补齐：

- Ubuntu、ROS 2、GPU 驱动、CUDA、PCL、Eigen、OpenMP 的实际版本；
- 地图标识、FAST `map_dir`、BBS target 目录和 ICP PCD 的 SHA-256；
- RTK `imu_front` 杆臂与地图对齐 JSON 的适用地图版本；
- 真实机器狗传感器外参、话题和 QoS 的确认记录。

在这些清单完成前，本仓库只能保证源码组合可复现，不能证明任何服务器或真机环境可复现。
