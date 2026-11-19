# 在 Ubuntu 启动和回放

按 [README](README.md) 编译软件包。每个相关终端加载同一虚拟环境和工作空间。

```bash
cd ~/ros2_ws
source /opt/ros/humble/setup.bash
source .venv/bin/activate
source install/setup.bash
export ROS_DOMAIN_ID=0
mkdir -p work/ultralytics work/temp
export YOLO_CONFIG_DIR="$PWD/work/ultralytics"
export TMPDIR="$PWD/work/temp"
ros2 launch surgical_perception perception.launch.py model_size:=m device:=cuda:0
```

也可以用 `model_size:=s` 或 `model_size:=l` 切换权重，用 `device:=cpu` 运行 CPU 推理。

## 回放自采图像

在另一个加载相同环境的终端执行以下命令。回放期间保持相机节点停止，保证输入来自同一个发布者。

```bash
python src/surgical_perception/test/ros_replay.py --image /绝对路径/自采测试图.png
```

脚本检查实例消息、类别、检测框、原图尺寸掩膜、叠加图和输入时间戳。连接实体内窥镜时，在另一终端启动 `ros2 launch endoscope_device endoscope.launch.py`，并给两个节点配置一致的 ROS domain 和通信环境。
