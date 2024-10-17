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

## 1.2 Forward Kinematics

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

* **$a_{i-1}$** : the length of the link, i.e.  the distance of the common vertical line between $z_{i-1}$ and $z_i$

* **$\alpha_{i-1}$** : the translate angle between two joint coordinates, i.e the angle of rotation from $z_{i−1}$ to $z_{i}$ around $x_{i-1}$ axes
* $d_i$: the distance between two joint coordinates, i.e.  the distance of the common vertical line between $z_{i-1}$ and $z_i$
* 