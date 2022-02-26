
<!-- # Stream.reduce()合并流 -->

?> 在 Java 8 中，Stream.reduce()合并流的元素并产生单个值

### 求和运算 <!-- {docsify-ignore} -->

**使用 for 循环的简单求和运算**

```java
int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

int sum = 0;
for (int i : numbers) {
   sum += i;
}
 
System.out.println("sum : " + sum); // 55
```

**相当于 Stream.reduce()**

```java
int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
 
// 1st argument, init value = 0
int sum = Arrays.stream(numbers).reduce(0, (a, b) -> a + b);

System.out.println("sum : " + sum); // 55
```

或方法引用 Integer::sum

```java
int sum = Arrays.stream(numbers).reduce(0, Integer::sum); // 55


//Integer.java
/**
 * Adds two integers together as per the + operator.
 *
 * @param a the first operand
 * @param b the second operand
 * @return the sum of {@code a} and {@code b}
 * @see java.util.function.BinaryOperator
 * @since 1.8
 */
public static int sum(int a, int b) {
    return a + b;
}
```

<br/>

### 方法签名 <!-- {docsify-ignore} -->

**Stream.reduce()方法签名**

```java
//Stream.java
T reduce(T identity, BinaryOperator<T> accumulator);

//IntStream.java
int reduce(int identity, IntBinaryOperator op);

//LongStream.java
long reduce(int identity, LongBinaryOperator op);
```

● identity = 默认值或初始值
● BinaryOperator = 函数式接口，取两个值并产生一个新值。（注： java Function 函数中的 BinaryOperator 接口用于执行 lambda 表达式并返回一个 T 类型的返回值）


```java
//Stream.java  如果缺少identity参数，则没有默认值或初始值，并且它返回 optional
Optional<T> reduce(BinaryOperator<T> accumulator);
```

<br/>

**更多使用示例**

```java
//1 数学运算
int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
 
int sum = Arrays.stream(numbers).reduce(0, (a, b) -> a + b);    // 55
int sum2 = Arrays.stream(numbers).reduce(0, Integer::sum);      // 55
 
int sum3 = Arrays.stream(numbers).reduce(0, (a, b) -> a - b);   // -55
int sum4 = Arrays.stream(numbers).reduce(0, (a, b) -> a * b);   // 0, initial is 0, 0 * whatever = 0
int sum5 = Arrays.stream(numbers).reduce(0, (a, b) -> a / b);   // 0

//2 最大和最小
int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
 
int max = Arrays.stream(numbers).reduce(0, (a, b) -> a > b ? a : b);  // 10
int max1 = Arrays.stream(numbers).reduce(0, Integer::max);            // 10
 
int min = Arrays.stream(numbers).reduce(0, (a, b) -> a < b ? a : b);  // 0
int min1 = Arrays.stream(numbers).reduce(0, Integer::min);            // 0

//3 连接字符串
String[] strings = {"a", "b", "c", "d", "e"};
 
// |a|b|c|d|e , the initial | join is not what we want
String reduce = Arrays.stream(strings).reduce("", (a, b) -> a + "|" + b);
 
// a|b|c|d|e, filter the initial "" empty string
String reduce2 = Arrays.stream(strings).reduce("", (a, b) -> {
    if (!"".equals(a)) {
        return a + "|" + b;
    } else {
        return b;
    }
});
 
// a|b|c|d|e , better uses the Java 8 String.join :)  （最好使用 Java 8 的 String.join）
String join = String.join("|", strings);
```

3. Map & Reduce
一个简单的 map 和 reduce 示例，用于从发票 List 中求 BigDecimal 的和

```java
import java.math.BigDecimal;
import java.math.RoundingMode;
import java.util.Arrays;
import java.util.List;
 
public class JavaReduce {
 
    public static void main(String[] args) {
 
        // 发票集合  发票：发票号码，价格，数量
        List<Invoice> invoices = Arrays.asList(
                new Invoice("A01", BigDecimal.valueOf(9.99), BigDecimal.valueOf(1)),
                new Invoice("A02", BigDecimal.valueOf(19.99), BigDecimal.valueOf(1.5)),
                new Invoice("A03", BigDecimal.valueOf(4.99), BigDecimal.valueOf(2))
        );
 
        //reduce，将上一步得到的结果进行合并得到最终的结果
        BigDecimal sum = invoices.stream().map(x -> x.getQty().multiply(x.getPrice()))
                .reduce(BigDecimal.ZERO, BigDecimal::add);
 
        System.out.println(sum);//49.955
        System.out.println(sum.setScale(2, RoundingMode.HALF_UP));//49.96使用setScale方法进行四舍五入
    }
 
}
```
