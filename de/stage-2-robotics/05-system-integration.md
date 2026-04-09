# Systemintegration

## Konzept

Einzelne Subsysteme — Motoren, Sensoren, Vision — werden erst zu einem Roboter, wenn sie zuverlässig miteinander kommunizieren. ROS2 (Robot Operating System 2) liefert das Messaging-Rückgrat: Nodes veröffentlichen Daten auf Topics, andere Nodes abonnieren und reagieren. Diese Entkopplung ermöglicht es, jedes Teil unabhängig zu entwickeln und zu testen.

## Themen

- ROS2-Grundlagen: Nodes, Topics, Messages
- Eine vollständige Sense-Plan-Act-Schleife schreiben
- Logging, Debugging und Testen auf Hardware

## Code

```python
# ros2_drive_node.py — ein minimaler ROS2-Python-Node
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
        self.get_logger().info("DriveNode gestartet")

    def on_range(self, msg: Range):
        cmd = Twist()
        if msg.range < 0.3:          # Hindernis näher als 30 cm
            cmd.linear.x  = 0.0
            cmd.angular.z = 0.5      # auf der Stelle drehen
            self.get_logger().warn(f"Hindernis bei {msg.range:.2f} m — drehe")
        else:
            cmd.linear.x = 0.3       # vorwärts fahren
        self.publisher.publish(cmd)

def main():
    rclpy.init()
    node = DriveNode()
    rclpy.spin(node)
    node.destroy_node()
    rclpy.shutdown()
```

```bash
# In separaten Terminals ausführen
ros2 run your_package drive_node
ros2 topic echo /cmd_vel            # Nachrichten inspizieren
ros2 topic hz  /ultrasonic/front    # Sensorrate prüfen
ros2 run rqt_graph rqt_graph        # Node-Verbindungen visualisieren
```

```
Sense → Plan → Act Schleife
─────────────────────────────────────────────────
Sensor-Node  veröffentlicht /ultrasonic/front (Range)
Drive-Node   abonniert, entscheidet Geschwindigkeit
Drive-Node   veröffentlicht /cmd_vel (Twist)
Motor-Node   abonniert, setzt PWM auf Hardware
```

## Aktion

1. Installiere ROS2 Humble auf dem Jetson (falls noch nicht vorhanden).
2. Erstelle ein ROS2-Python-Paket und implementiere den obigen `DriveNode`.
3. Schreibe einen Fake-Sensor-Node, der eine `Range`-Nachricht veröffentlicht, die von 1,0 m auf 0,1 m abnimmt, um die Hindernisvermeidung ohne Hardware zu testen.
4. Nutze `ros2 topic echo`, um zu verifizieren, dass Nachrichten korrekt fließen.
5. Zeichne den Node-Graphen mit `rqt_graph`.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Was ist ein ROS2-Topic, und wie unterscheidet es sich von einem direkten Funktionsaufruf zwischen zwei Codeteilen?
- Warum ist die Sense-Plan-Act-Trennung nützlich, selbst für einen einfachen hindernisausweichenden Roboter?
- Was würdest du zuerst prüfen, wenn ein Node veröffentlicht, aber der Subscriber keine Nachrichten empfängt?

## Erweiterung

Verändere das ROS2-System, um das integrierte Verhalten des Roboters zu ändern:

1. Ändere den Hindernisschwellenwert von 30 cm auf 50 cm — der Roboter wird vorsichtiger. Versuche 15 cm — er wird mutiger. Beobachte die Persönlichkeitsverschiebung.
2. Füge einen neuen ROS2-Node hinzu, der ein „Stimmungs"-Topic veröffentlicht, basierend darauf, wie oft Hindernisse auftreten — der interne Zustand des Roboters wird beobachtbar.
3. Erstelle einen Node, der alle Sensorwerte in eine CSV-Datei loggt — der Roboter hat jetzt Gedächtnis. Spiele die Daten später ab und du kannst genau sehen, was der Roboter erlebt hat.
