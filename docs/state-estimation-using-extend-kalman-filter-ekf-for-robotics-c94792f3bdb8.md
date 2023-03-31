# 机器人状态估计的扩展卡尔曼滤波器(EKF)

> 原文：<https://medium.com/analytics-vidhya/state-estimation-using-extend-kalman-filter-ekf-for-robotics-c94792f3bdb8?source=collection_archive---------6----------------------->

KF 被设计成使卡尔曼滤波器能够应用于非线性运动系统，例如机器人。EKF 产生了比仅仅使用实际测量更精确的状态估计。在本帖中，我们将简要介绍扩展卡尔曼滤波器，并了解传感器融合的工作原理。为了讨论 EKF，我们将考虑一辆机器人汽车(在这种情况下是自动驾驶汽车)。我们可以如下图所示在一个全局坐标系中对这辆车建模，坐标为:X *全局*，Y *全局*，Z *全局*(面…