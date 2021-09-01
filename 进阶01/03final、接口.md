## final

> 修饰类：不能被继承
>
> 修饰方法：不能被重写
>
> 修饰变量：变为常量，值无法再次赋值
>
> 		- 基本数据类型：值不能被修改
> 		- 引用数据类型： 地址值不能被修改
>
> 修饰局部变量： 在使用前必须赋值，不能被重新赋值
>
> 修饰成员变量：必须在构造方法执行完之前赋值，不能使用默认值



## 接口 interface

### 特点

- 接口不能创建对象
- 子类与接口之间是实现关系，可以多实现
- 子类实现接口必须重写接口中的所有抽象方法（规范性的体现）



### 接口的作用

规范性、拓展性



### 成员

jdk9之前，接口中所有成员都是public的，jdk9之后有私有方法`private ...`



### 构造方法

无



### 成员变量

接口中的成员变量全部都是常量		`public static final`



### 成员方法

#### jdk8之前

接口中的方法只能有抽象方法 默认使用 `public abstract`修饰

#### jdk8之后

默认方法`public default ...`

静态方法`public static ...`：只能通过接口名调用

#### jdk9之后

有私有方法`private ...` `private static ...`



### 类与接口的关系

类和类：继承关系，单继承，不可多继承，可多层继承

类和接口：实现关系，可单继承，可多继承

接口和接口：继承关系，可单继承，可多继承



### 接口的解耦合性

#### 工厂设计模式

解耦合，将两个类的联系使用一个factory类断开

代码重点StudentDao、StudentServeice、Factory类

```java
package main.java.tests;

import java.util.ArrayList;

public class Demo01 {
/*分包:
    专门和用户交互的 */
    class StudentController {
        private StudentService ss = new StudentService();
        public void 界面(){
            // 让用户录入
            // 当用户录入 1  则表明添加学生 
            // 此时调用 本类的 addStudent()方法
        }
        public void addStudent(){
            // 录入学生
            boolean b =  ss.addStudent(stu);
            if (b){
                System.out.println("添加成功");
            }
        }
    }

/*    专门操作业务的 Service*/
    class StudentService {
        //private StudentDao sd = new StudentDao();
        //private OtherStudentDao sd = new OtherStudentDao();
        private  BaseStudentDao sd = DaoFactory.getDao();

        public boolean addStudent(Student stu){
            return sd.addStudent(stu);
        }
    }

/*    加一层 -----这一层的作用  用来获取 Dao的。*/
    class DaoFactory {
        public static BaseStudentDao getDao(){
            return new OtherStudentDao();
        }
    }  // 工厂设计模式。

  /*  专门操作数据的 Dao*/
    class BaseStudentDao {
        public boolean addStudent(Student stu){
            return true;
        }
    }
    class StudentDao extends BaseStudentDao{
        private static Student[] stus = new Student[5];

        public boolean addStudent(Student stu){
            // 添加到数组
            return true;
        }
    }

    class OtherStudentDao extends BaseStudentDao {
        private static ArrayList<Student> al = new ArrayList<>();

        public boolean addStudent(Student stu){
            // 添加到集合
            return true;
        }
    }
}

```

