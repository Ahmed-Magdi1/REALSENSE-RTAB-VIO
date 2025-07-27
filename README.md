# RealSense RTAB-Map VIO Demo

This project showcases a complete implementation of visual-inertial SLAM using the Intel RealSense D455 and RTAB-Map within the ROS environment.


##  Overview

The project highlights practical integration through the following key capabilities:

- Real-time 3D mapping using RGB-D and IMU data
- Visual odometry with both **ORB and SIFT** feature detectors (compared side-by-side)
- **Loop closure detection** to correct trajectory drift
- **Pose graph optimization** for global consistency
- Generation of **dense point cloud maps**
- Visualizations of the **trajectory**, **graph structure**, and **loop closures**

---

## Project Setup

This project was implemented on a mobile robotic system for testing visual-inertial SLAM performance in real-world indoor settings.

### Hardware Setup

- **Intel RealSense D455**
  - RGB-D camera with onboard IMU
  - Used for both visual odometry and depth mapping
- **Clearpath Husky UGV**
  - Served as the mobile base to replicate realistic ground robot operation
- **Onboard Laptop**
  - Runs ROS Noetic and all mapping processes
  - Specs: NVIDIA GTX 1660 Ti (mobile), Intel Core i7-9750h, 16GB RAM

<p align="center">
  <img src="images/Husky UAV.PNG" width="300"/>
</p>

---

### Test Environment

- Mapping conducted in **structured indoor spaces**
- Included **hallways** and **rooms with furniture and varying layout**
- Designed to test **loop closures**, **graph optimization**, and **keypoint robustness**

<table align="center">
  <tr>
    <td align="center">
      <img src="images/Environment 1.PNG" width="300"/><br/>
      <em>Structured Hallway</em>
    </td>
    <td align="center">
      <img src="images/Environment 2.PNG" width="400"/><br/>
      <em>Lab Space with Dense Obstacles</em>
    </td>
  </tr>
</table>
