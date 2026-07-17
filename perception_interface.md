本文件定义了感知与导航模块（E）与任务规划行为树模块（D）之间的 ROS2 通信标准。双方必须严格遵循此结构，禁止私自更改字段或数据类型。

1. 任务规划下发接口 (D -> E)
行为树向下发出的导航移动指令。
Topic 名称: /task_planner/nav_goal
消息类型: geometry_msgs/msg/PoseStamped
说明: 包含机器人需要前往的绝对或相对坐标 (x, y, yaw)。

2. 结构化感知上报接口 (E -> D)
感知模块（E）识别到目标后，向行为树（D）发布的结构化环境信息。
Topic 名称: /perception/target_info
消息类型: std_msgs/msg/String (内含 JSON 格式字符串)
JSON 数据结构规范:
{
  "target_id": "aruco_01",       
  "relative_pos": [1.2, -0.5, 0.0], 
  "confidence": 0.95,            
  "timestamp": 1689242300.123,
  "event_type": "checkpoint"     
}
字段解析:
target_id (String): 识别到的具体目标标识（如 aruco_01, red_rescue_sign, obstacle_box）。
relative_pos (Float Array): 目标相对于机器人（或相机）的 [X, Y, Z] 距离，单位为米。
confidence (Float): 识别的置信度 (0.0 ~ 1.0)，建议 D 模块在行为树中过滤掉小于 0.8 的结果。
timestamp (Float): ROS2 系统时间戳。
event_type (String): 场景分类枚举，包含 checkpoint (巡检点), anomaly (异常), obstacle (障碍物)。

3. 导航状态反馈接口 (E -> D)
导航模块（E）实时向行为树（D）汇报当前的移动状态，用于触发行为树的节点跳转或兜底（Fallback）机制。
Topic 名称: /navigation/status
消息类型: std_msgs/msg/Int8
状态码枚举字典:
0 : NAV_IDLE (空闲状态，等待指令)
1 : NAV_RUNNING (正在前往目标路点)
2 : NAV_REACHED (已到达目标点，定位误差已满足 <= 0.5m)
3 : NAV_BLOCKED (路径受阻或遭遇强制避障，请求行为树重规划)
