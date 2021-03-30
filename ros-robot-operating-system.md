---
description: >-
  There are many versions of ROS(kinetic, indigo, ...) but we will use Melodic
  to run in Ubuntu 18.04
---

# ROS : robot operating system

## **Install ROS\_Melodic**

```text
will be edited later
```

## ROS Topic

Topic is a channel through which nodes can communicate. Some communication occurs between the two programs, and some messages are exchanged. This path is called '**topic**'.

If you do not understand from the explanation above, look at an example of using a topic in ROS and find out the role of the topic in the example.

### ROS Topic tutorial

After installing Roth, enter **roscore** to run it.

```text
$ roscore
```

After roscore works fine, run **turtlesim**. \(Please run in a new terminal\)

```text
$ rosrun turtlesim turtlesim_node
```



Execute **teleop**, a node that will receive key input corresponding to turtlesim's direction control.\(in a new terminal\)

```text
$ rosrun turtlesim turtle_teleop_key
```

 

![Source : http://wiki.ros.org/ko/ROS/Tutorials/UnderstandingTopics?action=AttachFile&amp;do=get&amp;target=turtle\_key.png](.gitbook/assets/image%20%281%29.png)

If you run teleop and operate it with the keyboard, the turtle summoned by the turtlesim node will move as shown in the picture above.

자 이제 위 거북이가 움직이는 과정에서 topic이 무슨 역할을 하였는지 알아봅시다.

turtlesim\_node and __turtle\_teleop\_key node communicates with ROS Topic.

turtle_teleop_k 







For more information and ROS Topic tutorials, check out:  [ROS Topic Tutorial](http://wiki.ros.org/ko/ROS/Tutorials/UnderstandingTopics)

Source of above tutorial : [http://wiki.ros.org/ko/ROS/Tutorials/UnderstandingTopics](http://wiki.ros.org/ko/ROS/Tutorials/UnderstandingTopics)

## ROS Node

## ROS Service

## ROS Message



