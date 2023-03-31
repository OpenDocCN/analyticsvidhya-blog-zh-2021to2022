# 超参数自动调谐(带数据块的 Keras 调谐器)

> 原文：<https://medium.com/analytics-vidhya/hyper-parameter-auto-tuning-keras-tuner-with-databricks-2a1c12af6aca?source=collection_archive---------6----------------------->

最近我参加了胸部 X 射线(CXRs) 的[肺炎分类挑战。本轮机器学习挑战赛由](https://dphi.tech/challenges/pneumonia-classification-challenge-by-segmind/76/overview/about) [DPhi 社区](https://dphi.tech) & [Segmind](https://segmind.com) 组织。你可能知道，“肺炎在 2017 年导致超过 808，000 名 5 岁以下儿童死亡，占 5 岁以下儿童死亡总数的 15%。有患肺炎风险的人还包括 65 岁以上的成年人和已有健康问题的人。- [世卫组织](https://www.who.int/news-room/fact-sheets/detail/pneumonia)

虽然流行，准确诊断 CXR 肺炎是困难的。需要放射科专家…