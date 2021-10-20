---
draft: false
slides: 
url_pdf: ""
title: Database system project
subtitle: Polar-DB based Educational Administration System
date: 2021-09-19T15:16:37.591Z
summary: Polar-DB based Educational Administration System
authors:
  - Le Zhang
url_video: ""
featured: false
external_link: 
url_slides: ""
tags: 
  - Other
categories:
  - Other
links: []
image:
  caption: Database System
  focal_point: Center
  filename: ""
url_code: "https://github.com/Magiccircuit/-Polar-DB"
---


This is a project about designing a educational administration system is capable of managing teaching fairs in real university, up to four entity including teachers, students, courses, scores are designed. Also, it has got different interface for different account (e.g teacher or student), and different functions are accessible to different authorities. 

#### Main functions include  :

* Login & Multi option card interface 
* Information of students and teachers
* Modifying information
* Data analysis
* Abundant of data integrity constraints

#### System overview :

![image-20211019233328955](https://i.loli.net/2021/10/19/VxmPJwFT4hiZ9CH.png)

The system is programmed in python and GUI is designed with PyQt5. This system is linked to PolarDB and provide SQL services with simple click operations in GUI. Connections settings are in main.py , via code the program can connect to polarDB service:

```python
connection = pymysql.connect(host='pc-bp18rn0tqu85a1600-public.rwlb.rds.aliyuncs.com',
                             port=3306,
                             user='lab_1506359621',
                             passwd='19152398ed17_#@Aa',
                             db='final_pj')

```

![image-20211019233602194](https://i.loli.net/2021/10/19/6A251gHFlkGPJR8.png)



#### GUI (Chinese)

I designed a GUI for better interaction and operation, main components are`login`, `stastics`, `switch-student`, `switch-teacher`, `switch-course`

1. **Login UI** 

   If the user and password are blank, you will log in student account, and you will be restricted to only several functions; if you login with predefined account, you will have full authority to create or modify all information.

   <img src="https://i.loli.net/2021/10/19/ApdgEPbxZeXSYf5.png" alt="image-20211019234026824" style="zoom: 67%;" />

2. **Multi-pages**

   - Teacher page

     ![image-20211019234711997](https://i.loli.net/2021/10/19/zLFQdTYt2kn5RqJ.png)

   - Student page (restricted authority)

     ![image-20211019234510813](https://i.loli.net/2021/10/19/wjnOsd4roRKBa9F.png)

3. **Functions**

   - **Inquiry**

     By select the list, you can inquiry students info, course info, teacher info and scores.

     <img src="https://i.loli.net/2021/10/19/I1BftrCxZgovN7u.png" alt="image-20211019235108056" style="zoom: 67%;" />

     If you login in with teacher account, you can inquiry all info of students

     <img src="https://i.loli.net/2021/10/19/ucAXJhglo6HQYSa.png" alt="image-20211019235149419" style="zoom: 67%;" />

     By inputing the teacher's ID, you can inquiry their courses

     <img src="https://i.loli.net/2021/10/19/QrwnFYxmeUgGjtl.png" alt="image-20211019235207630" style="zoom:67%;" />

     In course selection interface, all courses will be displayed 

     <img src="https://i.loli.net/2021/10/20/BEPuFH39b1f6Crs.png" alt="image-20211020093210793" style="zoom:50%;" />

   - **Delete**

     By inputing the correct ID, you can delete student info.

     <img src="https://i.loli.net/2021/10/20/SXP7q8zEs4wNLom.png" alt="image-20211020093331890" style="zoom:33%;" />

     If you want to quit an course, you just input the course ID, then it will delete Sid tuple and Cid tuple in the SC table

   - **Insert**

     If you want to add new student, you enter the ID, and select sex, if the ID already exists in the DB, it will fail.

     <img src="https://i.loli.net/2021/10/20/ajmB9cpV12oxSNC.png" alt="image-20211020093537567" style="zoom:33%;" />

     <img src="https://i.loli.net/2021/10/20/o8i4rFbvNLWRlPp.png" alt="image-20211020093621226" style="zoom:33%;" />

   - **Modify**

     We can edit teachers' title by entering ID and choose  new title for teachers.

     <img src="C:/Users/zhangle/AppData/Roaming/Typora/typora-user-images/image-20211020093743743.png" alt="image-20211020093743743" style="zoom:33%;" />

   - **Statics**

     Entering the  ID of the teacher, you can view the average scores static of the courses taught by this employee

     <img src="C:/Users/zhangle/AppData/Roaming/Typora/typora-user-images/image-20211020094023844.png" alt="image-20211020094023844" style="zoom:33%;" />

     Besides, in the course page, you can also check the statics of all courses

     <img src="C:/Users/zhangle/AppData/Roaming/Typora/typora-user-images/image-20211020094113534.png" alt="image-20211020094113534" style="zoom:33%;" />