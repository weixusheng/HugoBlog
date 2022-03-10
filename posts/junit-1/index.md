# 初识Junit



了解一下单元测试!
<!--more-->

# 什么是Junit

JUnit 是一个 Java 编程语言的单元测试框架。JUnit 在测试驱动的开发方面有很重要的发展，是起源于 JUnit 的一个统称为 xUnit 的单元测试框架之一

JUnit 促进了“先测试后编码”的理念，强调建立测试数据的一段代码，可以先测试，然后再应用。这个方法就好比“测试一点，编码一点，测试一点，编码一点……”，增加了程序员的产量和程序的稳定性，可以减少程序员的压力和花费在排错上的时间

# 什么是单元测试

单元测试用例是一部分代码，可以确保另一端代码（方法）按预期工作。为了迅速达到预期的结果，就需要测试框架。JUnit 是 java 编程语言理想的单元测试框架。

## 人工测试

手动执行测试用例并不借助任何工具的测试被称为人工测试。 - 消耗时间并单调：由于测试用例是由人力资源执行，所以非常缓慢并乏味。 - 人力资源上投资巨大：由于测试用例需要人工执行，所以在人工测试上需要更多的试验员。 - 可信度较低：人工测试可信度较低是可能由于人工错误导致测试运行时不够精确。 - 非程式化：编写复杂并可以获取隐藏的信息的测试的话，这样的程序无法编写。

## 自动测试

借助工具支持并且利用自动工具执行用例被称为自动测试。 - 快速自动化运行测试用例时明显比人力资源快。 - 人力资源投资较少：测试用例由自动工具执行，所以在自动测试中需要较少的试验员。 - 可信度更高：自动化测试每次运行时精确地执行相同的操作。 - 程式化：试验员可以编写复杂的测试来显示隐藏信息。

一个正式的编写好的单元测试用例的特点是：**已知输入和预期输出**，即在测试执行前就已知。已知输入需要测试的先决条件，预期输出需要测试后置条件。

# 优点

1. 可以书写一系列的测试方法，对项目所有的接口或者方法进行单元测试。

2. 启动后，自动化测试，并判断执行结果, 不需要人为的干预。

3. 只需要查看最后结果，就知道整个项目的方法接口是否通畅。

4. 每个单元测试用例相对独立，由Junit 启动，自动调用。不需要添加额外的调用语句。

5. 添加，删除，屏蔽测试方法，不影响其他的测试方法。 开源框架都对JUnit 有相应的支持。

# 编写一个Junit

## 引入jar包

使用Maven管理器,在pom.xml中添加

```xml
<!-- https://mvnrepository.com/artifact/junit/junit -->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
```

## 项目目录

新建Maven项目,目录正好符合要求

其中test就是存放测试代码的地方

![](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Java/junit-1/1.png)

被测试代码放在`main`下的`java`目录中，junit测试代码放在`test`下的`java`目录中，形成一一对应关系，测试代码使用`Test`开头命名

## 单一单元测试实例

### MessageDemo.java

```java
// path: src/main/java/MessageDemo.java
public class MessageDemo {
    private String message;

    public MessageDemo(String message){
        this.message = message;
    }

    public String printMessage(){
        System.out.println(this.message);
        return message;
    }
}
```

### TestMessageDemo.java

```java
// path: src/test/java/TestMessageDemo.java
import org.junit.Test;
import static org.junit.Assert.assertEquals;

public class TestMessageDemo {
    private String message = "HelloWorld";
    private MessageDemo messageDemo = new MessageDemo(this.message);

    @Test
    public void testPrintMessage(){
        assertEquals(message, messageDemo.printMessage());
    }
}
```

TestRunner.java

```java
// path: src/test/java/TestRunner
import org.junit.Test;
import org.junit.runner.JUnitCore;
import org.junit.runner.Result;
import org.junit.runner.notification.Failure;

public class TestRunner {
    public static void main(String[] args) {
        Result result = JUnitCore.runClasses(TestMessageDemo.class);
        for (Failure failure : result.getFailures()){
            System.out.println(failure.toString());
        }
        System.out.println("测试结果" + result.wasSuccessful());
    }
}
```

`MessageDemo.java`为业务代码(待测试代码)

`TestMessageDemo`为测试代码,对业务代码进行判断`assertEquals`

`TestRunner`为测试运行代码,是main主函数

## JUnit 断言

什么是断言？刚开始我也很困惑，后来搞了大半天才明白断言就是"判断"。

Junit所有的断言都包含在 Assert 类中。

这个类提供了很多有用的断言方法来编写测试用例。只有失败的断言才会被记录。Assert 类中的一些有用的方法列式如下：

1. `void assertEquals(boolean expected, boolean actual)`:检查两个变量或者等式是否平衡
2. `void assertTrue(boolean expected, boolean actual)`:检查条件为真
3. `void assertFalse(boolean condition)`:检查条件为假
4. `void assertNotNull(Object object)`:检查对象不为空
5. `void assertNull(Object object)`:检查对象为空
6. `void assertSame(boolean condition)`:assertSame() 方法检查两个相关对象是否指向同一个对象
7. `void assertNotSame(boolean condition)`:assertNotSame() 方法检查两个相关对象是否不指向同一个对象
8. `void assertArrayEquals(expectedArray, resultArray)`:assertArrayEquals() 方法检查两个数组是否相等

## JUnit 注解

1. `@Test`:这个注释说明依附在 JUnit 的 public void 方法可以作为一个测试案例。
2. `@Before`:有些测试在运行前需要创造几个相似的对象。在 public void 方法加该注释是因为该方法需要在 test 方法前运行。
3. `@After`:如果你将外部资源在 Before 方法中分配，那么你需要在测试运行后释放他们。在 public void 方法加该注释是因为该方法需要在 test 方法后运行。
4. `@BeforeClass`:在 public void 方法加该注释是因为该方法需要在类中所有方法前运行。
5. `@AfterClass`:它将会使方法在所有测试结束后执行。这个可以用来进行清理活动。
6. `@Ignore`:这个注释是用来忽略有关不需要执行的测试的。

### JUnit 加注解执行过程

- `beforeClass()`: 方法首先执行，并且只执行一次。
- `afterClass()`:方法最后执行，并且只执行一次。
- `before()`:方法针对每一个测试用例执行，但是是在执行测试用例之前。
- `after()`:方法针对每一个测试用例执行，但是是在执行测试用例之后。
- 在 before() 方法和 after() 方法之间，执行每一个测试用例。

## JUnit 执行测试

测试用例是使用 JUnitCore 类来执行的。JUnitCore 是运行测试的外观类。要从命令行运行测试，可以运行`java org.junit.runner.JUnitCore`。对于只有一次的测试运行，可以使用静态方法 `runClasses(Class[])`。

## 多单元测试实例

### JUnit 套件测试

测试套件意味着捆绑几个单元测试用例并且一起执行他们。在 JUnit 中，`@RunWith`和`@Suite`注释用来运行套件测试。

### 文件组成

- main/java/SuiteDemo
- test/java/TestDemo1
- test/java/TestDemo2
- test/java/TestSuite
- test/java/TestSuiteRun

其中`SuiteDemo`为待测单元(业务)代码

`TestDemo1`,`TestDemo2`为两个测试单元

`TestSuite`将两个测试单元引入套件

`TestSuiteRun`为main函数,启动单元测试

### SuiteDemo

```java
public class SuiteDemo {
    private String message;
    public SuiteDemo(String message){
        this.message = message;
    }

    public String printmessage(){
        System.out.println(this.message);
        return message;
    }
}
```

### TestDemo1

```java
import org.junit.Test;
import static org.junit.Assert.assertEquals;

public class TestDemo {
    private String message = "Helloworld";
    private SuiteDemo suiteDemo = new SuiteDemo(this.message);

    @Test
    public void testPrintMessage(){
        assertEquals(message, suiteDemo.printmessage());
    }
}
```

### TestDemo2

```java
import org.junit.Test;
import static org.junit.Assert.assertEquals;

public class TestDemo2 {
    private String message = "Robert";
    private SuiteDemo suiteDemo = new SuiteDemo(this.message);

    @Test
    public void testPrintMessage(){
        message = "HI";
        assertEquals(message, suiteDemo.printmessage());
    }
}
```

### TestSuite

```java
import org.junit.runner.RunWith;
import org.junit.runners.Suite;

@RunWith(Suite.class)
@Suite.SuiteClasses({
        TestDemo.class,
        TestDemo2.class
})

public class TestSuite {

}
```

### TestSuiteRun

```java
import org.junit.runner.JUnitCore;
import org.junit.runner.Result;
import org.junit.runner.notification.Failure;

public class TestSuiteRun {
    public static void main(String[] args) {
        Result result = JUnitCore.runClasses(TestSuite.class);
        for(Failure failure : result.getFailures()){
            System.out.println(failure.toString());
        }
        System.out.println("测试结果" + result.wasSuccessful());
    }
}
```

## Junit时间测试

如果一个测试用例比起指定的毫秒数花费了更多的时间，那么 Junit 将自动将它标记为失败。`timeout`参数和 `@Test` 注释一起使用。是不是很强大？

```java
@Test(timeout=1000)	//时间单位是毫秒
```

## JUnit 异常测试

Junit 用代码处理提供了一个追踪异常的选项。你可以测试代码是否它抛出了想要得到的异常。`expected` 参数和 `@Test` 注释一起使用。

```java
@Test(expected = ArithmeticException.class)
```

## JUnit参数化测试

- 使用@RunWith（Parameterized.class）注释测试类。
- 创建一个使用@Parameters注释的公共静态方法，该方法返回一个对象集合（作为数组）作为测试数据集。
- 创建一个公共构造函数，它接受相当于一行“测试数据”的内容。
- 为测试数据的每个“列”创建一个实例变量。
- 使用实例变量作为测试数据的来源创建测试用例。

对于每行数据，将调用一次测试用例

### 文件结构

- src/main/java/NumsDemo

- src/test/java/TestNumsChecker

- src/test/java/TestNums

其中`NumsDemo`为待测单元(业务)代码

`TestNumsChecker`为测试代码,其中包含多条测试参数

`TestNums`为测试执行main函数

### NumsDemo.java

```java
//path: src/main/java/NumsDemo
public class NumsDemo {
    public boolean validate(final int primeNumber){
        for(int i=2; i<(primeNumber/2); i++){
            if(primeNumber % i == 0){
                return false;
            }
        }
        return true;
    }
}
```

### TestNumsChecker.java

```java
//path: src/test/java/TestNumsChecker
import java.util.Arrays;
import java.util.Collection;
import org.junit.Test;
import org.junit.Before;
import org.junit.runners.Parameterized;
import org.junit.runners.Parameterized.Parameters;
import org.junit.runner.RunWith;
import static org.junit.Assert.assertEquals;

@RunWith(Parameterized.class)
public class TestNumsChecker {
    private Integer inputNumber;
    private Boolean expectedResult;
    private NumsDemo primeNumberChecker;
    @Before
    public void initialize() {
        primeNumberChecker = new NumsDemo();
    }
    // Each parameter should be placed as an argument here
    // Every time runner triggers, it will pass the arguments
    // from parameters we defined in primeNumbers() method
    public TestNumsChecker(Integer inputNumber, Boolean expectedResult) {
        this.inputNumber = inputNumber;
        this.expectedResult = expectedResult;
    }

    @Parameterized.Parameters
    public static Collection primeNumbers() {
        return Arrays.asList(new Object[][] {
                { 2, true },
                { 6, false },
                { 19, true },
                { 22, false },
                { 23, true }
        });
    }
    // This test will run 4 times since we have 5 parameters defined
    @Test
    public void testPrimeNumberChecker() {
        System.out.println("Parameterized Number is : " + inputNumber);
        assertEquals(expectedResult,
                primeNumberChecker.validate(inputNumber));
    }
}
```

### TestNums.java

```java
//path: src/test/java/TestNums
import org.junit.runner.JUnitCore;
import org.junit.runner.Result;
import org.junit.runner.notification.Failure;

public class TestNums {
    public static void main(String[] args) {
        Result result = JUnitCore.runClasses(TestNumsChecker.class);
        for (Failure failure : result.getFailures()) {
            System.out.println(failure.toString());
        }
        System.out.println(result.wasSuccessful());
    }
}
```






