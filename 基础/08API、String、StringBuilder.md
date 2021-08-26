### API文档

用来查看Java核心类库中的类的描述，查询Java类的构造方法和方法。

`java.lang`下的类不用进行`import`导包动作。



### ’基础‘常用类

- `Scanner` :键盘录入，后期用不到

- `Random` :生成随机数

- `String` :字符串类

  - 常用的String类方法：

    - `char charAt (int index)`：返回指定索引处的char值
    - `boolean equals (object obj)`：将此字符串与指定的对象比较（主要用来比较字符串内容）
      - 使用`==`比较的是地址值，使用`equals`比较的是内容。
    - `boolean equalsIgnoreCase`：与`equals`作用一样，但忽略大小写
    - `int indexOf()`
    - `int length()`
    - `String replace(要替换的字符序列（字符）,替换后的字符序列（字符）)`
    - `String[] split(String regex)`
    - `char[] toCharArray()`

  - `String`类的拼接原理

    - ```java
      String str = "";
      str += "a";
      //======================
      实际底层原理：
      new StringBuilder(str).append("a").toString;
      ```

  - 字符串常量池：

    - 在堆中，使用`String str = "string"`的方式定义时，`string`会进入字符串常量池，当有另一个字符串常量`String str1 = 'string'`被赋相同值时，java虚拟机检索堆中字符串常量池存在此值，则会将此String对象同样也指向字符串常量池中的`string`地址，此时两个String对象的地址值相同。

- `StringBuilder`: 主要用来操作字符串，字符串多次拼接效率比直接用String要高

  - ```java
    psvm....{
        String str = new String();
    	for(int i = 0; i < 5;i++){
       	 str += "a";
    	}
    }
    上述代码的拼接过程：
        1.每次拼接前先转成StringBuilder对象=>new StringBuilder(str);
    	2.调用append(...)方法拼接字符串=>new StringBuilder(str).append("a");
        3.调用toString()方法转成字符串对象=>new StringBuilder(str).append("a").toString();
    	相当于每循环一次都要创建一个StringBuilder对象并调用其方法，完成后使用toString()生成新的String对象赋值给拼接对象
    
    而先将String转成StringBuilder再调用其方法对字符串进行操作，所有的字符拼接操作完成后再转成String对象，可以大大节省在堆内存中反复创建新对象的资源。
    ```

  - 常用方法

    - `StringBuilder append(...)`:在调用这个方法之后返回的还是一个`StringBuilder`对象，因此可以进行链式编程，可以借鉴此思想进行方法构建编程。

      - ```java
        StringBuilder sb = new StringBuilder();
        sb.append("a").append("b").append...;
        //因为返回的是StringBuilder对象，因此可以继续调用StringBuilder的方法。
        ```

    - 其他方法查api文档吧......

- `ArrayList`:集合

  - ```java
    ArrayList<E> al = new ArratList<>()
        /*
        泛型
        以后了解
        */
    ```

  - 方法见API文档，懒得写。。。

  

