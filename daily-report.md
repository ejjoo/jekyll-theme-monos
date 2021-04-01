# Daily report

## ' 3.31\(수\) 공부 및 보고내

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
**MAVROS는 mavlink를 통해 ROS에서 동작하는 node를 만든다.**  
  
‌  
**Onboard system\(rasp pi\)에 설치되며 fc와  onboard, gcs의 통신을 담당한다.**  
  
‌  
**MAVROS가 FC와 GCS를 이어주는 다리역할을 하는데 이 다리를 정확히 말하면 MAVROS가 생성한 ROS와 통신이 가능한 node가 그것이다.**  
  
**그 노드사이의 통신을 MAVLink protocol type인 메세지를 통해 통신한다.**

정리하자면 FC\(Pixhawk\) &lt;-mavlink type message-&gt; Onboard\(Rasp pi\) &lt;- mavlink type message -&gt; GCS

실제 드론 운용은 위와같은 system으로 이루어지고, FC가 없을때 시뮬레이팅 하는 방법은 **다음 두 가지가 있다. \(매번 드론을 날릴수 없으니 시뮬레이팅을하는데 \)**

**SITL은 FC없이 드론의 비행을 가상 시뮬레이터인 가제보 안의 가상환경에서 드론을 비행시키며 PX4 소프트웨어만으로 비행 시뮬레이팅하는 방식이다.**

**SITL하면 컴퓨터만으로 시뮬레이션**

**HITL이란 FC를 시뮬레이터\(Gazebo\)에 연결하여 시뮬레이션 하는 방식으로  FC에서는 PX4가 작동한다.**

**HITL은 FC\(Pixhawk\)를 컴퓨터에 연결해서 시뮬레이션**







FC\(Pixhawk with PX4 installed\) &lt;-&gt; Onboard\(Rapsberry Pi\) &lt;-&gt; GCS\(QGroundControl\) == desektop??



## ' 4.1\(목\) 공부 및 보고내용

* 오늘 공부할거 : MAVLINk 안에서 드론의 비행정보들, 그리고 비행을 제어하기위한 신호 protocol들 어떻게 통신되는

MAVLink에는 두가지의 메세지 통신모드가 있는데 Topic과 Point to Point이다. Topic은 메세지의 타겟 시스템과 component ID를 보내지 않고 여러 Subscriber가 이 메세지를 받을 수 있다. PTP모드에서는 타겟 ID와 타겟 타겟 컴퍼넌트를 사용한다. 대부분의 경우 위 필드\(타겟 ID, 타겟 컴퍼넌트\)를 사용하고 이 방식은 메세지의 전송을 보장한다.

Topic모드를 제외하고 각 MAvlink메세지는 System ID와 Component ID필드를 포함한다. 추가로 SET\_POSITION\_TARGET\_GLOBAL\_INT를 포함한 몇몇 메세지는 target\_system과 target\_component 필드를 포함하는데 어떤 system이나 component가 명령어를 실행해야 하는지 알려준다.

비행제어를 위한 명령어로는 SET\_POSITION\_TARGET\_LOCAL\_NED, SET\_POSITION\_TARGET\_LOCAL\_NED, SET\_POSITION\_GLOBAL\_INT가있다. MAVLink의 명령어이고 이 명령어를 기체가 받는다. 보통 이 명령어는 GCS나 Compuer에서 발신된다.





* + 원래 하던공부 복습, 마브링크 \(피치 롤 요\)

피치 : 드론의 X축으로 드론의 전후진을 담당하는 축이다.. 피치축을 기준으로 앞으로 기울어지면 전진을하게되고 뒤로 기울어지면 후진하게된다  

롤 : 드론의 Y축으로 드론의 우진, 좌진을 담당하는 축이다.. 롤축을 기준으로 왼쪽으로 기울어지며 좌진, 오른쪽으로 기울어지면 우진하게된다. 

요 : 드론의 Z축으로 드론의 회전을 담당하는 축이다. 왼쪽으로 회전하려면 오른쪽에 있는 모터의 출력을 높이고 오른쪽으로 회전하려면 왼쪽에있는 모터의 출력을 높이면 된다.  

드 론 조종기는 2가지 모드가있는데 과거에는 모드1을 더 많이썼다고 한다.\(일본식\)

 모드1 : throttle이 오른쪽 모드2 : throttle이 왼쪽

모드2기준으로 설명하겠습니다.

throttle는 왼쪽 스틱으로 조절하며 을 앞뒤로 밀어 드론을 상승시키거나 하강시킨다 

yaw또한 왼쪽스틱으로 조절하며 왼쪽으로 스틱을밀면 반시계회전, 오른쪽으로 밀면 시계방향 회전을한다.

pitch는 오른쪽 스틱으로 조절하며 오른쪽 스틱을 앞으로밀면 전진하고 뒤로밀면 후진한다. 

roll또한 오른쪽 스틱으로 조절하며 오른쪽 스틱을 왼쪽으로밀면 좌진하고 오른쪽으로 밀면 우진한다.











 

## 

