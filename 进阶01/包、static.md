### 分包

```java
分包分类：例：
		Controller包
			StudentController
			TeacherController
			CourseController

		Service包
			StudentService
			TeacherService
			CourseService

		dao包
			StudentDao
			TeacherDao
			CourseDao

		domain包
			Student
			Teacher
			Course
```



### static

> `共享`、`类名`、`先后人`、`this`、`一次`
>
> 1：静态的东西 可以被该类所有的对象共享
>
> 2：静态的东西，可以用对象名调用，也可以用类名调用，但是 推荐用类名调用。
>
> 3：静态的随着类的加载而加载进来的。 比创建对象，要加载的早。
> 	非静态的东西随着对象的创建而存在的，  他比静态加载的晚。
>
> ​	静态的不能直接访问非静态的。   非静态的可以直接访问静态的。
>
> 4：静态的方法里面不能有 this
>
> 5：静态代码块(☆☆☆☆☆): 是随着类的加载而执行， 而且仅执行唯一的一次。（最先执行，只执行一次）

> 代码块：
>
> 代码块分为：
>
> - 局部代码块：`{...}`
>
> ​				目的是让你尽早的把变量从内存中清理出去。
>
> - 构造代码块： 
>
> ```java
> class demo{
>     {
>         ...
>     }
> }
> ```
>
> ​				每次调用构造方法的时候，都会执行构造代码块，而且是在构造方法之前执行的。
> ​				作用： 可以所有构造方法里面 共有的一些繁复的代码 提取出来 放到构造代码块中。
>
> - 静态代码块(☆☆☆☆☆): 
>
> ```java
> class demo{
>     static{
>         ....
>     }
> }
> ```
>
> ​				是随着类的加载而执行， 而且仅执行唯一的一次。

