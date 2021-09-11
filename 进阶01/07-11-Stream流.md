### Stream流

#### 可变参数

可变参数相当于传入一个数组，可变参数要放在形参最后面

	格式：
		修饰符 返回值类型 方法名(数据类型... 参数名){
		
		}

#### 创建不可变集合

集合的of方法

> 用来批量添加元素
>
> 不能增删改，只能用来查
>
> `List.of(<E>... e)`
>
> `Set.of(<E>... e)`
>
> `Map.of(<E>... e)`
>
> `Map.entryOf(<E>... e)`

> Arrays类的方法
>
> 不能增删们只能改查
>
> `Arrays.asList()`

#### Stream流

操作集合数据

使用匿名内部类的延迟执行（不调用不执行）实现

- 生成Stream流的方式
  - Collection体系集合

    使用默认方法stream()生成流， default Stream<E> stream()

  - Map体系集合

    把Map转成Set集合，间接的生成流

  - 数组

    通过Arrays中的静态方法stream生成流

  - 同种数据类型的多个数据

    通过Stream接口的静态方法of(T... values)生成流

##### 方法

| 方法名                                          | 说明                                                       |
| ----------------------------------------------- | ---------------------------------------------------------- |
| Stream<T> filter(Predicate predicate)           | 用于对流中的数据进行过滤                                   |
| Stream<T> limit(long maxSize)                   | 返回此流中的元素组成的流，截取前指定参数个数的数据         |
| Stream<T> skip(long n)                          | 跳过指定参数个数的数据，返回由该流的剩余元素组成的流       |
| static <T> Stream<T> concat(Stream a, Stream b) | 合并a和b两个流为一个流                                     |
| Stream<T> distinct()                            | 返回由该流的不同元素（根据Object.equals(Object) ）组成的流 |

| 方法名                        | 说明                     |
| ----------------------------- | ------------------------ |
| void forEach(Consumer action) | 对此流的每个元素执行操作 |
| long count()                  | 返回此流中的元素数       |

| 方法名                         | 说明               |
| ------------------------------ | ------------------ |
| R collect(Collector collector) | 把结果收集到集合中 |

| 方法名                                                       | 说明                   |
| ------------------------------------------------------------ | ---------------------- |
| public static <T> Collector toList()                         | 把元素收集到List集合中 |
| public static <T> Collector toSet()                          | 把元素收集到Set集合中  |
| public static  Collector toMap(Function keyMapper,Function valueMapper) | 把元素收集到Map集合中  |
