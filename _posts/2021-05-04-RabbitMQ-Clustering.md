---
layout: post
title: RabbitMQ Cluster
date: 2021-05-04 18:30:00 +0900
category: tech
---

# RabbitMQ Cluster

- Cluster는 고가용성(고장대비)를 위해 구성한다.

- Cluster 요구 사항

  - Cluster를 구성할 서버들끼리 `hostname` 으로 서로를 찾을 수 있어야한다.
  - `hostname` 은  DNS Records나 `/etc/hosts` 파일을 이용하면 된다.
  - 포트
    - 4369: 노드를 찾는 검색 데몬
    - 25672: inter-node및 CLI 도구 통신 포트 (AMQP포트 + 20000)
    - 5672, 5671: AMQP
    - 15672: 관리 GUI 툴
    - 35672-35682: used by CLI tools (Erlang distribution client ports) for communication with nodes and is allocated from a dynamic range (computed as server distribution port + 10000 through server distribution port + 10010). See [networking guide](https://www.rabbitmq.com/networking.html) for details.
    - 61613, 61614: [STOMP clients](https://stomp.github.io/stomp-specification-1.2.html) without and with TLS (only if the [STOMP plugin](https://www.rabbitmq.com/stomp.html) is enabled)
    - 1883, 8883: [MQTT clients](http://mqtt.org/) without and with TLS, if the [MQTT plugin](https://www.rabbitmq.com/mqtt.html) is enabled
    - 15674: STOMP-over-WebSockets clients (only if the [Web STOMP plugin](https://www.rabbitmq.com/web-stomp.html) is enabled)
    - 15675: MQTT-over-WebSockets clients (only if the [Web MQTT plugin](https://www.rabbitmq.com/web-mqtt.html) is enabled)
    - 15692: Prometheus metrics (only if the [Prometheus plugin](https://www.rabbitmq.com/prometheus.html) is enabled)

- Node in a Cluster

  - RabbitMQ가 운영되는데 필요한 모든 데이터와 상태는 Cluster안에 모든 노드에 복제된다.
  - MQ자체는 복사되지 않는다.
  - 어떤 노드로 접근하든지 다른 노드의 MQ를 읽을 수 있다.
  - 모든 노드의 MQ를 복사하려면 미러링 구성을 해야한다.

- Erlang 쿠키 맞추기

  - 노드끼리의 통신을 위해서 Erlang Cookie를 공유해야 한다(private key 역할)
  - `.erlang.cookie` 파일을 모든 노드에서 맞춰주면 된다.
  - `.erlang.cookie` 의 파일 위치는 `/var/lib/rabbitmq/ (/usr/local/var/lib/rabbitmq)` 또는 `home` 이다.
  - 하나의 파일로 똑같이 통일 해주면된다. (또는 복사 붙여넣기!)
  - RabbitMQ 재시동

- Cluster 구성하기

  - 각각 모든 노드에서 RabbitMQ시작

    ```shell
    # rabbitA
    rabbitmq-server -detached
    # rabbitB
    rabbitmq-server -detached
    # rabbitC
    rabbitmq-server -detached
    ```

  - cluster_status 명령어로 확인하면 각각 노드가 독립적으로 생성된걸 확인 가능

    ```shell
    # rabbit A, B, C
    rabbitmqctl cluster_status
    
    # => Cluster status of node rabbit@rabbit1 ...
    # => [{nodes,[{disc,[rabbit@rabbitA]}]},{running_nodes,[rabbit@rabbitA]}]
    # => ...done.
    ```

  - rabbitB와 rabbitC에서 rabbitA에 연결되는 클러스터 구성

    ```shell
    # rabbitB
    rabbitmqctl stop_app
    # => Stopping node rabbit@rabbitB ...done.
    
    rabbitmqctl reset
    # => Resetting node rabbit@rabbitB ...
    
    rabbitmqctl join_cluster rabbit@rabbitA
    # => Clustering node rabbit@rabbitB with [rabbit@rabbitA] ...done.
    
    rabbitmqctl start_app
    # => Starting node rabbit@rabbitB ...done.
    
    ```

    ```shell
    # rabbitC
    rabbitmqctl stop_app
    # => Stopping node rabbit@rabbitC ...done.
    
    rabbitmqctl reset
    # => Resetting node rabbit@rabbitC ...
    
    rabbitmqctl join_cluster rabbit@rabbitA
    # => Clustering node rabbit@rabbitC with [rabbit@rabbitA] ...done.
    
    rabbitmqctl start_app
    # => Starting node rabbit@rabbitC ...done.
    
    ```

    

  - 이후 각각 노드에서 cluster_status 명령어로 확인하면 모든 노드가 연결되어 있는 것을 확인 가능

- Cluster 노드 타입

  - RAM 타입과 Disk 타입을 결정 할 수 있음

  - RabbitMQ는 Cluster 내 모든 노드가 RAM타입으로 구성되는 것을 금지한다. (1개 이상 Disk타입을 포함 해야함)

  - Cluster에 join할 때, 노드 타입 지정

    ```
    # on rabbitB
    rabbitmqctl stop_app
    # => Stopping node rabbit@rabbitB ...done.
    
    rabbitmqctl join_cluster --ram rabbit@rabbitA
    # => Clustering node rabbit@rabbitB with [rabbit@rabbitA] ...done.
    
    rabbitmqctl start_app
    # => Starting node rabbit@rabbitB ...done.
    ```

  - 타입을 변경 할 때

    ```
    # on rabbitB
    rabbitmqctl stop_app
    # => Stopping node rabbit@rabbitB ...done.
    
    rabbitmqctl change_cluster_node_type disc
    # => Turning rabbit@rabbitB into a disc node ...done.
    
    rabbitmqctl start_app
    # => Starting node rabbit@rabbitB ...done.
    
    # on rabbitA
    rabbitmqctl stop_app
    # => Stopping node rabbit@rabbitB ...done.
    
    rabbitmqctl change_cluster_node_type ram
    # => Turning rabbit@rabbitA into a ram node ...done.
    
    rabbitmqctl start_app
    # => Starting node rabbit@rabbitA ...done.
    ```

- Mirroring 구성

  - MQ의 Exchage와 Binding은 모든 노드가 공유하지만 Queue 내부의 데이터는 공유 되지 않기 때문에, 해당 노드가 다운되면 그 노드안에 데이터는 유실된다.

  - 때문에 고가용성을 위해 Mirroring 구성을 해서 모든 노드에 서로가 복제되어 저장될 수 있도록 할 수 있다.

    모든 요청은 리더노드가 우선적으로 처리하고, 미러 노드로 전파된다.

  - 미러링은 policy를 이용해서 설정 가능

  - [reference example](https://www.rabbitmq.com/ha.html#examples)



##### 참고자료

- [RabbitMQ reference Clustering Guide](https://www.rabbitmq.com/clustering.html)
- [https://jonnung.dev/rabbitmq/2019/08/08/rabbitmq-cluster/](https://jonnung.dev/rabbitmq/2019/08/08/rabbitmq-cluster/)

