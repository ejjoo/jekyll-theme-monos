---
description: >-
  There are many versions of ROS(kinetic, indigo, ...) but we will use Melodic
  to run in Ubuntu 18.04
---

# ROS : robot operating system

## **Install ROS\_Melodic**

```text
will be edited later :D 
```

## ROS Topic

Topic is a channel through which nodes can communicate. Some communication occurs between the two programs, and some messages are exchanged. This path is called '**topic**'.

If you do not understand from the explanation above, look at an example of using a topic in ROS and find out the role of the topic in the example.

### ROS Topic tutorial

After installing ROS, enter **roscore** to run it.

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

Now let's see what the topic played in the process of moving the turtle above.

turtlesim\_node and __turtle\_teleop\_key node communicates with ROS Topic.

turtle\_teleop_\__key node publishes the input of key on a topic, turtlesim node subscribes to the same topic to receive the input of key.

We can intuitively check **nodes** and **topics** through rqt\_graph.

#### Using rqt\_graph

rqt\_graph shows topics and nodes running in ros.

Enter the following command to run rqt\_graph

```text
$ sudo apt-get install ros-melodic-rqt
$ sudo apt-get install ros-melodic-rqt-common-plugins
```

Enter following command in a new terminal.

```text
$ rosrun rqt_graph rqt_graph
```

You can see a picture similar to the one below

![Source : http://wiki.ros.org/ko/ROS/Tutorials/UnderstandingTopics?action=AttachFile&amp;do=get&amp;target=rqt\_graph\_turtle\_key.png](.gitbook/assets/image%20%286%29.png)

In rqt_graph above, /teleopturtle and /turtlesim are nodes, and teleop publishes a topic named /turtle1/command_\_velocity. Then /turtlesim node subscribes the topic\(/turtle1/command\_velocity\) to get the input of key.

#### Introducing rostopic

You can get a lot of information about topics through the 'rostopic' command.

```text
$ rostopic -h
```

```text
rostopic bw     display bandwidth used by topic
rostopic echo   print messages to screen
rostopic hz     display publishing rate of topic    
rostopic list   print information about active topics
rostopic pub    publish data to topic
rostopic type   print topic type
```

#### Using rostopic list

rostopic list returns a list of all topics currently subscribed or published.

```text
$ rostopic list -h
```

```text
Usage: rostopic list [/topic]

Options:
  -h, --help            show this help message and exit
  -b BAGFILE, --bag=BAGFILE
                        list topics in .bag file
  -v, --verbose         list full details about each topic
  -p                    list only publishers
  -s                    list only subscribers
```

Command rostopic list can use option 'verbose'\("-v"\)

```text
$ rostopic list -v
```

With that, you can see where the topic is published, where it is subscribed, and the type of topic.

```text
Published topics:
 * /turtle1/color_sensor [turtlesim/Color] 1 publisher
 * /turtle1/cmd_vel [geometry_msgs/Twist] 1 publisher
 * /rosout [rosgraph_msgs/Log] 2 publishers
 * /rosout_agg [rosgraph_msgs/Log] 1 publisher
 * /turtle1/pose [turtlesim/Pose] 1 publisher

Subscribed topics:
 * /turtle1/cmd_vel [geometry_msgs/Twist] 1 subscriber
 * /rosout [rosgraph_msgs/Log] 1 subscriber
```

For more information or ROS Topic tutorials, check out:  [ROS Topic Tutorial](http://wiki.ros.org/ko/ROS/Tutorials/UnderstandingTopics) 

Source of above tutorial : [http://wiki.ros.org/ko/ROS/Tutorials/UnderstandingTopics](http://wiki.ros.org/ko/ROS/Tutorials/UnderstandingTopics)

## ROS Message

Communication on topics happens by transmitting ROS messages between nodes.

For the publisher \(turtle\_teleop\_key\) and subscriber \(turtlesim\_node\) to communicate, the publisher and subscriber must send and receive the same type of message.

This means that a topic type is defined by the message type published on it. The type of the message sent on a topic can be determined using rostopic type.

Data is exchanged between nodes through messages. Messages are in the form of variables such as integers, floating points, and booleans.

![Source : https://cafe.naver.com/openrt/2468 \(CC BY-NC 3.0\)](.gitbook/assets/image%20%285%29.png)

In the picture above, suppose that node 1 is a publish node and node 2 is a subscribe node.

After node1 and node2 are connected to the master node, node1 publishes msg on a topic and delivers information to node2\(subscribe node\)

For detailed information related to the node, such as the communication order between the node and the  master node, see the ROS Node Table of Contents.

### Using rostopic type



## ROS Node

## ROS Service



