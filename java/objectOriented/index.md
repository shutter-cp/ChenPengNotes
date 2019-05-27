**ChenPengNotes**

# 1. 面向对象简介
> 面向对象是一种编程思想
## 1.1 与面向过程区别
- 面向过程
遇到一个问题，亲力亲为的一步步解决他（小兵），典型代表语言：c    
- 面向对象    
遇到一个问题，找具有解决这个问题的对象，调用这个对象的方法（老板），典型代表java   

内存分配情况：
```java
class Demo{
    public static void main(String[] args){
        Person person = new Person();
        person.name = "张三";
        person.country = "中国";
        Person person2 = new Person();
        person.name = "李四";
        person.country = "台湾";
        person.sleep();
        person2.sleep();
    }
}
class Person{
    String name;
    static String country;
    public void sleep(){
        System.out.println(name+"======"+country);
    }
}
```
图片如下：
![](https://upload-images.jianshu.io/upload_images/15992215-13adf1383238540e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 1.2. 封装工具类
- 方法都用static
- 开头写一个构造方法  private Demo(){}

## 1.3. 代码块
- 构造代码块
```
{
    ......
}
```
- 静态代码块
```
static{
    .......
}
```
- 特点
    - 使用一个类时，这个类中的静态代码块会自动执行
    - 只会执行一次，第一次使用该类，就立刻执行
    - 在同一个类中，静态代码块的优先级很高，比构造方法优先，比main方法优先

## 1.4. 继承
### 1.4.1 优点
1. 提供了代码的复用性
2. 提高代码的可维护性

### 1.4.2 缺点
增加了类的耦合性    
- 耦合        
类与类的关系
- 内聚    
自己完成某件事情的能力     

### 1.4.3 注意事项
1. 子类不能继承父类的构造方法，可以通过super去访问
2. 不要为了部分功能而去继承，继承是一种关系的提现
3. 子类的构造方法默认会访问父类中的空参构造方法；因为每一个构造方法的第一句默认都是 super（）；因为子类继承父类就是需要父类中的数据
4. 如果父类有有参构造，没有空参构造；子类中还是会在构造方法中加一条super（）；父类不会自动产生无参构造，那么就会报错，
    1. 在子类手动写一个super（参数）；
    2. 在子类无参构造写一个this（参数）来调用子类是有参构造
    3. this（）和super（）不能同时存在
    4. 不管是用过父类new子类，还是子类new子类  调用的方法都是子类中重写的

### 1.4.4 执行过程
```java
class Fu {
    public int num = 10;
    public Fu(){
         System.out.println("fu");
    }
}
class Zi extends Fu{
    public int num = 20;
    public Zi(){
        System.out.println("zi");
    }
    public void show(){
        int num = 30;
        System.out.println(num);
        System.out.println(this.num);
        System.out.println(super.num);
    }
}

class Test{
    public static void main(String[] args){
        Zi zi = new Zi();
        zi.show();
    }
}

class Fu1 {
    static {
         System.out.println("静态代码块Fu");
    }
    {
         System.out.println("构造代码块Fu");
    }
    public Fu1() {
         System.out.println("构造方法Fu");
    }
}
class Zi1 extends Fu1 {
    static {
        System.out.println("静态代码块Zi");
    }
    {
        System.out.println("构造代码块Zi");
    }
    public Zi1() {
        System.out.println("构造方法Zi");
    }
}
class Test1{
    public static void main(String[] args){
        Zi1 zi1 = new Zi1();
    }
}
```

1. jvm调用了main方法，main先进栈
2. 遇到new子类
    1. 将父类.class和子类.class分别加载进内存
    2. 创建对象
    3. 当父类.class加载进内存父类的静态代码块会随着父类.class一起加载
    4. 子类静态代码块会随着子类.class一起加载进内存
3. 开始走子类的构造方法，因为java中是分层初始化的，先初始化父类，再初始化子类，所以先走父类
4. 父类初始化结束，子类开始初始化

## 1.5 接口与抽象类
### 1.5.1 相同点
1. 都不能创建对象
2. 都是作为父类/接口
3. 子类/实现都必须重写方法，然后才能创建对象

### 1.5.2 不同点
1. 抽象类用关键字abstract接口用关键字interface
2. 抽象类中可以有抽象方法，可以没有抽象方法，也可以部分是抽象方法
3. 接口中只有方法，都必须是抽象的
4. 抽象类可以定义任意成员变量，接口的成员变量必须加上public static final修饰
5. 类和抽象类之间的关系是单继承，类和接口的关系是多实现
6. 思想上的区别
    1. 抽象类中必须定义整个继承体系的共性内容
    2. 接口中定义整个继承体系同之外的额外的扩展的功能
7. 抽象类为部分方法提供实现，避免了子类重复实现这些方法，提高代码重用性

抽象类为继承体系中的共性内容，接口为继承体系外的扩展功能

### 1.5.3 二者选用
1. 优先选用接口，尽量少使用抽象类     
2. 需要定义子类的行为，又要为子类提供共性功能时才轩选用抽象类

### 1.5.4 多态
1. 前提
    1. 必须有子父类关系（必须有继承）
    2. 必须有方法的重写
2. 表现形式     
父类类型    变量名   = new 子类类型（）；
3. 注意事件
    1. 成员变量编译和运行都是使用的父类的
    2. 成员方法编译时看父类，运行时看子类
4. 缺点   
    多台只能调用子父类共有的方法，不能调用子类特有的方法
5. 优点
提高了程序的灵活性   
    1. 父类类型的变量可以接受任何一个子类的对象
    2. 编译时看父类，运行时看子类
    
6. 解决不能调用子类特有方法     
向上向下转型
1. 强制转换
2. instanceof运算符  可以判断是不是某个类型

### 1.5.5 匿名内部类
1. 作用  
快速创建抽象类的子类对象，接口实现类对象

```java
public abstract class AbstractAnimal {
    public abstract void eat();
    public abstract void speeack();
}
```
利用内部类快速创建抽象类的子类
```
public static void absTest(){
    Dog dog = new Dog();
    dog.eat();
    dog.speeack();

    new AbstractAnimal(){
        @Override
        public void eat() {
            System.out.println("不好吃");
        }
        @Override
        public void speeack() {
            System.out.println("哈哈哈");
        }
    };
    //匿名内部类
    new AbstractAnimal(){
        @Override
        public void eat() {
            System.out.println("不好吃");
        }
        @Override
        public void speeack() {
            System.out.println("哈哈哈");
        }
    }.eat();

    //利用多态 父类对象接受子类对象
    AbstractAnimal absAnimal = new AbstractAnimal() {
        @Override
        public void eat() {
            System.out.println("一般般");
        }
        @Override
        public void speeack() {
            System.out.println("对的");
        }
    };
    absAnimal.eat();
    absAnimal.speeack();
}
```

利用内部类快速创建接口的子类

```
public interface NuNi {
    void cook();
    void daoShui();
    void moMo();
}
```

```
public static void infTest(){
    new NuNi(){
        @Override
        public void cook() {
        }
        @Override
        public void daoShui() {
        }
        @Override
        public void moMo() {
        }
    };
    new NuNi(){
        @Override
        public void cook() {
        }
        @Override
        public void daoShui() {
        }
        @Override
        public void moMo() {
        }
    }.cook();
    NuNi nn = new NuNi() {
        @Override
        public void cook() {
        }
        @Override
        public void daoShui() {
        }
        @Override
        public void moMo() {
        }
    };
    nn.cook();
    nn.daoShui();
    nn.moMo();
}
```

### 1.5.6 内部接口
```java
public interface InternalInterface {
    void showOut();
    interface InInterface{
        void showIn();
    }
}

class InfDemo implements InternalInterface{
    @Override
    public void showOut() {
    }
}

class Inf2Demo implements InternalInterface.InInterface{
    @Override
    public void showIn() {
    }
}
```
### 1.5.7 内部类
```java
public class InternalClass {
    private String name;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }

    class IntClass{
        private String age;
        public String getAge() {
            return age;
        }
        public void setAge(String age) {
            this.age = age;
        }
    }
}


class IntClassDemo extends InternalClass{

}

class Int1ClassDemo extends InternalClass.IntClass {

}
```

### 1.5.8 static
1. 存在方法区中的静态区，只有一个空间
2. 静态是优先于对象存在的
3. 如何访问static成员
    1. 对象名.静态成员；//不建议
    2. 类名.静态成员;//建议

### 1.5.9 finally
1. final修饰类：不能被继承（没有子类），但是可以是其他类的子类
2. 修饰方法（最终方法，牛逼）：子类不能重写
3. 修饰成员变量
    1. 这个成员变量在创建对象之前必须初始化
    2. 只能赋值一次
4. 修饰局部变量
    1. 基本类型     
    该变量只能赋值一次（常量）
    2. 引用类型
        1. 改引用类型的变量的地址不能改变
        2. 地址指向空间中的内容可以改变
