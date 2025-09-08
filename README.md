# RealSense RTAB-Map VIO 

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

- Mapping conducted in structured indoor spaces
- Included hallways and rooms with furniture and varying layout
- Designed to test loop closures, graph optimization, and keypoint robustness

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

## Core SLAM Processes

The SLAM system uses RTAB-Map to incrementally build 3D maps while tracking the robot's position over time. It combines visual odometry, loop closure, and graph optimization to maintain an accurate and drift-corrected trajectory. As the robot explores, the system detects previously visited locations and refines the map for global consistency.

##  Loop Closure Detection

Loop closures were automatically detected by comparing current visual data with previously seen scenes. When a match was found, RTAB-Map estimated the correction and updated the pose graph to align the trajectory. This helped reduce drift and improve map consistency during longer sessions.

<table align="center">
  <tr>
    <td align="center">
      <img src="images/Loop Closure.gif" width="300"/><br/>
      <em>Loop Closure Detection</em>
    </td>
    <td align="center">
      <img src="images/Feature Matching.png" width="500"/><br/>
      <em>Feature Matching</em>
      <img src="images/3D Frame.png" width="445"/><br/>
      <em>Camera odometry frame and 3D point cloud projection </em>
    </td>
  </tr>
</table>

In the example above, a loop closure was detected between Frame 135 and Frame 87, applying a correction of Δx = -0.0117 m and Δθ = -0.43° to the trajectory.

## Graph Optimization

After detecting loop closures, RTAB-Map applies graph optimization to refine the map and reduce accumulated drift.  
This process removes redundant poses, merges overlapping data, and ensures the trajectory remains globally consistent.

<table align="center">
  <tr>
    <td align="center">
      <img src="images/First Frame Graph.png" width="200"/><br/>
      <em>Pose Graph Before Optimization</em>
    </td>
    <td align="center">
      <img src="images/Last Frame Graph.png" width="200"/><br/>
      <em>Pose Graph After Optimization</em>
    </td>
  </tr>
</table>

The global graph was reduced from 445 nodes to 407 nodes (an 8.5% reduction).  
This optimization improved the alignment of the robot’s trajectory, removing drift and enhancing overall map accuracy.

---

## Feature Detection: ORB vs. SIFT

Feature detection was tested with both ORB and SIFT to evaluate the trade-off between speed and accuracy.  
ORB is fast and lightweight, making it suitable for real-time operation, while SIFT is more computationally demanding but detects a larger number of robust features, especially in low-texture areas.

<table align="center">
  <tr>
    <td align="center">
      <img src="images/ORB Features.png" width="500"/><br/>
      <em>ORB Feature Detection</em>
    </td>
    <td align="center">
      <img src="images/SIFT Features.png" width="500"/><br/>
      <em>SIFT Feature Detection</em>
    </td>
  </tr>
</table>

- **ORB** – Efficient, real-time performance but fewer detected keypoints.  
- **SIFT** – Higher accuracy in challenging environments but slower processing.  

This comparison highlights the balance between real-time operation and feature richness in SLAM implementations.


## Generated 3D Maps

Below are two examples of 3D point cloud maps generated using RTAB-Map with the Intel RealSense D455. These maps were built in real time while maintaining a drift-corrected trajectory through loop closure and graph optimization.

<table align="center">
  <tr>
    <td align="center">
      <img src="images/Evironment_1_Map.PNG" width="800"/><br/>
      <em>Structured Hallway</em>
    </td>
    <td align="center">
      <img src="images/Evironment_2_Map.PNG" width="800"/><br/>
      <em>Lab Space with Dense Obstacles</em>
    </td>
  </tr>
</table>

- **Lab Environment** – Captured in a confined indoor lab with dense obstacles such as tables and chairs. The map clearly shows the workspace layout, with a well-defined trajectory loop in the center.
- **Corridor Environment** – Captured along a long structured hallway with multiple turns. The map maintains wall alignment and straight trajectories despite extended travel distance.

These results highlight the system’s ability to adapt to both **dense, cluttered spaces** and **open, structured layouts**, producing consistent maps in varied indoor settings.


