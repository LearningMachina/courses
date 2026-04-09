# System Integration

## Concept

Individual subsystems — motors, sensors, vision — become a robot only when they communicate with each other reliably. ROS2 (Robot Operating System 2) provides the messaging backbone: nodes publish data to topics, other nodes subscribe and react. This decoupling lets you develop and test each piece independently.

## Topics

- ROS2 basics: nodes, topics, messages
- Writing a complete sense–plan–act loop
- Logging, debugging, and testing on hardware

## Code

```python
# ros2_drive_node.py — a minimal ROS2 Python node
import rclpy
from rclpy.node import Node
from geometry_msgs.msg import Twist
from sensor_msgs.msg import Range

class DriveNode(Node):
    def __init__(self):
        super().__init__("drive_node")
        self.publisher = self.create_publisher(Twist, "cmd_vel", 10)
        self.sub = self.create_subscription(
            Range, "ultrasonic/front", self.on_range, 10
        )
        self.get_logger().info("DriveNode started")

    def on_range(self, msg: Range):
        cmd = Twist()
        if msg.range < 0.3:          # obstacle closer than 30 cm
            cmd.linear.x  = 0.0
            cmd.angular.z = 0.5      # turn in place
            self.get_logger().warn(f"Obstacle at {msg.range:.2f} m — turning")
        else:
            cmd.linear.x = 0.3       # drive forward
        self.publisher.publish(cmd)

def main():
    rclpy.init()
    node = DriveNode()
    rclpy.spin(node)
    node.destroy_node()
    rclpy.shutdown()
```

```bash
# Run in separate terminals
ros2 run your_package drive_node
ros2 topic echo /cmd_vel            # inspect messages
ros2 topic hz  /ultrasonic/front    # check sensor rate
ros2 run rqt_graph rqt_graph        # visualise node connections
```

```
Sense → Plan → Act loop
─────────────────────────────────────────────────
Sensor node  publishes /ultrasonic/front (Range)
Drive node   subscribes, decides velocity
Drive node   publishes /cmd_vel (Twist)
Motor node   subscribes, sets PWM on hardware
```

## Action

1. Install ROS2 Humble on the Jetson (if not already present).
2. Create a ROS2 Python package and implement the `DriveNode` above.
3. Write a fake sensor node that publishes a `Range` message decreasing from 1.0 m to 0.1 m, to test obstacle avoidance without hardware.
4. Use `ros2 topic echo` to verify messages flow correctly.
5. Draw the node graph with `rqt_graph`.

## Reflection

After observing the robot's behavior, reflect on:

- What is a ROS2 topic, and how is it different from a direct function call between two pieces of code?
- Why is the sense–plan–act separation useful, even for a simple obstacle-avoidance robot?
- What would you check first if a node is publishing but the subscriber is not receiving messages?

## Extension

Modify the ROS2 system to change the robot's integrated behavior:

1. Change the obstacle threshold from 30 cm to 50 cm — the robot becomes more cautious. Try 15 cm — it becomes bold. Observe the personality shift.
2. Add a new ROS2 node that publishes a "mood" topic based on how often obstacles are encountered — the robot's internal state becomes observable.
3. Create a node that logs all sensor readings to a CSV file — the robot now has memory. Replay the data later and you can see exactly what the robot experienced.
