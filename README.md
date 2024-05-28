# 선박생산공학기초및실습 과제1
---
## 퍼블리셔 노드
### import rclpy
from rclpy.node import Node
from rclpy.qos import QoSProfile
from std_msgs.msg import String

class JogwangjongPublisher(Node):

    def __init__(self):
        super().__init__('jogwangjong_publisher')
        qos_profile = QoSProfile(depth=10)
        self.publisher = self.create_publisher(String, 'jogwangjong', qos_profile)
        self.timer = self.create_timer(1, self.publish_jogwangjong_msg)
        self.count = 0

    def publish_jogwangjong_msg(self):
        msg = String()
        msg.data = 'jogwangjong: {0}'.format(self.count)
        self.publisher.publish(msg)
        self.get_logger().info('Published message: {0}'.format(msg.data))
        self.count += 1

def main(args=None):
    rclpy.init(args=args)
    node = JogwangjongPublisher()
    try:
        rclpy.spin(node)
    except KeyboardInterrupt:
        node.get_logger().info('Keyboard Interrupt (SIGINT)')
    finally:
        if rclpy.ok():
            node.destroy_node()
        rclpy.shutdown()

if __name__ == '__main__':
    main()

---

## 서브스크라이브 노드
### import rclpy
from rclpy.node import Node
from rclpy.qos import QoSProfile
from std_msgs.msg import String

class JogwangjongSubscriber(Node):

    def __init__(self):
        super().__init__('jogwangjong_subscriber')
        qos_profile = QoSProfile(depth=10)
        self.subscription = self.create_subscription(
            String,
            'jogwangjong',
            self.listener_callback,
            qos_profile)
        self.subscription  # prevent unused variable warning

    def listener_callback(self, msg):
        self.get_logger().info('Received message: {0}'.format(msg.data))

def main(args=None):
    rclpy.init(args=args)
    node = JogwangjongSubscriber()
    try:
        rclpy.spin(node)
    except KeyboardInterrupt:
        node.get_logger().info('Keyboard Interrupt (SIGINT)')
    finally:
        if rclpy.ok():
            node.destroy_node()
        rclpy.shutdown()

if __name__ == '__main__':
    main()
---


