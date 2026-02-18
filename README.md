🤖 Autonomous Connect 4 Robot System (Niryo Ned2)

An end-to-end autonomous mechatronics system where a Niryo Ned2 6-axis robotic arm plays Connect 4 against a human opponent in real-time. This project demonstrates the complete perception → decision → action pipeline, integrating Computer Vision for environment sensing, Minimax AI for strategic reasoning, and precise robotic actuation.

📺 Project Demonstration

Watch the full project video on YouTube

🚀 Key Features

Closed-Loop Integration: Fully autonomous feedback loop using OpenCV, strategic AI, and Niryo mechatronics.

Strategic AI: Minimax algorithm with Alpha-Beta Pruning and a custom heuristic evaluation function.

Robust Vision: HSV-based color segmentation and centroid-to-grid mapping to handle real-world lighting.

Precision Robotics: Collision-safe trajectory planning and automated Round-Robin coin management.

Configuration Dashboard: GUI-based setup for difficulty levels (Easy, Medium, Hard) and player settings.

Mock Hardware Support: Integrated simulation mode (_MockNiryoRobot) for logic testing without physical hardware.

🧠 Technical Deep Dive

1. Artificial Intelligence (The Brain)

The decision engine (ai_strategy.py) simulates future game states to find the optimal column.

Optimization: Alpha-Beta Pruning allows the search tree to reach deeper depths (up to 8 moves) within the same time window by cutting off unpromising branches.

Heuristic Evaluation: Positions are scored using a weighted sliding window:

Win (4-in-a-row): $+100,000$

Threat (3-in-a-row): $+100$

Blocking Opponent: $-120$ (Prioritizes defensive stability)

Center Column Bonus: $+4$ (Maximizes connection potential)

2. Computer Vision (The Eyes)

Built with OpenCV, the vision pipeline digitizes the physical board state:

HSV Segmentation: Uses a dual-mask approach to handle the Red hue wrap-around ($0^{\circ}$ and $180^{\circ}$) and single-mask thresholding for Yellow.

Centroid Mapping: Uses Image Moments (cv2.moments) to calculate the exact center of detected discs, mapping them to the grid using pixel-to-coordinate division.

Gravity-Aware Logic: Detects human moves by differencing frames and selecting the lowest changed cell in a column to ignore falling discs.

3. Robotics (The Hands)

Controlled via the pyniryo library with custom movement primitives:

Suction End-Effector: Utilizes a vacuum pump for reliable, non-marring pickup of flat coins.

Stack Management: A CoinManager tracks 3 distinct coin stacks (21 coins total), cycling through them in a round-robin fashion to prevent supply depletion in a single zone.

Safe Trajectories: Enforces a SAFE_Z height (0.40m) during all translational movements to prevent board collisions and Inverse Kinematics (IK) failures.

🛠️ Physical Challenges & Engineering Solutions

The Suction Problem: Plastic coins were too textured for a vacuum seal.

Solution: Applied thin circular cardboard backings to provide a perfectly flat, non-porous surface.

Targeting Accuracy: The Connect 4 frame is extremely narrow ($<1$cm clearance).

Solution: Built a custom cardboard funnel and performed millimeter-level manual pose calibration for all 7 drop columns.

Development Constraints: Limited access to the physical robot.

Solution: Implemented a software Mock Robot abstraction layer that prints actions to the console, allowing full AI/Vision debugging offline.

📁 Project Structure

├── main.py              # Central Orchestrator & Game Loop
├── ai_strategy.py       # Minimax AI & Heuristic Logic
├── connect4_game.py     # Logical board representation & Win checks
├── connect4_robot.py    # Hardware Abstraction & Mock support
├── robot_actions.py     # Pick-and-Place movement primitives
├── robot_positions.py   # Calibrated 6D Poses (Pickup/Drop)
├── coin_manager.py      # Round-robin stack management
├── color_detection.py   # OpenCV HSV color segmentation
├── map_discs_to_grid.py # Centroid & Pixel-to-Grid mapping logic
└── dashboard.py         # Tkinter-based configuration UI


👥 Contributors

Satya Naga Vamsi Ganesh Manepalli

Arpitha Thimmegowda

Supervised by: Prof. Dr.-Ing. Thomas Nierhoff

Institution: OTH Amberg-Weiden
