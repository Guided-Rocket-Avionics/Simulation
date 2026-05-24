# Guided Model Rocket — GNC Simulation

Flight simulation, sensor fusion, and attitude control for a custom-built guided model rocket with active Thrust Vector Control (TVC).

This repository is the GNC (Guidance, Navigation & Control) layer of a three-subsystem avionics project built entirely from scratch by a team of ECE students. No off-the-shelf flight controller is used at any layer.

---

## What This Repository Contains

| Folder | Contents |
|---|---|
| `models/` | 3DOF and 6DOF flight dynamics, Extended Kalman Filter |
| `controllers/` | PID attitude controller |
| `data/motor_curves/` | Thrust curve CSVs for motor characterisation |
| `tests/` | Validation scripts — simulation output vs OpenRocket |
| `notebooks/` | Exploration and visualisation notebooks |
| `docs/` | Design decisions, derivations, reference notes |

---

## Project Overview

The full avionics system consists of three subsystems:

- **GNC & Simulation** (this repo) — 6DOF Python flight simulation, Extended Kalman Filter for state estimation, PID attitude controller
- **Firmware** — STM32 FreeRTOS firmware, sensor drivers, flight state machine, SD card logging
- **Hardware** — KiCad 4-layer custom PCB, STM32F405, IMU, barometer, magnetometer, pyrotechnic channel

The simulation is built first and used to validate all control system design before any hardware is touched. PID gains are tuned here. The Kalman filter is validated here against simulated noisy sensor data. Only after simulation validation does the filter get ported to C on the STM32.

---

## Simulation Architecture

### 3DOF Model (`models/threedof.py`)
Point-mass rocket simulation. Inputs: thrust curve, mass, drag coefficient, launch angle. Outputs: altitude, velocity, acceleration vs time. Validated against OpenRocket within 10%.

### 6DOF Model (`models/sixdof.py`)
Full rigid body dynamics. Adds rotational degrees of freedom — pitch, yaw, roll. Aerodynamic moments calculated using Barrowman equations for Centre of Pressure. Wind disturbance input included.

### Extended Kalman Filter (`models/kalman_filter.py`)
9-state EKF fusing IMU accelerometer, gyroscope, barometer, and magnetometer data. State vector: [x, y, z, vx, vy, vz, roll, pitch, yaw]. Sensor noise modelled from datasheet Allan variance values. Validated by comparing estimated state against known 6DOF truth.

### PID Controller (`controllers/pid.py`)
Attitude controller commanding TVC gimbal deflection. Tuned in simulation loop before hardware implementation. Step response documented — settling time and overshoot targets defined before porting to firmware.

---

## Dependencies

```bash
pip install -r requirements.txt
```

Core packages: `numpy`, `scipy`, `matplotlib`, `pandas`, `jupyter`

Python 3.11+ recommended.

---

## Getting Started

```bash
git clone https://github.com/Guided-Rocket-Avionics/simulation.git
cd simulation
python -m venv venv
source venv/bin/activate    # Windows: venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook notebooks/exploration.ipynb
```

---

## Development Status

| Component | Status |
|---|---|
| 3DOF point-mass model | In progress |
| 6DOF rigid body model | Not started |
| Barrowman CP calculation | Not started |
| Extended Kalman Filter (Python) | Not started |
| PID attitude controller | Not started |
| EKF port to C (STM32) | Not started |
| Validation against OpenRocket | Not started |

---

## Related Repositories

- [`firmware`](https://github.com/YOUR-ORG/firmware) — STM32 FreeRTOS flight computer firmware
- [`hardware`](https://github.com/YOUR-ORG/hardware) — KiCad PCB design
