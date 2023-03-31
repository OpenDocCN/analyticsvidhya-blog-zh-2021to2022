# Django 中一对一、多对多和多对一关系的区别

> 原文：<https://medium.com/analytics-vidhya/difference-between-one-to-one-many-to-many-and-many-to-one-relationships-in-django-2304b567152f?source=collection_archive---------1----------------------->

# 介绍

当我开始学习 Django 的时候，我在弄清楚上面提到的关系的用例时遇到了一个问题。如果你有这样的问题，请留下来，因为我会向你详细解释，并给出例子。

# 一对一的关系

在这里，两个对象必须彼此具有单一/唯一的关系。以一个**学生**和**学生简介**为例，两个学生不可能有相同的简介。

```
**from** **django.db** **import** models

**class** **Student**(models.Model):
    name = models.CharField(max_length=50)
    student_id = models.CharField(max_length=80)

    **def** __str__(self):
        **return** self.name

**class** **Student_Profile**(models.Model):
    student = models.OneToOneField(
        student,
        on_delete=models.CASCADE,
    ) student_id= models.CharField(max_length=80)

    **def** __str__(self):
        **return** self.student.name
```

# 多对多关系

在这里，一个模型中的 0 个或多个对象可以与另一个模型中的 0 个或多个对象相关，是的，就是这么简单。以医生和病人之间的关系为例，医生可以治疗零个或多个病人。同样，一个病人可以由 0 个或更多的医生治疗。

```
class Doctor(models.Model):
    name = models.CharField(max_length=150)  
    **def** __str__(self):
        **return** self.name

class Patient(models.Model):
    doctors = models.ManyToManyField(Doctor)
    name = models.CharField(max_length=150)
    **def** __str__(self):
        **return** self.name
```

# 多对一关系

在 Django 中，多对一关系被称为外键关系，在这里，一个模型中的 0 到多个对象可能与另一个模型中的一个对象相关。一个项目可以有多个学生参与。在这个例子中，我将比较两个实体**学生**和**项目**之间的关系。几个学生可以被分配一个项目来完成一项任务或其他事情。

```
**class** **Student**(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)

    **def** __str__(self):
        **return** self.first_name

**class** **Project**(models.Model):
    name = models.CharField(max_length=30)
    sudents = models.ForeignKey(Student, on_delete=models.CASCADE) **def** __str__(self):
        **return** self.name
```

# 结论

希望这能以某种方式帮助你，如果你想联系我，你可以在推特上联系我