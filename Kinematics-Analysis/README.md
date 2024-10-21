# Kinematics Analysis

Before starting the project, we need to model and analyze the quadruped robots. Here is the mathematical model for the single leg of the quadruped robot provided by the manufacture and perform both forward and inverse kinematics analysis.

## 1. Forward Kinematics Analysis

![image-20220729141430256](http://www.yahboom.net/public/upload/upload-html/1692065522/image-20220729141430256.png)

In the robot simulation model above, each leg of the quadruped robot has three degrees of freedom, from top to bottom are the root joint (controlling the shoulder), the hip joint (controlling the thigh), and the knee joint (controlling the calf). The link between the root joint and the hip joint is defined as the base joint, the link between the hip joint and the knee joint is defined as the thigh, and the link between the knee joint and the foot is defined as the calf. The structure, shape and size parameters of the four legs are exactly the same, and the robot parameters are shown in the figure.

| symbol |                    Name                     | Value(mm) |
| :----: | :-----------------------------------------: | :-------: |
|  $L$   | Distance between Root Joints along $x$ axes |    45     |
|  $W$   | Distance between Root Joints along $y$ axes |    150    |
| $L_1$  |        Length of Basal segment link         |   31.6    |
| $L_2$  |            Length of thigh link             |    60     |
| $L_3$  |             Length of calf link             |    75     |

## 1.1 Forward Kinematics

Given the parameters:

+ joint angle of the manipulator
+ the length of the connecting rod

Solve:

+ the position of the end effector
+ attitude of the end effector

The inverse kinematics solution means that given the position and attitude of the end effector to solve the motion parameters of each joint. 



The commonly used kinematic modeling methods of quadruped robots include D-H method, Lie algebra method, etc.

Here introduce **D-H method:**

+ The idea of the D-H method is to establish a coordinate on all the links of the robot arm, and use the transformation matrix to describe the transformation relationship between the two adjacent links, which can be applied to the forward kinematics scene of a series mechanism with any degree of freedom.

+ But when there are too many degrees of freedom, there will be problems to use D-H method to solve the inverse kinematics. he solution is not unique and the can not solve inverse kinematics. 

+ However, since this robot dog has only three degrees of freedom per leg, the D-H method can be used for kinematic modeling.

![coordinate](http://www.yahboom.net/public/upload/upload-html/1692065522/image-20220729141430258.png)

As shown in the figure above, firstly establish the coordinate of the robot frame[body frame] and the coordinate of the single-leg frame, and then solve the forward kinematics of the single leg.

| Number of coordinate | $a_{i-1}$ | $\alpha_{i_1}$ | $d_i$ | angle of the joint |
| :------------------: | :-------: | :------------: | :---: | :----------------: |
|          1           |     0     |       0        |   0   |     $\theta_0$     |
|          2           |     0     |    $\pi/2$     | $L_1$ |     $\theta_1$     |
|          3           |   $L_2$   |       0        |   0   |     $\theta_2$     |
|          4           |   $L_3$   |       0        |   0   |         0          |

* **$a_{i-1}$** : the length of the joint axes, i.e.  the length of the common vertical line between $z_{i-1}$ and $z_i$ 
* **$\alpha_{i-1}$** : the translate angle between two joint coordinates, i.e the angle of rotation from $z_{i−1}$ to $z_{i}$ around $x_{i-1}$ axes
* $d_i$: the distance between two joint coordinates, i.e.  the distance of the common vertical line between $z_{i-1}$ and $z_i$
* angle of the joint: the rotation angle from old z-axis to new z-axis.

The spatial relationship between two adjacent connecting rod coordinate system{1} and {i-1} is described by the transformations:
$$
{}^{i-1}_i T = \text{Rot}(x, \alpha_{i-1}) \cdot \text{Trans}(x, a_{i-1}) \cdot \text{Rot}(x, \alpha_{i-1}) \cdot \text{Trans}(z, d_i) =
\\
\begin{bmatrix}
c\theta_i & -s\theta_i & 0 & a_{i-1} \\
s\theta_i c\alpha_{i-1} & c\theta_i c\alpha_{i-1} & -s\alpha_{i-1} & -d_i s\alpha_{i-1} \\
s\theta_i s\alpha_{i-1} & c\theta_i s\alpha_{i-1} & c\alpha_{i-1} & d_i c\alpha_{i-1} \\
0 & 0 & 0 & 1
\end{bmatrix}\tag{2-1}
$$
then we get:
$$
{}^0_1 T =
\begin{bmatrix}
c\theta_1 & -s\theta_1 & 0 & 0 \\
s\theta_1 & c\theta_1 & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}\tag{2-2}
$$

$$
{}^1_2 T =
\begin{bmatrix}
c\theta_2 & -s\theta_2 & 0 & 0 \\
0 & 0 & -1 & -L_1 \\
s\theta_2 & c\theta_2 & 0 & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}\tag{2-3}
$$

$$
{}^2_3 T =
\begin{bmatrix}
c\theta_3 & -s\theta_3 & 0 & L_2 \\
s\theta_3 & c\theta_3 & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}\tag{2-4}
$$

$$
{}^3_4 T =
\begin{bmatrix}
1 & 0 & 0 & L_3 \\
0 & 1 & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}\tag{2-5}
$$

$$
{}^{B}_0 T =
\begin{bmatrix}
0 & 0 & 1 & L/2 \\
0 & -1 & 0 & W/2 \\
1 & 0 & 0 & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}\tag{2-6}
$$

Combine these above matrix, we get $^{B}_{4}T$ :

$$
{}^B_4 T = {}^B_0 T \cdot {}^0_1 T \cdot {}^1_2 T \cdot {}^2_3 T \cdot {}^3_4 T =
\\
\begin{bmatrix}
S_{23} & c_{23} & 0 & L/2 + L_2 S_2 - L_3 S_{23} \\
-S_1 c_{23} & S_1 S_{23} & c_1 & W/2 + L_1 c_1 + L_2 S_1 c_2 - L_3 S_1 c_{23} \\
c_1 c_{23} & -c_1 S_{23} & S_1 & L_1 S_1 + L_2 c_1 c_2 - L_3 c_1 c_{23} \\
0 & 0 & 0 & 1
\end{bmatrix}\tag{2-7}
$$
If we break down $^{B}_{4}T$  into: 
$$
{}^B_4 T =
\begin{bmatrix}
&&& P_x \\
&R&& P_y \\
&&& P_z \\
0 & 0 & 0 & 1
\end{bmatrix}\tag{2-8}
$$

$$
R =
\begin{bmatrix}
S_{23} & c_{23} & 0 \\
-S_1 c_{23} & S_1 S_{23} & c_1 \\
c_1 c_{23} & -c_1 S_{23} & S_1
\end{bmatrix}\tag{2-9}
$$

$$
P =
\begin{bmatrix}
{}^B P_x \\
{}^B P_y \\
{}^B P_z
\end{bmatrix}
=
\begin{bmatrix}
L/2 + L_2 s_2 - L_3 s_{23} \\
W/2 + L_1 c_1 + L_2 s_1 c_2 - L_3 s_1 c_{23} \\
L_1 s_1 + L_2 c_1 c_2 - L_3 c_1 c_{23}
\end{bmatrix} \tag{2-10}
$$

In formula (2-8), R represents the posture of the foot end coordinate system relative to the body coordinate system, and the foot end posture is not considered in the plane free gait planning problem, and P represents the position of the foot end in the body coordinate system. Equation (2-10) is the positive kinematics solution of the left front leg of the quadruped robot. If the three joint angles of the leg are known, the position of the foot end relative to the coordinate system can be obtained by bringing it into the equation.   

## 1.2 Inverse Kinematics

### 1.2.1 Modeling of single leg inverse kinematics

In the quadruped robot motion control problem, the trajectory planning of the foot end should be carried out first. This requires the inverse kinematics solution, according to the given foot trajectory, to solve the corresponding joint angles, and use it as the input of the steering gear, so that the foot can move  according to the specified trajectory.
$$
\theta_1 = \arcsin\left(\frac{L \cdot y - L_1 \cdot z}{y^2 + z^2}\right) \tag{2-11}
$$

$$
\theta_3 = \arcsin\left(\frac{x^2 + y^2 + z^2 - L_1^2 - L_2^2 - L_3^2}{2 \cdot L_2 \cdot L_3}\right) \tag{2-12}
$$

$$
\theta_2 = \arcsin\left(\frac{L_3 \cdot c_3 \cdot L - (L_3 \cdot s_3 + L_2) \cdot x}{(L_3 \cdot s_3 + L_2)^2 + (L_3 \cdot c_3)^2}\right) \tag{2-13}
$$

where

$$
L = \sqrt{y^2 + z^2 - L_1^2} \tag{2-14}
$$
According to equations(2-11) to (2-14), if given the coordinates of the foot end, the joint angles corresponding to the left front leg can be solved.

