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

* **$a_{i-1}$** : the length of the joint axes, i.e.  the length of the common vertical line between $z_{i-1}$ and $z_i$
* **$\alpha_{i-1}$** : the translate angle between two joint coordinates, i.e the angle of rotation from $z_{i−1}$ to $z_{i}$ around $x_{i-1}$ axes
* $d_i$: the distance between two joint coordinates, i.e.  the distance of the common vertical line between $z_{i-1}$ and $z_i$
* angle of the joint: the rotation angle from to along.

The spatial relationship between two adjacent connecting rod coordinate system{1} and {i-1} is described by the transformations:

![image-20220729143811431](https://www.yahboom.com/public/upload/upload-html/1660303584/image-20220729143811431.png)

then we get:

![image-20220729144603046](https://www.yahboom.com/public/upload/upload-html/1660303584/image-20220729144603046.png)

![image-20220729144624309](https://www.yahboom.com/public/upload/upload-html/1660303584/image-20220729144624309.png)

![image-20220729144653868](https://www.yahboom.com/public/upload/upload-html/1660303584/image-20220729144653868.png)

![image-20220729144712769](https://www.yahboom.com/public/upload/upload-html/1660303584/image-20220729144712769.png)

![image-20220729144736267](https://www.yahboom.com/public/upload/upload-html/1660303584/image-20220729144736267.png)

Combine these above matrix, we get $^{B}_{4}T$ :

![image-20220729144840491](https://www.yahboom.com/public/upload/upload-html/1660303584/image-20220729144840491.png)

![image-20220729144952717](https://www.yahboom.com/public/upload/upload-html/1660303584/image-20220729144952717.png)

![image-20220729145022103](https://www.yahboom.com/public/upload/upload-html/1660303584/image-20220729145022103.png)

![image-20220729145047731](https://www.yahboom.com/public/upload/upload-html/1660303584/image-20220729145047731.png)

In formula (2-8), R represents the posture of the foot end coordinate system relative to the body coordinate system, and the foot end posture is not considered in the plane free gait planning problem, and P represents the position of the foot end in the body coordinate system. Equation (2-10) is the positive kinematics solution of the left front leg of the quadruped robot. If the three joint angles of the leg are known, the position of the foot end relative to the coordinate system can be obtained by bringing it into the equation.   

It is known that the root joint angle range is [−50∘, 90∘], the hip joint angle range is [−90∘, 80∘], and the knee joint angle range is [−60∘, 45∘]. Use the robotics toolbox in matlab to create a single-leg model of a quadruped robot, and traverse each joint angle within a limited range to obtain the foot end workspace, as shown in Figure 2-3.

![image-20220729145047732](http://www.yahboom.net/public/upload/upload-html/1692065522/image-20220729145047732.png)