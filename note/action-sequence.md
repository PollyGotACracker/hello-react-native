# React Native 앱의 동작 순서

```mermaid
%%{init: {'flowchart' : {'curve' : 'linear'}}}%%
flowchart TB
  subgraph JavaScript
    node4["4 Process event"]
    node5["5 Call native methods or update UI"]
  end
  subgraph Bridge
    node3["3 Serialized payload"]
    node6["6 Serialized batched response"]
  end
  subgraph Native
    node1["1 Event"]
    node2["2 Collect data and notify"]
    node7["7 Process commands"]
    node8["8 Update UI"]
  end
  node1 --> node2
  node2 --> node3
  node3 --> node4
  node4 --> node5
  node5 ==> node6
  node6 ==> node7
  node7 --> node8
```

- JavaScript 레이어(인터페이스, 영역)는 개발자가 작성한 코드

1. 사용자의 버튼 터치 등 이벤트 발생
2. nativeOS(android/iOS)에서 이를 감지하고 이벤트 정보 수집 및 전달 준비
3. 이벤트 데이터를 JSON 형태로 직렬화하여 bridge 를 통해 JS 로 메시지 전송
4. JS 에서 메시지를 받아 처리
   (eventListener 호출)
5. JS 에서 native 메서드 호출 또는 UI 업데이트 요청
6. JS 응답을 직렬화한 후 bridge 를 통해 native 로 메시지 전송
   (비동기적, batch 방식)
7. nativeOS에서 JS 로부터 전달받은 명령 처리
8. nativeOS에서 최종 UI 업데이트
