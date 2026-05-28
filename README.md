# Control Systems Simulation Portfolio

[cite_start]This repository contains two academic projects focused on designing, implementing, and evaluating closed-loop control systems using MATLAB and Simulink[cite: 9, 102]. [cite_start]Both systems are analyzed through time-domain simulations to verify stability, transient response, actuator limits, and robustness[cite: 8, 9, 101, 102].

## Developer Information
* [cite_start]**Student Name:** Tanagana Nikhil Patro [cite: 4, 97]
* [cite_start]**Student ID:** 2089827 [cite: 5, 98]
* [cite_start]**Course:** Control Systems [cite: 2, 95]
* [cite_start]**Instructor:** Prof. Pietro Falco [cite: 3, 96]

---

## Project 1: Drone Altitude Control (PID)

### Overview
[cite_start]The objective of this project is to design and evaluate a closed-loop control system to regulate a drone's vertical altitude[cite: 7]. [cite_start]The controller must maintain steady-state accuracy, satisfy strict physical actuator limits, reject external environmental disturbances (wind gusts), and remain stable despite parameter variations[cite: 8].

### System Modeling & Design
[cite_start]The vertical motion of the drone is governed by a second-order system[cite: 11]:
* [cite_start]Equation: m * h''(t) + b * h'(t) = u(t) - m * g + d(t) [cite: 12]

[cite_start]Where h(t) is altitude, u(t) is thrust force, m is drone mass, b is the damping coefficient, and d(t) is external disturbance[cite: 13]. [cite_start]By applying gravity compensation to regulate deviations directly around the hover condition[cite: 14], the model is simplified to:
* [cite_start]Simplified Equation: m * h''(t) + b * h'(t) = u_c(t) [cite: 16]

[cite_start]A Proportional-Integral-Derivative (PID) controller was implemented with the following parameters[cite: 18]:
* [cite_start]**Tuned Gains:** Kp = 7, Ki = 0.5, Kd = 5 [cite: 22]
* [cite_start]**Actuator Constraints:** Saturation block applied at +/-10 N [cite: 23]

### Performance Summary
[cite_start]The closed-loop architecture was simulated using a continuous-time solver over 10 seconds with a 1-meter unit step reference[cite: 25, 26].

| Scenario | Static Error | Overshoot | Rise Time (tr) | Settling Time (ts) | Max Effort (|u|) | Gust Recovery |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Nominal (m = 1.0 kg)** | [cite_start]~0 [cite: 43] | [cite_start]~4-5% [cite: 43] | [cite_start]~0.9 s [cite: 43] | [cite_start]~2.8 s [cite: 43] | [cite_start]<= 10 N [cite: 43] | — |
| **Mass Variation (m = 0.8 kg)** | [cite_start]<= 1% [cite: 43] | [cite_start]<= 5% [cite: 43] | [cite_start]~0.8 s [cite: 43] | [cite_start]<= 3 s [cite: 43] | [cite_start]<= 10 N [cite: 43] | — |
| **Mass Variation (m = 1.2 kg)** | [cite_start]<= 1% [cite: 43] | [cite_start]<= 5% [cite: 43] | [cite_start]~1.0 s [cite: 43] | [cite_start]<= 3 s [cite: 43] | [cite_start]<= 10 N [cite: 43] | — |
| **Step Disturbance (d = +0.1 N)**| [cite_start]~0 [cite: 43] | — | — | — | [cite_start]<= 10 N [cite: 43] | [cite_start]<= 1 s [cite: 43] |

* [cite_start]**Disturbance Rejection:** When a +/-0.1 N step disturbance is introduced at t = 4 s [cite: 34, 36][cite_start], the system returns safely to the +/-5% target band within approximately 1 second[cite: 35].
* [cite_start]**Robustness:** Testing mass variations of +/-20% (m = 0.8 kg and m = 1.2 kg) without retuning gains confirmed that the system remains stable and compliant with design targets[cite: 38, 39, 40].

---

## Project 2: Artificial Pancreas Glucose Regulation (PI)

### Overview
[cite_start]This project involves designing a medical-grade closed-loop control system for an artificial pancreas to regulate blood glucose concentration[cite: 100]. [cite_start]The controller safely mitigates a hyperglycemic condition, steering glucose levels back to a basal target while avoiding dangerous hypoglycemic drops or excessive insulin delivery rates[cite: 101].

### System Modeling & Design
[cite_start]The physiological glucose-insulin interactions are modeled utilizing a linearized three-state differential equation network tracking glucose deviations (G), insulin action (X), and plasma insulin concentration (I) around basal target boundaries[cite: 104, 105]:
* [cite_start]dG/dt = -k1 * G(t) - X(t) [cite: 106]
* [cite_start]dX/dt = -k2 * X(t) + k3 * I(t) [cite: 107]
* [cite_start]dI/dt = -n * I(t) + u(t) [cite: 108]

[cite_start]A conservative Proportional-Integral (PI) control algorithm was chosen to prioritize patient safety and prevent oscillatory insulin over-delivery[cite: 110, 124]:
* [cite_start]**Tuned Gains:** Kp = 0.02, Ki = 0.0003 [cite: 112, 113]
* [cite_start]**Basal Insulin Offset:** 0.3 [cite: 114] [cite_start](continuous background delivery [cite: 111])
* [cite_start]**Safety Saturation Limits:** 0 <= u(t) <= 3 [cite: 115]

### Performance Summary
[cite_start]Simulations evaluate a severe hyperglycemic starting point (130 mg/dL) tracked across a 200-unit temporal duration[cite: 117, 118].

| Scenario | Steady-State Glucose | Overshoot | Oscillations | Max Insulin | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Nominal Case** | [cite_start]~95-100 mg/dL [cite: 126] | [cite_start]None [cite: 126] | [cite_start]None [cite: 126] | [cite_start]<= 3 [cite: 126] | [cite_start]PASS [cite: 126] |
| **Safety Constraint** | >[cite_start]= 70 mg/dL [cite: 126] | — | — | [cite_start]<= 3 [cite: 126] | [cite_start]PASS [cite: 126] |
| **Actuator Limit** | — | — | — | [cite_start]<= 3 [cite: 126] | [cite_start]PASS [cite: 126] |

* [cite_start]**Clinical Safety Profile:** The glucose tracking curve falls smoothly into the basal target without any undershoot or volatile oscillations[cite: 120]. 
* [cite_start]**Actuator Bound Validation:** The insulin delivery signal remains within the specified saturation limits throughout the simulation, confirming safe operation[cite: 122].

---

## Repository Structure
```text
├── Drone_Altitude_Control/
│   ├── drone_altitude_model.slx   # Simulink model file
│   ├── drone_params.m             # Initialization script for gains and mass
│   └── Drone_Altitude_Control_Final_Report.pdf  # Project documentation
│
└── Artificial_Pancreas_Control/
    ├── pancreas_model.slx         # Simulink schematic 
    ├── pancreas_params.m          # Linearized state equations script
    └── Artificial_Pancreas_Report_MATCHED_FORMAT.pdf # Project documentation
