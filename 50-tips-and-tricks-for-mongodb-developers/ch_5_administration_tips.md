Administration Tips
=

chunks collections을 직접 정리하라
-
GridFS은 컨텐츠들을 chunks collections(fs.chunks)으로 유지한다.
고아 chunks가 없는지 때때로 체크를 하는 것은 좋은 일이다.
고아 chunks는 저장 도중 데이터베이스에 문제가 생기면 생성될 수 있다.

트래픽이 적을 때, 아래와 같은 방식으로 확인할 수 있다.
```
> var cursor = db.fs.chunks.find({}, {"_id" : 1, "files_id" : 1}); > while (cursor.hasNext()) {
... var chunk = cursor.next();
... if (db.fs.files.findOne({_id : chunk.files_id}) == null) {
... print("orphaned chunk: " + chunk._id); 
... }
```

한 서버의 투표권을 조작하지 마라
-
특정 서버를 프라이머리로 유지하고 싶어 투표권을 조작하려 할 수 있다. 하지만 서버는 자신에게 투표하지 못한다. 그러니 이런것 하지마라.

레플리카 셋은 마스터가 없는 상황에도 reconfigured 될 수 있다.
-

--notablescan은 개발환경에서만 사용하라
-
--notablescan은 좋은 디버깅 툴이지만 프로덕션에서는 쓰지 마라.

js를 공부하라
-

서버와 DB를 한 가지 shell에서 관리할 수 있다
-
```
> master = connect("ny1a:27017/admin") connecting to: ny1a:27017/admin
admin
> slave = connect("ny1b:27017/admin") connecting to: ny1b:27017/admin
admin
```
위와 같은 방식으로 DB를 변수에 할당하여 사용할 수 있다

Get “help” for any function
-
몽고에서 함수를 사용할 때, 내용을 모르겠으면 js 코드를 까봐라.
```
> db.addUser
function (username, pass, readOnly) {
readOnly = readOnly || false;
var c = this.getCollection("system.users");
```
이와 같은 방식으로 도움을 얻을 수 있다.

startup files을 만들어라
-
js 파일을 사용해, 몽고의 시작전에 원하는 대로 사용할 수 있는 모듈을 만들어라

자신만의 함수를 만들어라
-
마찬가지로 js의 문법을 사용해 정의하면 된다.

또 load() function을 사용해서 js의 라이브러리를 shell에 불러오는 것도 언제든지 가능하다.

```
// hello.js
print("Hello, world!")

------------shell--------------
> load("hello.js") Hello, world!
```

read your own writes을 하기위해 하나의 커넥션을 유지하라
-
몽고의 커넥션을 사용할 때, 커넥션은 요청에 대한 큐와 같이 동작한다.

따라서 커넥션을 하나로 유지하면 read your own write을 할 수 있다.


