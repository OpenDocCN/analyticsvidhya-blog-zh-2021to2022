# 基于 Azure Face Service 和 OpenCV 的人脸地标分析

> 原文：<https://medium.com/analytics-vidhya/face-landmarks-analysis-with-azure-face-service-and-opencv-fb898ab58929?source=collection_archive---------10----------------------->

一个[微表情](https://en.wikipedia.org/wiki/Microexpression)是只持续很短一瞬间的面部表情。它是一种自发和无意的情绪反应同时发生并相互冲突的内在结果。

最近，我正在做一个有趣的项目，实时捕捉和分析人们的微表情。以前我们可以用[OpenCV](https://docs.opencv.org/3.4/da/d60/tutorial_face_main.html)+[DLib](http://dlib.net/)+[shape _ predictor _ 68 _ face _ landmarks model](https://github.com/davisking/dlib-models#shape_predictor_68_face_landmarksdatbz2)来实现。但是如何在云计算时代用基于云的人工智能模型来实现呢？Azure Face 服务提供人工智能算法…