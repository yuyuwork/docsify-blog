
<!-- # Stream流转换 -->

**集合List转Map**

- 指定key-value value可以为 `对象属性`

```java
//Key不能重复，否则会报错，默认排序
Map<Integer,String> --> collect(Collectors.toMap(User::getId,User::getName))
```

<br/>

- 指定key-value value可以为 `对象本身`

```java
//1、Function.identity()简洁写法,且按key区分唯一
Map<Integer,User> --> collect(Collectors.toMap(User::getId,User->User))
Map<Integer,User> --> collect(Collectors.toMap(User::getId, Function.identity()))

//2、同上，key冲突的解决办法，选择第二个key覆盖第一个key，后面的重复的Key添加不进去，默认排序
Map<Integer,User> --> collect(Collectors.toMap(User::getId,a->a,(k1,k2)-> k2))
Map<Integer,User> --> collect(Collectors.toMap(User::getId, Function.identity(),(key1,key2)->key2))

//3、有序的List转Map,转为一个有序的map
Map<Integer,User> --> collect(Collectors.toMap(UserLombok::getName, a -> a, (k1, k2) -> k2,LinkedHashMap::new))
```

<br/>

- 指定key-value 按key分组,value可以为 `对象属性集合` 

```java
//最外层groupingBy分组,接着按firstName进行统计并放入集合
Map<Integer, List<String>> --> collect(Collectors.groupingBy(User::getAge,Collectors.mapping(User::getName,Collectors.toList())))
```

- 指定key-value 按key分组,value可以为 `对象本身集合`

```java
Map<Integer, List<User>> --> collect(Collectors.groupingBy(User::getAge))
```

<br/>
