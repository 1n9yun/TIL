# Proxy & LoadBalancer

## Proxy

* 남을 대신해서 처리

* Network Proxy...

* ### Forward Proxy

  * 일반적으로 말하는 Proxy

  * Client - Proxy - Internet - Server

  * #### 캐싱

    * 클라이언트가 요청한 내용 캐싱
    * 외부 요청이 감소 -> 네트워크 병목 현상 방지

  * #### 익명성

    * 클라이언트가 보낸 요청 감춤
    * 프록시 서버가 보낸 것 처럼 해서 원래 누가 보냈는지 알지 못하게 함

* ### Reverse Proxy

  * Client - Internet - Proxy - Server

  * #### 캐싱

  * #### 보안

    * 서버 정보를 클라이언트로부터 숨김
    * Client는 Reverse Proxy를 실제 서버라고 생각하여 요청
    * 실제 서버의 IP가 노출되지 않음

  * #### Load Balancing - 부하분산

## Proxy Server

* 클라이언트와 서버 간의 중계 서버
* 통신을 대리 수행하는 서버
* 캐시, 보안, 트래픽 분산 등 여러 장점



## Load Balancer

* ### 배경

  * 부하가 늘어남에 따라 하드웨어 스케일 업을 해왔음
  * 부하가 더 늘어나다가 하드웨어 스케일 업의 한계에 마주침
  * 스케일 아웃하여 여러 대의 서버를 이용해 부하를 나누기로 함

* ### 종류

  * L2, L3, L4, L7 - OSI 7 Layer를 기준으로 어떤 것을 나누는지에 따라 다름
    * L2 - MAC을 기준으로 나눔
    * L3 - IP를 기준으로 나눔
    * L4 - Transport Layer에서 Load Balancing (TCP/UDP)
    * L7 - Application Level
      * URL 기준 등 어플리케이션이 어떻게 나누냐에 따라 분배