# Day 7

## 今天做的事

1. 先梳理一下通过昨天晚上的学习建立起的层次三的思路
   1. 数据库中增加标记，标记管理员（学委/老师），标记同班同学
      1. users表中新增两列，一列是成员班级，一列是是否为管理员
      2. 新增表homework，用于存放所有作业的地址以及相应的编号
   2. 设计修改身份的语句
      1. 修改身份为班级管理员
      2. 修改身份为班级成员
   3. 根据不同身份开启不同的回应，比如管理员可以回复“提醒同学”来给全班同学发消息
   4. 对于提醒同学这一项可以通过查询users表中班级来获取结果，然后根据结果进行通知
   5. 对于作业提交以及下载，规定是通过图片格式，目前已知是图片会在coolq中存放至data的一个位置，但是我一直没找到对应的地方，难搞
   6. 如果能找到对应的地方，在两个容器之间的转移也是个麻烦事，目前想法是，获取保存位置之后，从bot容器访问coolq容器，然后把文件下载到bot容器指定位置并重新命名
   7. 还有一个知识上的漏洞就是，不知道如何提供打包下载的服务，细分可以分为
      1. 如何对指定位置多个文件通过命令行进行打包？
      2. 如何提供文件下载服务？
   8. 对于同学提交作业，可以设置一个模式：
      1. 发送“更新班级 id”，可以将已有班级更新为id，默认班级为空
      2. 若班级为空执行以下操作，则无法进行，返回“您还未加入班级，请加入后交作业”
      3. 发送“提交作业”，从下一条开始的内容被检测是否含有图片，若有，加入作业文件夹，加入过程同ddl提醒，给同学发送一个该作业的id
      4. 直到同学发送一条“结束提交”，回复到正常复读机状态
      5. 当同学想要查看之前提交过的作业时，发送“查看提交”，返回带有编号的图片
      6. 当同学想要删除的时候，发送一条“删除作业 id”，此时删除作业编号为id的作业
   9. 对于管理员提醒同学交作业：
      1. 管理员发送“查看班级”，返回列出的班级id
      2. 发送“提醒作业 id”，后台列出id对应下的班级成员，逐一发送“管理员催您写作业”，发送完成后，给管理员返回“已提醒”
      3. 发送“收作业 id”，对id班级对应下的同学，将这些同学的文件夹打包进一个压缩文集那，然后清空同学文件夹，清空同学提交记录，返回文件地址

## 今天学到的东西

今天沉迷家务以及学校事务，基本上没学习

上午学院开会两个半小时，下午党课两个小时，写感想半个小时，晚上补作业一晚上

## 今天未解决的问题

1. 能否将层次二中，提前提醒的两个参数设定默认值，即当给出命令为“add,task,year-month-day hour-minute-second”时，默认为提前24小时，每隔30分钟提醒一次？
2. 能否将层次二中，提前提醒时间与提醒间隔的数据格式放宽，即可以提供提前半小时提醒的服务，初步想法是都转化成秒，当出现非整数秒的情况就取整，差个半秒问题不大
3. 如何对指定位置多个文件通过命令行进行打包？
4. 如何提供文件下载服务？