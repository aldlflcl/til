###Object
```html
user.username => user.getUsername() // 프로퍼티 접근법
user['username'] => user.getUsername()
user.getUsername() => 직접 호출
```

###List
```html
users[0].username => List에서 첫번째 객체를 찾고 username프로퍼티 접근  list.get(0).getUsername();
users[0].['username']: 위와 같다.
users[0].getUsername() => List에서 첫번째 회원을 찾고 메서드 직접 호출
```

###Map
```html
userMap['userA'].username: Map에서 userA를 키로 찾고, username을 프로퍼티 접근
userMap['userA']['username']: 위와 같음
userMap['userA'].getUsername(): Map에서 userA를 키로 찾고 직접 메서드 호출
```