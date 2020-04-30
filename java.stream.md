##Stream 
```
中间操作符
对于数据流来说，中间操作符在执行制定处理程序后，数据流依然可以传递给下一级的操作符。

中间操作符包含8种(排除了parallel,sequential,这两个操作并不涉及到对数据流的加工操作)：

map(mapToInt,mapToLong,mapToDouble) 转换操作符，把比如A->B，这里默认提供了转int，long，double的操作符。
flatmap(flatmapToInt,flatmapToLong,flatmapToDouble) 拍平操作比如把 int[]{2,3,4} 拍平 变成 2，3，4 也就是从原来的一个数据变成了3个数据，这里默认提供了拍平成int,long,double的操作符。
limit 限流操作，比如数据流中有10个 我只要出前3个就可以使用。
distint 去重操作，对重复元素去重，底层使用了equals方法。
filter 过滤操作，把不想要的数据过滤。
peek 挑出操作，如果想对数据进行某些操作，如：读取、编辑修改等。
skip 跳过操作，跳过某些元素。
sorted(unordered) 排序操作，对元素排序，前提是实现Comparable接口，当然也可以自定义比较器。
```
##终止操作符
```
数据经过中间加工操作，就轮到终止操作符上场了；终止操作符就是用来对数据进行收集或者消费的，数据到了终止操作这里就不会向下流动了，终止操作符只能使用一次。
collect 收集操作，将所有数据收集起来，这个操作非常重要，官方的提供的Collectors 提供了非常多收集器，可以说Stream 的核心在于Collectors。
count 统计操作，统计最终的数据个数。
findFirst、findAny 查找操作，查找第一个、查找任何一个 返回的类型为Optional。
noneMatch、allMatch、anyMatch 匹配操作，数据流中是否存在符合条件的元素 返回值为bool 值。
min、max 最值操作，需要自定义比较器，返回数据流中最大最小的值。
reduce 规约操作，将整个数据流的值规约为一个值，count、min、max底层就是使用reduce。
forEach、forEachOrdered 遍历操作，这里就是对最终的数据进行消费了。
toArray 数组操作，将数据流的元素转换成数组。
```
##创建流的方法1
```
List<String> list = new ArrayList<>();
Stream<String> stream = list.stream(); //获取一个顺序流
Stream<String> parallelStream = list.parallelStream(); //获取一个并行流
```
##创建流的方法2
```
Integer[] nums = new Integer[10];
Stream<Integer> stream = Arrays.stream(nums);
```
##创建流的方法3
```
Stream<Integer> stream = Stream.of(1,2,3,4,5,6); 
Stream<Integer> stream2 = Stream.iterate(0, (x) -> x + 2).limit(6);
stream2.forEach(System.out::println); // 0 2 4 6 8 10
 
Stream<Double> stream3 = Stream.generate(Math::random).limit(2);
stream3.forEach(System.out::println);
```
##创建流的方法4
```
Pattern pattern = Pattern.compile(",");
Stream<String> stringStream = pattern.splitAsStream("a,b,c,d");
stringStream.forEach(System.out::println);
```
##流的中间操作例子1
```
筛选与切片
filter：过滤流中的某些元素
limit(n)：获取n个元素
skip(n)：跳过n元素，配合limit(n)可实现分页
distinct：通过流中元素的 hashCode() 和 equals() 去除重复元素

Stream<Integer> stream = Stream.of(6, 4, 6, 7, 3, 9, 8, 10, 12, 14, 14); 
Stream<Integer> newStream = stream.filter(s -> s > 5) //6 6 7 9 8 10 12 14 14
        .distinct() //6 7 9 8 10 12 14
        .skip(2) //9 8 10 12 14
        .limit(2); //9 8
newStream.forEach(System.out::println);
```
##流的中间操作例子2
```
映射        
map：接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素。
flatMap：接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流。

List<String> list = Arrays.asList("a,b,c", "1,2,3");
 
//将每个元素转成一个新的且不带逗号的元素
Stream<String> s1 = list.stream().map(s -> s.replaceAll(",", ""));
s1.forEach(System.out::println); // abc  123
 
Stream<String> s3 = list.stream().flatMap(s -> {
    //将每个元素转换成一个stream
    String[] split = s.split(",");
    Stream<String> s2 = Arrays.stream(split);
    return s2;
});
s3.forEach(System.out::println); // a b c 1 2 3
```
##流的中间操作例子3
```
排序
sorted()：自然排序，流中元素需实现Comparable接口
sorted(Comparator com)：定制排序，自定义Comparator排序器  

List<String> list = Arrays.asList("aa", "ff", "dd");
//String 类自身已实现Compareable接口
list.stream().sorted().forEach(System.out::println);// aa dd ff
 
Student s1 = new Student("aa", 10);
Student s2 = new Student("bb", 20);
Student s3 = new Student("aa", 30);
Student s4 = new Student("dd", 40);
List<Student> studentList = Arrays.asList(s1, s2, s3, s4);
 
//自定义排序：先按姓名升序，姓名相同则按年龄升序
studentList.stream().sorted(
        (o1, o2) -> {
            if (o1.getName().equals(o2.getName())) {
                return o1.getAge() - o2.getAge();
            } else {
                return o1.getName().compareTo(o2.getName());
            }
        }
).forEach(System.out::println);
```
##流的中间操作例子4
```
peek：如同于map，能得到流中的每一个元素。但map接收的是一个Function表达式，有返回值；而peek接收的是Consumer表达式，没有返回值。

Student s1 = new Student("aa", 10);
Student s2 = new Student("bb", 20);
List<Student> studentList = Arrays.asList(s1, s2);
 
studentList.stream()
        .peek(o -> o.setAge(100))
        .forEach(System.out::println);   
 
//结果：
Student{name='aa', age=100}
Student{name='bb', age=100}
```
##流的终止操作1
```
匹配、聚合操作
        allMatch：接收一个 Predicate 函数，当流中每个元素都符合该断言时才返回true，否则返回false
        noneMatch：接收一个 Predicate 函数，当流中每个元素都不符合该断言时才返回true，否则返回false
        anyMatch：接收一个 Predicate 函数，只要流中有一个元素满足该断言则返回true，否则返回false
        findFirst：返回流中第一个元素
        findAny：返回流中的任意元素
        count：返回流中元素的总个数
        max：返回流中元素最大值
        min：返回流中元素最小值

List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
 
boolean allMatch = list.stream().allMatch(e -> e > 10); //false
boolean noneMatch = list.stream().noneMatch(e -> e > 10); //true
boolean anyMatch = list.stream().anyMatch(e -> e > 4);  //true
 
Integer findFirst = list.stream().findFirst().get(); //1
Integer findAny = list.stream().findAny().get(); //1
 
long count = list.stream().count(); //5
Integer max = list.stream().max(Integer::compareTo).get(); //5
Integer min = list.stream().min(Integer::compareTo).get(); //1
```
##流的终止操作2
```
Collector 工具库：Collectors

Student s1 = new Student("aa", 10,1);
Student s2 = new Student("bb", 20,2);
Student s3 = new Student("cc", 10,3);
List<Student> list = Arrays.asList(s1, s2, s3);
 
//转成list
List<Integer> ageList = list.stream().map(Student::getAge).collect(Collectors.toList()); // [10, 20, 10]
 
//转成set
Set<Integer> ageSet = list.stream().map(Student::getAge).collect(Collectors.toSet()); // [20, 10]
 
//转成map,注:key不能相同，否则报错
Map<String, Integer> studentMap = list.stream().collect(Collectors.toMap(Student::getName, Student::getAge)); // {cc=10, bb=20, aa=10}
 
//字符串分隔符连接
String joinName = list.stream().map(Student::getName).collect(Collectors.joining(",", "(", ")")); // (aa,bb,cc)
 
//聚合操作
//1.学生总数
Long count = list.stream().collect(Collectors.counting()); // 3
//2.最大年龄 (最小的minBy同理)
Integer maxAge = list.stream().map(Student::getAge).collect(Collectors.maxBy(Integer::compare)).get(); // 20
//3.所有人的年龄
Integer sumAge = list.stream().collect(Collectors.summingInt(Student::getAge)); // 40
//4.平均年龄
Double averageAge = list.stream().collect(Collectors.averagingDouble(Student::getAge)); // 13.333333333333334
// 带上以上所有方法
DoubleSummaryStatistics statistics = list.stream().collect(Collectors.summarizingDouble(Student::getAge));
System.out.println("count:" + statistics.getCount() + ",max:" + statistics.getMax() + ",sum:" + statistics.getSum() + ",average:" + statistics.getAverage());
 
//分组
Map<Integer, List<Student>> ageMap = list.stream().collect(Collectors.groupingBy(Student::getAge));
//多重分组,先根据类型分再根据年龄分
Map<Integer, Map<Integer, List<Student>>> typeAgeMap = list.stream().collect(Collectors.groupingBy(Student::getType, Collectors.groupingBy(Student::getAge)));
 
//分区
//分成两部分，一部分大于10岁，一部分小于等于10岁
Map<Boolean, List<Student>> partMap = list.stream().collect(Collectors.partitioningBy(v -> v.getAge() > 10));
 
//规约
Integer allAge = list.stream().map(Student::getAge).collect(Collectors.reducing(Integer::sum)).get(); //40
```




