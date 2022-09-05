### 객체 리스트 정렬 (오름차순)

```java
Collections.sort(list, new Comparator<UserAppointmentResponseDTO>() {
            @Override
            public int compare(UserAppointmentResponseDTO o1, UserAppointmentResponseDTO o2) {
                return o1.getLocalDateTime().compareTo(o2.getLocalDateTime());
            }
        });
```


### 객체 리스트 정렬 (내림차순)

```java
Collections.sort(list, new Comparator<UserAppointmentResponseDTO>() {
            @Override
            public int compare(UserAppointmentResponseDTO o1, UserAppointmentResponseDTO o2) {
                return o2.getLocalDateTime().compareTo(o1.getLocalDateTime());
            }
        });
```

---
### 2차원 배열에서 2차원값 정렬하기

```java
Arrays.sort(location, (o1, o2) -> {
            if(o1[0] == o2[0]) return Integer.compare(o1[1], o2[1]);
            else return Integer.compare(o1[0], o2[0]);
        });
//
```
---
### 반올림

```java
double pie = 3.14159265358979;
System.out.println(String.format("%.2f", pie)); //결과 : 3.14
```

- return type → String

---
