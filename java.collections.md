# 集合类

## 直接构造
```
List<String> stringList = new LinkedList<>();
stringList.add("a");
stringList.add("b");
stringList.add("c");
```
```
HashMap<String, String> map = new HashMap<String, String>();
 map.put("Name", "June"); 
 map.put("test", "sss");
```
## 匿名内部类
```
List<String> names = new ArrayList<>() 
{
	{
		add("Tom");
		add("Sally");
		add("John");
	}
};
```
```
HashMap<String, String> map = new HashMap<String, String>() 
{
 {
	 put("Name", "June"); 
	 put("test", "sss"); 
 }
};
```
## 使用 Arrays.asList
```
// 生成的list不可变
List<String> list = Arrays.asList("1", "2", "3");
// 如果要可变需要用ArrayList包装一下
List<String> numbers = new ArrayList<>(Arrays.asList("1", "2", "3"));
numbers.add("4");

//不可改变
List<String> cat = Collections.singletonList("cat");
System.out.println(cat);

int[] intArray = new int[]{1, 2, 3};
Integer[] integerArray = new Integer[]{1, 2, 3}; 
List<int[] > intArrayList = Arrays.asList(intArray);
List<Integer> integerList = Arrays.asList(integerArray);
List<Integer> integerList2 = Arrays.asList(1, 2, 3);
```

## 使用 Stream (JDK8)
```

List<String> list = Stream.of("a", "b", "c").collect(Collectors.toList());

```