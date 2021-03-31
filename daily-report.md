# Daily report

' 3.31\(수\) 공부내용 및 보고할것

* ROS란?

**ROS는 ROS 패키지의 ROS msg, ROS service, ROS node등을 통해 로봇을 관리하게 해주는 운영체제이다.**

**ROS의 구성요소로는 msg, node, service 등이있다.**

**ROS node란 정확히는 ROS패키지의 내의 실행파일이다.**

**ROS node는 masternode와 publish, subscribe노드로 이루어지고 master node는 오직 1개이다.**

n**ode간에 msg를 통해 통신하게 되고 msg 통신의 방식은 topic, service, action이 있는데 주로 topic이나 service를 사용한다.**

**topic은 단방향이고 이다**. \(publish node가 메세지를 보내면 subscriber가 받는 방식\)

**service는 양방향이고 동기화방식이다. 서비스 서버와 클라이언트를 통해 통신하며 client의 리퀘스트가 있을때만 server가 응답한다.** 

드론 자율주행 시스템에 우리가 쓸 **하드웨어는 Pixhawk 입니다.** 

**pixhawk가 FC로 사용되며 pixhawk는 PX4, Ardupilot등의 소프트웨어 펌웨어를 지원한다.**

**펌웨어인 PX4나 Ardupilot을 FC에 설치하기 전에 Mission Planner나 QGroundControl등의 소프트웨어를 설치해야한다.** 

**QGC는 Pixhawk에 Firmware를 설치해주는 GCS이다.**

GCS란 지상통제소이다.

**QGroundControl은 MAVLink를 통해 비행체와 통신하며 비행체 설정 및 비행상태등을 표시하는 역할을 한다.**

그럼 mavlink는 뭔가?

  
‌  
mavlink를 알기전에 mavros부터 알아보면서 mavlink를 알아보자.  
  
‌  
MAVROS는 mavlink를 통해 ROS에서 동작하는 node를 만든다.  
  
‌  
Onboard system\(rasp pi\)에 설치되며 fc와  onboard, gcs의 통신을 담당한다.  
  
‌  
MAVROS가 FC와 GCS를 이어주는 다리역할을 하는데 이 다리를 정확히 말하면 MAVROS가 생성한 ROS와 통신이 가능한 node가 그것이다.  
  
그 노드사이의 통신을 MAVLink protocol type인 메세지를 통해 통신한다.

정리하자면 FC\(Pixhawk\) &lt;-mavlink type message-&gt; Onboard\(Rasp pi\) &lt;- mavlink type message -&gt; GCS

실제 드론 운용은 위와같은 system으로 이루어지고, FC가 없을때 시뮬레이팅 하는 방법은 다음 두 가지가 있다.

SITL은 FC없이 드론의 비행을 가상 시뮬레이터인 가제보 안의 가상환경에서 드론을 비행시키며 PX4 소프트웨어만으로 비행 시뮬레이팅하는 방식이다.

HITL이란 FC를 시뮬레이터\(Gazebo\)에 연결하여 시뮬레이션 하는 방식으로  FC에서는 PX4가 작동한다.









FC\(Pixhawk with PX4 installed\) &lt;-&gt; Onboard\(Rapsberry Pi\) &lt;-&gt; GCS\(QGroundControl\) == desektop??









 

## 

