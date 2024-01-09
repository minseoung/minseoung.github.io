# HTTP 요약 1

## 서두

만약 클라이언트 컴퓨터와 서버 컴퓨터가 바로 옆에 있다면 케이블로 연결해서 데이터를 주고받으면 끝난다.
하지만 한국과 미국에 있다면, 중간에 인터넷 망을 통해서 데이터를 통신해야한다.

## IP

IP 프로토콜은 전달하려는 데이터를 보내는 IP 주소와 받는 IP 주소와 함께 패킷이라는 단위로 감싸버린다. 그리고 보내려는 최종 서버에 도달하기까지 중간서버들을 거쳐서 전달된다. 그럼 받는쪽에서도 보낸 IP 주소를 보고 응답을 보낼수있다.
근데 문제가 있다.
비연결성: 만약 받는쪽(서버)가 데이터를 받을 수 없는 상황(컴퓨터를 꺼버렸네)이라면..? 못 받는다.
패킷소실: 만약 중간에 거치는 서버들 중 갑자기 전달 도중에 문제가 생겨 꺼져버린다면..? 못 받는다.
뒤죽박죽: 만약 데이터가 너무 커서 쪼개져서 보내질때 첫 번째로 간것보다 두 번째로 간게 중간 서버를 빠른걸 잘 타서 먼저 전달된다면..? 데이터가 뒤죽박죽 섞인다. 제대로 못 받는다.


## TCP & UDP

TCP는 프로토콜 계층상 IP 위에 존재하고 보완하는 느낌이다.
먼저 전달할 데이터에 TCP가 클라이언트와 서버의 포트를 가지고 부가정보(전달정보, 순서정보, 검증 등)와 감싼다.
그리고 IP에 의해 또 감싸진다.
여기서 TCP는
3 핸드 쉐이크를 쓴다. 씬-씬액-액 이게 뭐냐면, IP의 비연결성을 보완하는건데, 먼저 보낼 서버에 클라이언트가 연결을 건다. 그리고 서버가 연결되었다고 응답과 연결도 건다. 그럼 클라이언트도 연결되었다고 응답을 보내는것이다.
그리고 데이터를 전달하면 일단 서버가 받을수있다는 전제하에 보낼수있다.
그리고 서버가 데이터를 받으면 응답을 보내서 잘 받았다고 알려준다.
또한 순서 정보도 가지고 있어서 순서가 틀리게 왔다면 틀린부분부터 다시 보내달라고 클라이언트에 요청후 다시 받는다.
그럼 UDP는 뭘까? 단순히 IP에 Port정보도 같이 감싸주는 정도이다. 하지만 UDP는 거의 비어있는것과 같기에 이미 완성형인 TCP의 최적화 버전으로 리메이크 하는 수단이 된다.

## Port

그럼 포트는 뭘까? 우리집 인터넷 IP주소가 있고 이걸 쓰는 기기가 여러대일때 어떤 기기에서 보내는지, 받을지를 IP주소만으로는 알 수가 없다. 포트는 쉽게 말하면 아파트의 호수 같은거다. IP주소가 아파트면, Port는 몇동 몇호다를 알려주는것이다. 
정리하면 같은 IP내에서 프로세스를 구분해준다.

## DNS

DNS는 우리가 URL 창에 검색할 때 사이트의 IP주소를 일일이 외워서 치진 않고, naver.com 처럼 도메인 이름으로 검색을 할 것이다. 이렇게 하면 장점은 위 도메인을 쓰는 회사가 IP주소를 바꿔도 우리는 접근할때 도메인으로 접근만하면 접근이 된다. 약간 전화번호부 처럼 우리는 이름창에 친구1이라는 사람을 검색해서 전화하는것이다. 친구1의 전화번호가 뭔진 모르고

## URI

URI는 리소스를 구분하는 식별자이다. 리소스란 우리가 접근하는 모든 자원을 말하고, 검색하면 나올만한 것들이다. 그렇담 URL, URN은 뭘까? 그냥 큰 범위로 URI가 있고 URL은 리소스의 위치, URN은 리소스의 이름으로 리소스를 접근할수있는것인데, URN은 잘 안되었는지 처음 들어본다. 간단하게 보면 https://naver.com/?as=as 앞에는 스키마, https는 http의 보안강화 버전으로 알면되고 뒤에는 도메인 명, 그 뒤에는 리소스 경로가 오고, 그 뒤에는 쿼리 파라미터(쿼리 스트링)이라고 오는데 저기로 오는건 키 벨류 타입이고 벨류는 모두 String 타입이다.

## HTTP

http는 클라이언트와 서버의 구조로 되어있고, 클라이언트가 요청을 보내면 서버가 응답을 보낸다.
이걸 쓸 때 최대한 무상태로 설계해야하는데, 무상태란 서버가 클라이언트의 정보를 일일이 들고있으면 안된다는것이다. 그렇게 알고 구현을 하면 서버가 바뀌었을때 정보를 아는게 없어서 당혹스럽다. 요청을 보낼때 기억해야되는것까지 계속 한꺼번에 보내게 설계를 한다. 근데 무상태가 필요하지 않을때도 있다. 쿠키나 세션이나 그를 통한 로그인 관리나 등등 그럴때만 무상태를 쓰지 않도록 한다.

이렇게 무상태로 설계하면 좋은점이 서버를 바꾸거나 확장시켜도 문제가 없다. 무한증설 가능

http는 비연결성이다. 계속 연결되있으면 연결 이라는 자원이 서버에서 계속 낭비된다. 그러면 데이터를 주고받고 바로 끊는것이냐? 그것도 아니다. 그렇게 되면 끊자마자 바로 요청이 또오면 다시 3 핸드 쉐이크하고 다시 서버 자원들을 클라이언트에 내려주고 이러면 오래걸린다. 지속 연결로 해결했다. 그냥 연결하고 일정시간 지나면 그때 끊는것이다.