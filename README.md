# 🤖 Autonomous Connect 4 Robot System (Niryo Ned2)

An end-to-end autonomous mechatronics system where a **Niryo Ned2 6-axis robotic arm** plays Connect 4 against a human opponent in real-time.

This project demonstrates a complete:

> **Perception → Decision → Action** pipeline  

integrating **Computer Vision**, **Minimax AI**, and **precision robotic actuation**.

---

## 📺 Project Demonstration

🎥 Watch the full project video on YouTube  

*([Insert your YouTube link here](https://drive.google.com/file/d/156Vr7NH8XyMnDlW1Xb9NzeY3vUy2lh4W/view?usp=sharing))*

---

## 🚀 Key Features

- **Closed-Loop Integration**  
  Fully autonomous feedback loop using OpenCV, strategic AI, and Niryo mechatronics.

- **Strategic AI**  
  Minimax algorithm with Alpha-Beta Pruning and a custom heuristic evaluation function.

- **Robust Vision**  
  HSV-based color segmentation and centroid-to-grid mapping to handle real-world lighting conditions.

- **Precision Robotics**  
  Collision-safe trajectory planning and automated Round-Robin coin management.

- **Configuration Dashboard**  
  GUI-based setup for difficulty levels (Easy, Medium, Hard) and player settings.

- **Mock Hardware Support**  
  Integrated simulation mode (`_MockNiryoRobot`) for full logic testing without physical hardware.

---

# 🧠 Technical Deep Dive

---

### Artificial Intelligence (The Brain)

The decision engine (`ai_strategy.py`) simulates future game states to determine the optimal move.

### Optimization  
**Alpha-Beta Pruning** allows the search tree to reach deeper depths (up to 8 moves) within the same time window by cutting off unpromising branches.

### Heuristic Evaluation Function

Positions are scored using a weighted sliding window approach:

| Scenario | Score |
|----------|--------|
| Win (4-in-a-row) | +100,000 |
| Threat (3-in-a-row) | +100 |
| Blocking Opponent | -120 |
| Center Column Bonus | +4 |

This prioritizes:
- Offensive dominance  
- Defensive stability  
- Strong center control  

---

## 2️⃣ Computer Vision (The Eyes)

Built using **OpenCV**, the vision pipeline digitizes the physical board state.

### HSV Segmentation
- Dual-mask approach for **Red hue wrap-around** (0° and 180°)
- Single-mask thresholding for **Yellow**

### Centroid Mapping
- Uses `cv2.moments`
- Calculates exact disc centers
- Maps them to the grid via pixel-to-coordinate division

### Gravity-Aware Logic
- Detects human moves by frame differencing
- Selects the lowest changed cell in a column
- Ignores falling disc artifacts

---

## 3️⃣ Robotics (The Hands)

Controlled using the `pyniryo` library with custom motion primitives.

### Suction End-Effector
- Vacuum pump for reliable, non-marring pickup of flat coins

### Stack Management
A `CoinManager` tracks:
- 3 distinct coin stacks  
- 21 coins total  
- Round-robin cycling to prevent supply depletion

### Safe Trajectories
- Enforces `SAFE_Z = 0.40m` during translational movements
- Prevents board collisions
- Avoids Inverse Kinematics failures

---

# 📁 Project Structure

```bash
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
```

---

# 👥 Contributors

- **Satya Naga Vamsi Ganesh Manepalli**
- **Arpitha Thimmegowda**
 
**Institution:** OTH Amberg-Weiden  
