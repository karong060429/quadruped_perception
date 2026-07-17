# quadruped_perception
# 四组机器人感知层开发环境目录
quadruped_perception/               #  GitHub 仓库根目录 (也是 ROS2 Package 目录)
├── README.md                       # 仓库说明文档：写明依赖项和运行指令
├── docs/                           # 文档目录
│   └── perception_interface.md     # 🌟 第一步生成的 D与E 接口契约文档
├── quadruped_perception/           # Python 源码存放区 (必须与包名同名)
│   ├── __init__.py                 
│   ├── mock_perception_node.py     # 第二步要写的假数据节点
│   └── color_detector_node.py      # 未来要写的视觉节点
├── config/                         # 参数配置目录
│   └── perception_params.yaml      # 存放相机的内参、颜色阈值等魔法数字
├── package.xml                     # ROS2 依赖配置文件
├── setup.py                        # Python 节点编译配置
└── setup.cfg
