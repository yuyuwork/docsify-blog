
<!-- # 集合Stream流处理 -->

### 集合Stream转换

**集合List转Map**

- 指定key-value value可以为 `对象属性`

```java
//Key不能重复，否则会报错，默认排序
Map<Integer,String> --> collect(Collectors.toMap(User::getId,User::getName))
```

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

### 集合Stream统计

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

### 集合Stream排序

**集合List对象排序**
  
- 集合List默认排序

```java
//复合类型对象按age进行排序
// -正序  
personList.sort((o1, o2) -> o1.getAge().compareTo(o2.getAge()))
personList.sort(Comparator.comparing(UserLombok::getAge))

// -倒序
personList.sort((o1, o2) -> o2.getAge().compareTo(o1.getAge()))
personList.sort(Comparator.comparing(UserLombok::getAge).reversed())

//对象需实现Comparator<复合对象> ？
```

- 集合List Stream式排序

```java
//根据中文名称排序
List<UserLombok> --> sorted(Comparator.comparing(UserLombok::getName, Collator.getInstance(Locale.CHINA)))

//根据创建时间-倒序排列
List<UserLombok> --> sorted(Comparator.comparing(UserLombok::getCreateDate,Comparator.reverseOrder()))
```

`使用示例：根据创建时间倒序排列，然后按照年龄分组，然后取第一条数据返回`

<br/>

**集合Map对象排序**

- 集合Map Stream式排序

```java
Map<String, Integer> map = Maps.newHashMap();
map.put("张三",10);

//以下三种方式不同之处在于排序的处理，可以配合map()和collect()转换为List集合对象
map.entrySet().stream().sorted(Comparator.comparing(e->e.getKey()))
map.entrySet().stream().sorted(Comparator.comparing(Map.Entry::getValue))
map.entrySet().stream().sorted(Map.Entry.comparingByKey())

//https://www.concretepage.com/java/jdk-8/java-8-convert-map-to-list-using-collectors-tolist-example
```

`使用示例：Map数据转为自定义对象的List，如把map的key,value分别对应UserLombok对象两个属性`

