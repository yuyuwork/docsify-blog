
<!-- # Stream流统计 -->

**集合List分组统计处理**

```java
//按照年龄分组并统计其数量
Map<Integer,Long> --> collect(Collectors.groupingBy(UserLombok::getAge,Collectors.counting()))

//分组并获取各分组的第一个元素，借助Collectors.collectingAndThen方法，对组内的元素进行处理
Map<Integer,User> --> collect(Collectors.groupingBy(User::getAge,
    Collectors.collectingAndThen(Collectors.toList(), value->value.get(0))
))

//按年龄分组，返回每组集合对象
Map<Integer,List<User>> --> collect(Collectors.toMap(UserLombok::getAge, s -> {
    List<UserLombok> l = new ArrayList<>();
    l.add(s);
    return l;
}, (List<UserLombok> s1, List<UserLombok> s2) -> {
    s1.addAll(s2);
    return s1;
}));
```

<br/>
