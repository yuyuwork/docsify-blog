
<!-- # Stream的Map和FlatMap的区别 -->


### 单词拆分去重 <!-- {docsify-ignore} -->

**将Stream集合中的语句单词拆分，然后去重**

```java
Stream<String> testStream= Stream.of("hello welcome","world hello","hello world","hello world welcome");
```
 
1. map方法是将指定的Stream中元素进行“平级”处理，每个元素转化为各自所对应的一个Stream集合，输入与输出是一对一的关系，若进一步获取各个子Stream中的元素，还需要逐个遍历子集合得到的Stream中的元素与原Stream中的元素是“父子关系”

![](https://gitee.com/zhantu/picture-bed/raw/master/repository/technology-blog/blog-library/54.png)

```java
//通过Map方法
testStream.map(str-> Arrays.stream(str.split(" "))).forEach(strStream->
    strStream.forEach(s->totalList.add(s))
);
totalList.stream().distinct().forEach(System.out::println);
```

<br/>

2. flatMap将原有Stream中的元素进行转化为新的子Stream集合，同时对子Stream中的元素进行“跨级”处理，原有的Stream通过一次flatMap之后得到的不是各个子Stream的集合，而是各个子Steam集合中的元素的总集合，也就是对各个子Stream结合进行二次遍历取出中的元素，融合到一个总集合中得到的Stream中的元素与原Stream中的元素是“祖孙关系”

![](https://gitee.com/zhantu/picture-bed/raw/master/repository/technology-blog/blog-library/54.png)

```java
//通过flatMap方法
testStream.flatMap(str-> Arrays.stream(str.split(" "))).distinct().forEach(System.out::println);
```



