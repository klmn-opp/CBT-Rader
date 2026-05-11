# CBT-Rader

## 基本步骤

关于配置 Livox SDK2、ROS 2 与 Fast-LIO 等前置依赖的步骤，详见：[前置步骤](https://ecnmh7533t3z.feishu.cn/wiki/YWXewBB0piWYG9kSJs0cFNNtnKg)

### 1. 启动程序

打开 Rader 文件夹：

```bash
cd /home/rader
```

在该目录下打开终端，并执行以下命令：

```bash
bash rader.bash
```

### 2. 检查启动结果

执行指令后，应弹出以下窗口：

- `Livox Driver` 终端窗口
- `Fast-LIO` 终端窗口
- `Rviz` 窗口

在 `Rviz` 窗口中，应能够看到清晰的点云图。

---

## 注意事项

### 1. 电源要求

雷达功率为 **6.5W**，工作电压为 **9~27V**。

> **不允许接超过 27V 的电源。**

供电线接充电宝 **OUT1 输出口**，即蓝色 Type-C 口。

### 2. 光学表面清洁

激光雷达的半球为光学表面，请务必保证其表面洁净。

请注意：

- **不要**用手直接触摸光学表面。
- **不要**使用擦镜纸、纳米布以外的布料清洁光学表面。
- 如果发现表面落灰，请使用**气吹**进行清洁。

---

## 脚本内容

```bash
#!/bin/bash

# --- 使用说明 ---
# 1. 这个脚本用于在 Ubuntu 系统中启动两个 ROS2 节点：Livox 驱动和 Fast-LIO。
# 2. 脚本依赖 'gnome-terminal'。
# 3. 在运行此脚本之前，请务必根据您电脑上的实际路径，修改下方标记为 “用户需要修改的部分” 的 ROS 工作空间路径。

echo "正在启动 Livox 驱动终端..."

# 启动一个新的终端来运行 Livox 驱动节点
gnome-terminal --title="Livox Driver" -- /bin/bash -c " \
    echo 'ROS 节点: 正在启动 Livox 驱动...'; \
    
    # --- 需要修改的部分 ---
    # 请将下面的路径 '/home/c/ws_livox' 更改为您自己电脑上 Livox 工作空间的绝对路径。
    # 例如: source /home/YOUR_USERNAME/YOUR_LIVOX_WORKSPACE/install/setup.bash
    source /home/c/ws_livox/install/setup.bash; \
    
    # 启动 Livox 驱动
    ros2 launch livox_ros_driver2 msg_MID360_launch.py; \
    
    # 保持终端打开，以便查看日志和输出
    exec bash"

# 短暂等待，确保上一个终端已成功启动
sleep 1 

echo "正在启动 Fast-LIO 终端..."

# 启动一个新的终端来运行 Fast-LIO 节点
gnome-terminal --title="Fast-LIO" -- /bin/bash -c " \
    echo 'ROS 节点: 正在启动 Fast-LIO...'; \
    
    # --- 需要修改的部分 ---
    # 请将下面的路径 '/home/c/ws_Fast_LIO' 更改为您自己电脑上 Fast-LIO 工作空间的绝对路径。
    # 例如: source /home/YOUR_USERNAME/YOUR_FAST_LIO_WORKSPACE/install/setup.bash
    source /home/c/ws_Fast_LIO/install/setup.bash; \
    
    # 启动 Fast-LIO 建图，并指定MID360的配置文件
    # 请确保 'mid360.yaml' 文件位于 fast_lio 包的正确配置路径下。
    ros2 launch fast_lio mapping.launch.py config_file:=mid360.yaml; \
    
    # 保持终端打开
    exec bash"

echo "所有终端已启动。"
```
