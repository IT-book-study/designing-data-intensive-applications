# 04장 부호화와 발전

데이터 타입이나 스키마가 변경될 때 애플리케이션 코드에 대한 변경이 종종 발생하는데, 대규모 애플리케이션에서 코드 변경은 대개 즉시 반영할 수 없다.  

- BE app은 한 번에 몇 개의 노드에 새 버전을 배포하고 원할하게 실행되면, 단계적으로 모든 노드에 실행되게 하는 rolling upgrade 방식이 있다.

- Client app은 사용자에 전적으로 좌우된다.

즉 예전 버전의 코드와 새로운 버전의 코드, 이전 데이터 타입과 새로운 데이터 타입이 어쩌면 모든 시스템에 공존할 수 있다는 뜻이다.  
시스템이 계속 원할하게 실행되려면 양방향으로 호환성을 유지해야 한다.

## 데이터 부호화 형식

데이터는 메모리에 object, struct, list, array, hash table, tree 등으로 유지된다.  
이런 데이터는 CPU에서 효율적으로 접근하고 조작할 수 있게 최적화 된다.

데이터를 파일에 쓰거나 네트워크를 통해 전송하려면 일련의 바이트열(ex. JSON)의 형태로 부호화 해야 한다.

인메모리 표현에서 바이트열로의 전환을 부호화(직렬화, 마샬링)라고 하며, 그 반대를 복호화(파싱, 역직렬화, 언마샬링)라고 한다.  

## 언어별 형식

프로그래밍 언어에 내장된 부호화 라이브러리는 최소한의 추가 코드로 인메모리 객체를 저장하고 복원할 수 있기 때문에 매우 편리하지만 문제점도 많다.

- 다른 언어에서 데이터를 읽기 매우 어렵다.
- 동일한 객체 유형의 데이터를 복원하려면 복호화 과정이 임의 클래스로 인스턴스화할 수 있어야 하고, 보안 분제의 원인이 된다.
- 효율성 문제. 예를 들면 자바 내장 직렬화.

## JSON, XML, 이진 변형

텍스트 형식이기 때문에 어느 정도 사람이 읽을 수 있다.  
- 수의 부호화 문제. XML과 CSV에서는 수와 숫자로 구성된 문자열을 구분할 수 없다.
- JSON과 XML은 유니코드는 지원하지만 이진 문자열은 지원하지 않는다. 이 문제를 해결하기 위해 Base64로 부호화하지만 데이터 크기가 33% 증가된다.
- 등등 여러 문제가 있다.


## 이진 부호화

조직 내에서만 사용하는 데이터라면 최소공통분모 부호화 형식(lowest-common-denominator encoding format)을 사용해야 하는 부담감이 덜하다.  

메시지팩은 JSON용 이진 부호화 형식이다.  

이게  
![image](https://github.com/IT-book-study/designing-data-intensive-applications/assets/81370558/a297a419-0dea-4520-9878-756c4c6140b9)

이렇게 변환 됨  
![image](https://github.com/IT-book-study/designing-data-intensive-applications/assets/81370558/740694d6-68c6-47fa-bde2-6a9e4760819c)

상기 예시에서 이진 부호화는 길이가 66바이트, JSON 부호화는 81바이트이다. 이 만큼 적은 공간 절약과 가독성을 해칠 가치가 있는지 생각이 필요해 보인다.


## 스리프트와 프로토콜 버퍼

같은 원리를 기반으로 한 이진 부호화 라이브러리이다.  

스리프트  
![image](https://github.com/IT-book-study/designing-data-intensive-applications/assets/81370558/ef4be2ba-972d-4ebd-8885-28bf339c214d)

프로토콜 버퍼  
![image](https://github.com/IT-book-study/designing-data-intensive-applications/assets/81370558/dd10ffa7-e403-4bb6-8ec7-789c2a78e081)

스리프트는 BinaryProtocol과 CompactProtocol이라는 두 가지 다른 이진 부호화 형식이 있다.  

![image](https://github.com/IT-book-study/designing-data-intensive-applications/assets/81370558/e3f041fd-3fd1-4f35-9186-614e450a07dc)

바이너리프로토콜을 사용하여 부호화하면 59 바이트

메시지팩과 다른 점은 필드네임이 없다는 점이다.

대신 숫자와 같은 필드 태그를 포함한다.

![image](https://github.com/IT-book-study/designing-data-intensive-applications/assets/81370558/b3efcf53-20d6-41c4-92c4-9ce976d5cbde)

컴팩트 프로토콜 부호화는 34바이트  

필드 타입과 태그 숫자를 단일 바이트로 줄이고 가변 길이 정수를 사용하여 부호화한다. 

![image](https://github.com/IT-book-study/designing-data-intensive-applications/assets/81370558/41443940-528e-4f0f-aaaf-ae8c11e83578)

프로토콜 버퍼는 33바이트

프로토콜 버퍼는 동일한 데이터를 부호화한다.

비트를 줄여 저장하는 처리 방식이 약간 다르지만 스리프트의 컴팩트 프로토콜과 매우 비슷하다.

proto3에서는 required가 없어졌다.

## 필드 태그와 스키마 발전














