# eye attend——从头开始的基于面部识别的考勤系统。—完整的方法。(第一部分)

> 原文：<https://medium.com/analytics-vidhya/eyeattend-facial-recognition-based-attendance-system-from-scratch-16e3ece22089?source=collection_archive---------13----------------------->

—从数据收集到模型编译到后端数据存储再到前端 Android 应用程序

如果没有我的团队成员阿比吉特·巴鲁阿和 T2·阿鲁西·索德的努力工作，这个项目是不完整的。我们非常感谢我们的教授 Varun Gupta 博士(应用科学系主任),他给予了我们宝贵的指导和激励，让我们继续这个项目并不断成长。

# 在我们开始之前…

我们想澄清的是，这不会是一个单一的故事，而是一个故事的集合，因为该项目在开发中花费了很长时间，通过这些故事，我们的主要目的是澄清开发的每个部分，以便您能够为自己构建类似的，事实上更高级的版本。

由于系列功能已从 Medium 中弃用，完整的故事将被分成几个部分，并在标题中提到编号，以跟踪帖子。

# 动机

该项目是我们最后一年的迷你项目，我们已经选择了人工智能领域。深入挖掘人工智能是机器学习的领域，我们已经选择了我们的基础是深度学习(ML 的子集)。

我们记得在大学的第一年，我们觉得需要一些鼓舞人心的东西，一些让我们惊讶的东西，因此这种想法成为了我们项目的动力。遵循这个动机，我们决定通过这个项目，在 CSE 学习了四年的工程学士学位后，回馈我们当时的想法。

我们对这个项目的愿景是引入一个智能系统来激励大一学生学习机器。我们的目标是在我们的学院部署这个系统，这样，一旦大一学生观察到它在自己生活中的应用，这肯定会点燃一些学生的热情，以建立一个更好的版本，更好的项目，可以用于社会生态平台。

这个目标很简单，因为每个班级都有自己的班级代表，主要职责之一就是记录学生的出勤情况。通过这种方式，我们可以与目标受众，即大学新生直接互动。

# 本系列的收获

1.  一个可扩展的设计，重点是使考勤系统不仅自动化，而且智能化。
2.  该系统的架构主要关注其可用性和可扩展性，这将有助于我们根据未来的反馈重新设计它。
3.  使用模块化方法，ML 模型可以插入/拔出。
4.  使用机器学习和矢量化来获得更快的结果。
5.  Android 应用程序——“eye attend”，用于与系统交互。

# 你将学习建造什么

下面给出的视频，将展示该项目的核心工作。最重要的一步是选择一个包含房间中所有人的图像，将其发送到服务器，服务器将在图像上运行模型，并返回出席者和缺席者学生的列表。该项目在数据库中动态加载特定课程的特定科目的特定教师的考勤表，即创建表格，添加点名号码，添加出勤日期，还标记出勤。老师可以下载整个班级的出勤情况，学生只需点击一下鼠标，就可以下载特定老师的特定科目的整个学期的出勤情况。下面的演示展示了项目的核心工作。

 [## EyeAttendDemo.mp4

### 观看演示视频

drive.google.com](https://drive.google.com/file/d/1BO7evOKZDRKVQ1dijzrxLdv12u3DMaIA/view?usp=sharing) 

# 结论

是时候结束这个项目的介绍性故事了。我们将努力每周提供一次故事的下一部分。我们希望你会喜欢上面的演示和关于我们项目的信息，你真的很高兴学习如何自己建立自动考勤系统。让我们在下一篇博客中叙旧。在那之前，祝你一周愉快！欲知下一部的最新消息，请在 LinkedIn 上关注我们: [Keshav Tangri](https://www.linkedin.com/in/keshav-t-7ab782104/) ， [Abhijeet Baruah](https://www.linkedin.com/in/abhijeet-baruah-bb1b08165/) ，&Aarushi Sood。

![](img/138a8b35f5210c8cf7c2edd7f386fd70.png)

[Barry Zhou](https://unsplash.com/@sparkerz?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/classroom?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

# 参考

**【1】Logo 设计**:[https://www . designfreelogoonline . com/logoshop/free-Logo-maker-CCTV-camera-Logo-Design-copy/](https://www.designfreelogoonline.com/logoshop/free-logo-maker-cctv-camera-logo-design-copy/)