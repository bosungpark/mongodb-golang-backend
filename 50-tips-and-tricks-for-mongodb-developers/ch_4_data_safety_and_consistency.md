Data Safety and Consistency
=

single server에는 journal을 multiserver에는 replicas를 사용하라
-
Replication과 journaling(데이터의 변경 이력을 추적하고 복구하는 데이 사용)은 몽고에서 데이터의 안전성을 유지할 수 있는 방법들이다. 이 둘은 동시에 사용할 수도 있다.

- No safety: One server write per write request
- Replication: Two server writes per write request
- Journaling: Two server writes per write request
- Replication+journaling: Three server writes per write request

여러 레플리카에 문제가 생기고 저널링을 하고 있지 않았을 때에는 몽고는 데이터에 대해 보장해주지 않는다. 이 때에는 백업을 가지고 복구하거나 fastsync를 고려할 수 있다.

Replication과 journaling을 항상 사용하라.(함께 사용하거나)
-
journal을 항상 사용하지 않을 이유는 어디에도 없다.

리커버 데이터에 의존하지 마라
-
가장 좋은 대안은 fastsyncing이다.

safe writes를 항상 사용하라
-

w를 레플리카와 함께 사용하라
-
w는 커밋을 하기전 요구되는 레플라카의 응답 갯수를 의미한다.

w를 사용할 때에는 wtimeout을 사용하라
-

fsync를 모든 쓰기연산마다 쓰지 마라
-
정확성이 정말 중요한 데이터가 있다면 fsync를 사용해야한다.
fsync는 다음 flush에서 성공 응답을 보내기 이전에 데이터가 저널에 제대로 입력되었는지를 확인한다. 즉시가 아니고 다음 flush에서다.
그래서 매 순간 사용하게 되면 100ms정도의 손해를 보게 된다.
fsync는 저널을 사용할 때에만 사용해야한다. 그렇지않으면 성능적으로 이득이 없다.

백업 데이터를 만들어라
-
Note that you can’t just copy all of the files without fsync and locking, as copying is not an instantaneous operation.
