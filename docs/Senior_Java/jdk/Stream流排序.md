
<!-- # Stream流排序 -->

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


