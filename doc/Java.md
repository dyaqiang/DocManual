##### 常量

1. 字符串常量、字符常量
2. 整数、小数常量
3. 布尔常量 true\false
4. Null 常量

##### 数据类型

1. 基本数据类型
   * 整数   byte、short  int  Long
   * 浮点数  float  double
   * 字符  char
   * 布尔 boolean
2. 引用数据类型

##### 变量

1. 格式。 数据类型  变量名 = 初始化值
2. 数据类型：



#####  数组

1. 格式 int[]   a;  

2. 初始化

3. 操作数据

   * 遍历. for 遍历 、增强for 

     ```java
     public void tt() {
             int[] a = {1, 2, 3, 4, 5}; //静态初始化
             int[] b = new int[3]; //动态初始化
             for (int s : a) {
                 System.out.println("-:for"+s);
             }
         }
     ```

##### 集合

```java
public static void main(String[] args) {
        Collection<String> c =new ArrayList<>();
        //+
        c.add("one");
        int num =c.size();
        boolean b= c.contains("one");
        boolean cc = c.isEmpty();

        //元素为对象
        Collection<emp> c2 =  new ArrayList<>();
        emp e  = new emp("zhangsan",12,12);
        c2.add(e);

        //批量添加
        Collection<String> d = new ArrayList<>();
        d.add("one");
        Collection<String> f = new HashSet<>();
        f.add("2");
        f.add("3");
        f.add("4");
        f.add("5");
        f.add("6");
        boolean bb = d.addAll(f);
        
        //遍历
        Iterator<String> itr = d.iterator();
        while (itr.hasNext()){
            String str = itr.next();
            System.out.println(str);
        }
    }
```

###### List

* 特点

  1. 有序、可重复

* 分类

  1. ArrayList

     常见用法

     ```java
     public static void main(String[] args) {
     
             List<String> list = new ArrayList<>();
             list.add("one");
             list.add("two");
             String str2 = list.get(1);
     
             //遍历
             for (int i=0; i < list.size(); i++) {
                 String str = list.get(i);
                 System.out.println(str);
             }
     
             //修改+添加+删除
             String rep = list.set(1,"kkk");
             list.add(0,"zero");
             String rem =  list.remove(1);
     
             //子集
             List<String> sublist = list.subList(1,3);
         }
     
     ```

     转化

     ```java
     //数据集合的转换
     public static void main(String[] args) {
             Collection<String> c = new ArrayList<>();
             c.add("one");
             c.add("two");
             c.add("three");
     
             //集合变数组
             String[] array = c.toArray(new String[c.size()]);
             array[0] = "1";
             System.out.println(Arrays.toString(array));
             System.out.println(c);
     
     
             //数组转换为集合--会有变化
             String []  arr ={"1","2","3","4"};
             List<String> l =  Arrays.asList(arr);
             System.out.println(l);
             l.add("23");//转换的不能新增元素
             System.out.println(l);
         }
     ```

     排序

     ```
     
     ```

     

  2. Vector

  3. LinkedList

* 用法

###### Set

* 特点
  1. 无序，唯一
* 分类
  1. HashSet
  2. linkedHashSet.  有序
  3. TreeSet 有序

###### Map

```java
public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();

        map.put("zhangsan", 22);
        map.put("lisi", 34);
        map.put("wangwu", 54);
        map.put("wangwu", 45);
        System.out.println(map);

        //遍历key
        Set<String> keyset =map.keySet();
        System.out.println(keyset);
        keyset.forEach((str)-> System.out.println(str));

        //循环key+value
        Set<Map.Entry<String, Integer>> kvSet = map.entrySet();
        kvSet.forEach((e)->System.out.println(e.getKey()+":"+e.getValue()));

        //循环value
        Collection<Integer> values = map.values();
        values.forEach(System.out::println);
    }
```



* 分类
  1. hashmap
  2. treemap
  3. Hash table













-----



##### 面向对象

###### 1、类和对象

* 概念理解：   

  1. 类：图纸。

     * 属性：类所有的共同属性
     * 动作：类所有的执行功能

  2. 对象：类创造的具体的东西

  3. 类和对象

     ```java
     public class Car {
         //成员属性
         String color;
         int speed;
         int seat;
         //成员方法
         public void run() {
         }
         //程序入口
         public static void main(String[] args) {
             //创建对象
             Car c = new Car(); //面向对象中不叫变量，叫引用
             //类 引用 =new 类（）
             //调用对象属性
             c.color = "red";
             //调用对象方法
             c.run();
         }
     }
     ```

  4. ***this*** 关键字

     ```java
     public class Car {
         String color;
         int speed;
         int seat;
         public void run() {
             System.out.println(this.color);//this 表示当前对象
             System.out.println(color);//局部变量->成员变量的顺序
         }
         public static void main(String[] args) {
             Car c = new Car(); 
             c.color = "red";
             c.run();
         }
     }
     ```

  5. ***static*** 关键字

     * 所有对象的变量共享

       ```java
       public class Daxia {
           String name;
           Integer age;
           String bangpai;
           String waihao;
           static String sex ="男";//静态的东西
           public Daxia(String name, Integer age, String bangpai) {
               this.name = name;
               this.age = age;
               this.bangpai = bangpai;
           }
           public Daxia(String name, Integer age, String bangpai, String waihao) {
               this(name, age, bangpai);
               this.waihao = waihao;
           }
           public static void main(String[] args) {
               Daxia.sex = "女";//静态的东西
           } 
       }
       ```

     * 创建对象的过程

       ```mermaid
       graph LR
          0-->类-执行静态的内容 --> 创建对象
       ```

       ```mermaid
       graph LR
          静态构造器 --> 通用构造器 -->构造方法-->创建对象  
       ```

###### 2、构造方法

1. 特点
   * 没有返回值，
   * 自动调用
   * 默认赠送一个==无参数==的构造方法
2. 作用： 创建对象时，设置属性
3. 构造方法重载

```java
public class Daxia {
    String name;
    Integer age;
    String bangpai;
    String waihao;
    public Daxia(String name, Integer age, String bangpai) {
        this.name = name;
        this.age = age;
        this.bangpai = bangpai;
    }
    public Daxia(String name, Integer age, String bangpai, String waihao) {
        this(name, age, bangpai);//其他的构造方法
        this.waihao = waihao;
    }
    public static void main(String[] args) {
    }
}
```



###### 3、访问权限

* 包 、导包
  1. 包：文件夹系统，package声明
  2. 导包：import
  3. 不需要导包：相同文件夹下、java.lang下
* public      所有地方可以用
* default    包的访问权限
* private    私有的只有自己可以用的
* private  +  (public+getter/setter)方法     保护成员变量不被随意赋值

###### 4、继承

* 特点：子类可以拥有夫类中所有非私有的能力
* ***super***关键字：默认没有super时先找子类，没有的时候再找夫类中的
  1. super:父类中的内容
  2. this:自己类中的内容
  3. 原理：
* 方法的重写/覆盖：方法参数完全相似

###### 5、多态

* 特点：同一个对象有多个形态

* 作用：不同的数据类型 进行统一

* 向上转型：会屏蔽子类中的特有方法，所以需要向下转型

* 向下转型（强制转换）

  ~~~java
  //猫
  public class Cat extends Animal {
      public void eat(){
          System.out.println("猫吃鱼");
      }
      public void catchMouse(){
          System.out.println("猫抓老鼠");   //猫特有的能力
      }
  }
  //狗
  public class Dog extends Animal{
      public void eat(){
          System.out.println("狗吃骨头");
      }
  }
  //动物
  public class Animal {
      public void eat(){
          System.out.println("动物在吃");
      }
  }
  
  //人
  public class Person {
      public void feed(Animal an){  //把不同的数据类型进行统一
          an.eat();
      }
  }
  //client
  public class client {
      public static void main(String[] args) {
          //把猫看成动物
          //子类的对象赋值给夫类的引用（变量）
          //向上转型
         Animal a1 =  new Cat(); 
         Animal a2 =  new Dog();
         Person p = new Person();
         p.feed(a1);
         p.feed(a2);
        
        //把动物转换为猫
        //向下转型
          Cat cc = (Cat) a1; //转上去后，屏蔽了猫的catchMouse的能力，所以需要转回来
          cc.catchMouse();
      }
  }
  ~~~

* ***final***

  1. 变量不能改变
  2. 方法不能重写
  3. 类不能继承

###### 6、抽象和接口

###### ***抽象类：只声明，不实现***

* 特点：
  1. 方法是抽象方法，类必须是抽象类，抽象方法不可以有方法体
  2. 抽象类不能创建对象
  3. 抽象类的子类必须重写夫类中的抽象方法，否则，子类也必须是抽象类
  4. 通过抽象类可以强制要求子类必须有哪些方法，起到规范的作用
  5. 抽象类可以有正常的方法

###### ***接口***

* 特点：

  1. 接口中所有的方法都是抽象的方法。不可以有正常方法，可以省略abstract
  2. 接口中所有的内容都是公开的，公共的  可以省略public
  3. 类只能继承一个类，但可以实现多个接口

  

###### 7、内存分析



###### 8、常识

* 成员变量初始值：

  1. 先声明 、再赋值 、才能用
  2. 但是在Java中会默认赋值

* object

  * 所有类都要继承
  * 一些方法 equals、tostring、 instanceof
  * equals  和  ==  
  1. equals 可以自己写判断逻辑。            new String（） 类中重写了 equals 判断相同的字符
  2. == 是对象地址是否相同，字符串除外特例。
  2. instanceof 判断对象是什么类型

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

