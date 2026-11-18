<div align="center">

# DeepWheat Drones

**Precision Agriculture Multi-Agent System**

A fleet of autonomous drones that looks after wheat fields. Scout drones photograph every plant and spot disease with the DeepWheat image model, payload drones spray fungicide or fertiliser where it's needed, and SPADE agents decide which drone does what based on battery, wind, weather and the crop calendar.

![Python](https://img.shields.io/badge/Python-3.10-3776AB?style=for-the-badge&logo=python&logoColor=white)
![SPADE](https://img.shields.io/badge/SPADE-4.0-5C2D91?style=for-the-badge&logoColor=white)
![ROS 2](https://img.shields.io/badge/ROS_2-22314E?style=for-the-badge&logo=ros&logoColor=white)
![PX4](https://img.shields.io/badge/PX4-0C1E3C?style=for-the-badge&logoColor=white)
![TensorFlow](https://img.shields.io/badge/TensorFlow-FF6F00?style=for-the-badge&logo=tensorflow&logoColor=white)

[Objectives](#objectives) · [Features](#features) · [Architecture](#system-architecture) · [Agents](#agent-specifications) · [Setup](#environment-setup) · [Behaviour](#system-behavior) · [Team](#team)

<sub>Challenge 4 · Master's in Artificial Intelligence Engineering · <a href="https://www.isep.ipp.pt">ISEP</a>, Porto · 2024–25</sub>

</div>

---

## 🚀 Key Features

### **Multi-Agent Coordination**
- **Contract Net Protocol** for optimal task allocation
- **Dynamic drone competition** based on battery levels and wind conditions
- **Parallel field operations** with intelligent request queuing
- **Real-time proposal evaluation** and task assignment

### **Smart Agriculture Operations**
- **Autonomous field monitoring** with disease detection
- **Weekly preventive monitoring** to ensure crop health
- **Precision fertilization** during growth seasons
- **Targeted pesticide application** for diseased plants
- **Weather-aware decision making** using OpenWeatherMap API

### **Intelligent Resource Management**
- **Dynamic battery management** with automatic recharging
- **Wind condition assessment** for optimal flight planning
- **Seasonal behavior adaptation** based on crop calendar
- **Load balancing** across multiple drone agents

---

## 🏗️ System Architecture

### **Agent Hierarchy**
```
CentralAgent (Coordinator)
├── Contract Net Protocol Manager
├── Request Queue Management
└── Drone Fleet Coordination
    ├── PayloadDrone Fleet (3 drones)
    │   ├── Fertilization Operations
    │   ├── Pesticide Application
    │   └── Battery Management
    ├── VigilantDrone Fleet (2 drones)
    │   ├── Field Monitoring
    │   ├── Disease Detection
    │   └── Plant Scanning
    └── FieldAgent Network (2 fields)
        ├── Weather Monitoring
        ├── Crop Status Tracking
        └── Request Generation
```

### **Communication Flow**
1. **Field Agents** monitor weather and crop conditions
2. **Weekly monitoring requests** sent to ensure crop health
3. **Fertilization requests** sent during growth season
4. **CentralAgent** broadcasts CFP (Call for Proposals)
5. **All available drones** submit competitive proposals
6. **Best proposal wins** based on battery + wind scoring
7. **Parallel operations** with automatic request queuing
8. **Disease alerts** trigger targeted treatment requests

---

## 📁 Project Structure
```
deepWheat/
├── agents/
│   ├── central_agent.py     # Contract Net Protocol coordinator
│   ├── field_agent.py       # Field monitoring and requests
│   ├── payload_drone.py     # Fertilization/pesticide operations
│   └── vigilant_drone.py    # Disease detection and monitoring
├── utils/
│   ├── battery.py           # Battery usage calculations
│   ├── field_map.py         # Shared field state management
│   ├── logger.py            # Color-coded logging system
│   ├── negotiation.py       # Proposal evaluation logic
│   ├── season.py            # Growth season detection
│   └── weather.py           # OpenWeatherMap integration
├── config.py                # System configuration
├── main.py                  # Simulation launcher
└── README.md               # This file
```

---

## 🤖 Agent Specifications

### **CentralAgent** - System Coordinator
- **Responsibilities**: Task assignment, drone coordination, request queuing
- **Protocol**: Contract Net Protocol with FSM-based state management
- **Features**: Dynamic proposal evaluation, parallel request processing
- **Drone Fleet**: Manages 3 payload drones + 2 vigilant drones

### **FieldAgent** - Field Management
- **Responsibilities**: Weather monitoring, crop status, fertilization requests, weekly monitoring
- **Data Sources**: OpenWeatherMap API, growth season calendar
- **Features**: Daily initialization, weekly preventive monitoring, treatment tracking, completion monitoring
- **Coverage**: Each agent manages one 2x5 grid field (10 plants)

### **PayloadDroneAgent** - Operations Execution
- **Responsibilities**: Fertilization, pesticide application, battery management
- **Features**: Proposal generation, task execution, automatic recharging
- **Operations**: Full-field fertilization, targeted plant treatment
- **Fleet Size**: 3 drones competing for tasks

### **VigilantDroneAgent** - Monitoring & Detection
- **Responsibilities**: Field scanning, disease detection, alert generation
- **Features**: Grid-based plant scanning, disease reporting, battery management
- **Coverage**: Plant-by-plant monitoring with position tracking
- **Fleet Size**: 2 drones for field surveillance

---

## 🛠️ Environment Setup

### **System Requirements**
- Ubuntu 22.04 LTS (recommended)
- Python 3.8+
- Prosody XMPP Server
- OpenWeatherMap API key

### **1. Install System Dependencies**
```bash
sudo apt update
sudo apt install python3 python3-pip python3-venv prosody -y
```

### **2. Setup Virtual Environment**
```bash
python3 -m venv spade_env
source spade_env/bin/activate
```

### **3. Install Python Dependencies**
```bash
pip install spade==4.0.3 requests aioxmpp termcolor
```

### **4. Configure Prosody XMPP Server**
```bash
# Start Prosody service
sudo systemctl start prosody
sudo systemctl enable prosody

# Create agent accounts
sudo prosodyctl adduser central@localhost
sudo prosodyctl adduser field1@localhost
sudo prosodyctl adduser field2@localhost
sudo prosodyctl adduser payload1@localhost
sudo prosodyctl adduser payload2@localhost
sudo prosodyctl adduser payload3@localhost
sudo prosodyctl adduser vigilant1@localhost
sudo prosodyctl adduser vigilant2@localhost
```

### **5. Configure Weather API**
Update `config.py` with your OpenWeatherMap API key:
```python
WEATHER_API_KEY = "your_api_key_here"
WEATHER_LAT = 41.1496  # Your latitude
WEATHER_LON = -8.6109  # Your longitude
```

---

## ▶️ Running the Simulation

### **Start the System**
```bash
python main.py
```

### **Expected Output**
```
🚁 Managing 2 vigilant drones and 3 payload drones
🔍 First monitoring request for field_1
📤 Weekly monitoring request sent for field_1
📤 Sending CFP for fertilization_request: field_1|3.09 to 3 payload drones
📤 Sending CFP for monitoring_request: field_1|3.09 to 2 vigilant drones
📊 Proposal from payload1: Battery=100.0%, Wind=10.58km/h, Score=0.65
📊 Proposal from vigilant1: Battery=100.0%, Wind=12.44km/h, Score=0.59
🏆 Best proposal: payload3 (score: 0.70)
✅ Assigned fertilization_request to payload3
🦠 Disease (white_stripe) detected at field_1 (0, 1)
🎉 Treatment COMPLETE @ field_1 (0,1) - plant is healthy!
```

### **Stop the Simulation**
Press **Q** and hit **Enter** for graceful shutdown.

---

## ⚙️ Configuration Options

### **Battery Management**
```python
BATTERY_LOW_THRESHOLD = 45      # Recharge below 45%
BATTERY_RECHARGE_STEP = 20      # Recharge 20% per interval
RECHARGE_INTERVAL = 1           # 1 second per recharge step
```

### **Seasonal Behavior**
```python
GROWTH_SEASON_START = (3, 15)   # March 15
GROWTH_SEASON_END = (6, 15)     # June 15
DEFAULT_CROP_TYPE = "spring"    # Spring wheat
```

### **Environmental Conditions**
```python
WIND_MIN = 5                    # Minimum wind speed (km/h)
WIND_MAX = 15                   # Maximum wind speed (km/h)
FLIGHT_TIME = 2                 # Flight time between positions (seconds)
APPLICATION_TIME = 2            # Application time per plant (seconds)
```

### **Field Configuration**
```python
FIELD_ROWS = 2                  # Grid rows per field
FIELD_COLS = 5                  # Grid columns per field

# Predefined diseased plants
DISEASED_PLANTS = {
    "field_1": [(0, 1, "white_stripe"), (1, 3, "brown_rust")],
    "field_2": [(1, 2, "yellow_rust"), (0, 4, "septoria")]
}
```

---

## 🔬 System Behavior

### **Weekly Monitoring System**
- **First Run**: Always requests monitoring for immediate crop assessment
- **Subsequent Runs**: Automatically requests monitoring every 7 days
- **Preventive Care**: Ensures regular field inspection regardless of detected conditions
- **Simple Logic**: Uses date comparison to track last monitoring request

### **Contract Net Protocol Flow**
1. **Field Agent** detects fertilization need
2. **Central Agent** broadcasts CFP to all payload drones
3. **Available drones** submit proposals with battery/wind data
4. **Proposals evaluated** using scoring algorithm: `battery_score - wind_penalty`
5. **Best drone selected** and assigned the task
6. **Other drones rejected** and remain available for future tasks

### **Proposal Evaluation Algorithm**
```python
def evaluate_proposal(battery_level, wind_speed):
    battery_score = battery_level / 100.0  # Normalize to 0-1
    wind_penalty = wind_speed / 30.0       # Assume max wind 30km/h
    return battery_score - wind_penalty    # Higher score = better drone
```

### **Disease Detection Workflow**
1. **Vigilant Drone** scans field grid systematically (weekly + on-demand)
2. **Disease detected** at specific plant coordinates
3. **Field Agent** receives disease alert with position
4. **Treatment request** sent if plant not already being treated
5. **Payload Drone** applies pesticide to specific plant
6. **Treatment completion** reported back to Field Agent

---

## 📊 Monitoring & Logging

### **Color-Coded Logging**
- **🔵 Central Agent**: Blue - Coordination and task assignment
- **🟢 Vigilant Drones**: Green - Monitoring and detection
- **🔴 Payload Drones**: Red - Operations and battery status
- **🟡 Field Agents**: Yellow - Field conditions and requests

### **Key Metrics Tracked**
- Drone battery levels and recharge cycles
- Wind conditions and flight efficiency
- Weekly monitoring schedule compliance
- Task completion rates and timing
- Proposal scores and selection rationale
- Disease detection and treatment effectiveness

---

## 🔄 Extensibility

### **Adding More Drones**
Simply update the range in `main.py`:
```python
payload_drones = [
    PayloadDroneAgent(f"payload{i}@localhost", "admin1234")
    for i in range(1, 5)  # Creates payload1 through payload4
]
```

### **Adding More Fields**
Update `config.py`:
```python
FIELD_AGENTS = [
    {"agent_jid": "field1@localhost", "field_id": "field_1"},
    {"agent_jid": "field2@localhost", "field_id": "field_2"},
    {"agent_jid": "field3@localhost", "field_id": "field_3"}  # New field
]
```

### **Custom Disease Types**
Add new diseases to `DISEASED_PLANTS` configuration:
```python
DISEASED_PLANTS = {
    "field_1": [(0, 1, "powdery_mildew"), (1, 3, "leaf_spot")],
    # ... additional disease configurations
}
```

---

## 🚀 Future Enhancements

### **Planned Features**
- [ ] **ROS2 Integration** for physical drone simulation
- [ ] **Gazebo 3D Environment** for realistic field visualization
- [ ] **Machine Learning** for disease prediction
- [ ] **IoT Sensor Integration** for real-time field data
- [ ] **Mobile Dashboard** for monitoring and control

### **Research Applications**
- Multi-agent coordination algorithms
- Precision agriculture optimization
- Autonomous vehicle fleet management
- Environmental monitoring systems
- Supply chain and logistics optimization

---

## 📜 License

This project is developed for research and educational purposes in precision agriculture and multi-agent systems.

---

## Team

<table align="center">
  <tr>
    <td align="center" width="150">
      <a href="https://github.com/Marco-Nobrega-Andrade"><img src="https://github.com/Marco-Nobrega-Andrade.png?size=200" width="100" alt="Marco Andrade"/></a><br>
      <b>Marco Andrade</b><br>
      <a href="https://github.com/Marco-Nobrega-Andrade">@Marco-Nobrega-Andrade</a>
    </td>
    <td align="center" width="150">
      <a href="https://github.com/SrMasterAD"><img src="https://github.com/SrMasterAD.png?size=200" width="100" alt="David Dias"/></a><br>
      <b>David Dias</b><br>
      <a href="https://github.com/SrMasterAD">@SrMasterAD</a>
    </td>
    <td align="center" width="150">
      <a href="https://github.com/nelsonnovais"><img src="https://github.com/nelsonnovais.png?size=200" width="100" alt="Nelson Novais"/></a><br>
      <b>Nelson Novais</b><br>
      <a href="https://github.com/nelsonnovais">@nelsonnovais</a>
    </td>
    <td align="center" width="150">
      <a href="https://github.com/ricardoham"><img src="https://github.com/ricardoham.png?size=200" width="100" alt="Ricardo Manoel"/></a><br>
      <b>Ricardo Manoel</b><br>
      <a href="https://github.com/ricardoham">@ricardoham</a>
    </td>
    <td align="center" width="150">
      <a href="https://github.com/binaps"><img src="https://github.com/binaps.png?size=200" width="100" alt="Sabrina"/></a><br>
      <b>Sabrina</b><br>
      <a href="https://github.com/binaps">@binaps</a>
    </td>
  </tr>
</table>

---

<div align="center">

**DeepWheat Drones** · *Autonomous Multi-Agent Precision Agriculture* 🌾🚁

</div>

Contributions are welcome! Please feel free to submit issues, feature requests, or pull requests to improve the deepWheat system.

---

**deepWheat** - *Autonomous Multi-Agent Precision Agriculture* 🌾🚁
