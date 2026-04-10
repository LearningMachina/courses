# Intégration système

## Concept

Les sous-systèmes individuels — moteurs, capteurs, vision — ne deviennent un robot que lorsqu'ils communiquent entre eux de manière fiable. ROS2 (Robot Operating System 2) fournit l'infrastructure de messagerie : les nœuds publient des données sur des topics, d'autres nœuds s'y abonnent et réagissent. Ce découplage vous permet de développer et de tester chaque composant indépendamment.

## Thèmes

- Les bases de ROS2 : nœuds, topics, messages
- Écrire une boucle complète perception–planification–action
- Journalisation, débogage et tests sur le matériel

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

1. Installez ROS2 Humble sur le Jetson (si ce n'est pas déjà fait).
2. Créez un package Python ROS2 et implémentez le `DriveNode` ci-dessus.
3. Écrivez un faux nœud capteur qui publie un message `Range` décroissant de 1,0 m à 0,1 m, pour tester l'évitement d'obstacles sans matériel.
4. Utilisez `ros2 topic echo` pour vérifier que les messages circulent correctement.
5. Dessinez le graphe des nœuds avec `rqt_graph`.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez aux questions suivantes :

- Qu'est-ce qu'un topic ROS2, et en quoi est-il différent d'un appel de fonction direct entre deux morceaux de code ?
- Pourquoi la séparation perception–planification–action est-elle utile, même pour un simple robot éviteur d'obstacles ?
- Que vérifieriez-vous en premier si un nœud publie mais que l'abonné ne reçoit pas les messages ?

## Extension

Modifiez le système ROS2 pour changer le comportement intégré du robot :

1. Changez le seuil d'obstacle de 30 cm à 50 cm — le robot devient plus prudent. Essayez 15 cm — il devient audacieux. Observez le changement de personnalité.
2. Ajoutez un nouveau nœud ROS2 qui publie un topic « humeur » basé sur la fréquence des obstacles rencontrés — l'état interne du robot devient observable.
3. Créez un nœud qui enregistre toutes les mesures des capteurs dans un fichier CSV — le robot a désormais une mémoire. Rejouez les données plus tard et vous pourrez voir exactement ce que le robot a vécu.
