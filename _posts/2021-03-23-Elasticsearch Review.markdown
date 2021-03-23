---

layout: post

title: "Elasticsearch Review"

date:  2021-03-23 10:00:00 +0900

categories: elasticsearch

---



```markdown
* Elasticsearch 를 스터디 및 운영하면서 정리한 내용 모음집
```



# 인덱스당 적정한 샤드와 레플리카 수

* 클러스터내 노드 사이에 이동하는 가장 작은 단위의 파일
* 샤드의 개수는 인덱스가 아닌 노드와 밀접한 관련 
* (김종민 elastic avangelist)경험상 1개 노드에 700개 이상의 샤드가 쌓이지 않게 하는 것이 중요 
* 그 이상이 되는 경우 Disk I/O, Network I/O로 인한 병목 현상 심각



# 인덱스당 적절한 사이즈

* 위와 같은 이유로 인덱스당 사이즈가 아닌 샤드당 사이즈가 중요
* 샤드의 1개 사이즈는 20~30GB 권고 
* 검색 위주의 클러스터면 20GB에 가까울수록 좋고, 단순 로깅 목적이라면 50GB 까지도 고려가능



# 오래된 인덱스를 관리할 방법?

* Timeseries data라면 rollover index 를 사용이 편리
* 조금 더 깊은 레벨로 관리하고 싶다면 Curator(인덱스 관리 도구) 사용 검토필요 
  * 기능 리스트 : https://www.elastic.co/guide/en/elasticsearch/client/curator/5.4/about.html
  * 깃허브 링크 : https://github.com/elastic/curator
* Rollup API
  * 오래된 인덱스의 필요한 통계치만 저장하여 저장 공간을 쉽게 줄여주는 데이터 관리 방법중 하나
  * 데이터를 미리 aggregation 하여 통계 데이터만 보는 방법
  * query&aggregaion을 미리 등록해두고 scedule을 설정하는 것으로 사용 가능
  * 참고 페이지 :  https://www.elastic.co/guide/en/elasticsearch/reference/master/rollup-job-config.html



# 검색 속도 튜닝

* 참고페이지: https://www.elastic.co/guide/en/elasticsearch/reference/master/tune-for-search-speed.html
* 파일 시스템 캐시에 메모리 부여
* SSD, 빠른 CPU 사용
* Document Modeling
  * 가능한 가벼운 검색이 될수있도록 모델링
  * nested, parent-child 구조는 검색이 느려지게 함
* 가능한 한 적은 수의 필드 검색
  * 'query_string' or 'multi_match' 쿼리에서 대상 필드가 많을 경우 느려짐
  * 'copy_to' 를 활용하면 쿼리 대상 필드를 줄여서 쿼리할 수 있음
* 숫자 필드의 경우 keyword 보다는 integer 또는 long 으로 매핑
* [Adaptive Replica Selection](https://www.elastic.co/guide/en/elasticsearch/reference/current/tune-for-search-speed.html#_turn_on_adaptive_replica_selection)



# 인덱싱 성능 고려사항

* 참고페이지: https://www.elastic.co/blog/performance-considerations-elasticsearch-indexing

  

# index를 close 할 때의 이점

* 참고페이지: https://www.elastic.co/guide/en/elasticsearch/reference/6.3/indices-open-close.html
* close는 말 그대로 메모리에 안 띄워두고 파일을 닫아둠
* 저장공간 빼곤 일체의 리소스를 차지 안함
* ES 샤드 형태로 오래도록 저장해두고, 검색이 필요없는 경우 close 적합
* 인덱스가 필요해지면 open해서 사용가능
* elasticsearch 6.6부터 frozen indices 기능 추가
  * 기본적으로 close indices와 같음
  * 검색요청이 들어오면 open 하여 검색하고 자동으로 다시 close 시킴