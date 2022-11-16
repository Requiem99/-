## 类的加载机制



![image-20210824165355629](image/image-20210824165355629.png)

### 类的加载

**静态加载**：编译时加载相关的类，如果没有则报错，有很强的依赖性。

**动态加载**：运行时加载需要的类，如果运行时不用改类，即使不存在该类，也不会报错，降低了依赖性。

```java
public Class ClassLoadTest{
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        String key= scanner.next();
        switch(key){
               case "1":
                    Dog dog = new Dog();//静态加载；编译时如果不引入 Dog类就会报错
                    dog.cry();
               break;
               case "2":
                    Class cls = Class.forName("Person");//通过反射动态加载。即使Person类不存在，编译时也不会报错，但是运行时如果类不存在就会报错
                	Person person = (Person) cls.newInstance();
                    Method method = person.getMethod("hello"),
                    method.invoke(person);
                break;
        }
       
    }
}
```

**类加载的时机**

1. 当创建对象时（new) // 静态加载

2. 当子类被加载时，父类也加载// 静态加载

3. 调用类的静态成员时// 静态加载

4. 通过反射// 动态加载

### 类加载的过程

![](C:\Users\邓文钢\AppData\Roaming\Typora\typora-user-images\image-20210821103126819.png)

![image-20210821103217530](C:\Users\邓文钢\AppData\Roaming\Typora\typora-user-images\image-20210821103217530.png)

#### 类的加载阶段分为三个过程  

**加载 (loading)  -->  连接(Linking)  -->  初始化(initialization)**

1. **加载阶段**(loading)

​        **JVM**在该阶段的主要目的是将字节码从不同的数据源（可能是class文件，也可能是jar包，甚至是网络）转化未==二进制字节流加载到内存中==并生成一个代表该类的java.long.Class对象。

2. **连接阶段**(Linking)

   连接阶段又分为：验证(verification); 准备(Preparation); 解析(Resolution) 三个子阶段。

   - **验证**

     2. > 1. 目的是为了确保Class文件的**字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机自身的安全**。
        >
        > 2. 包括：文件格式验证(是否以魔数 oxcafebabe开头)、元数据验证、字节码验证和符号引用验证。
        > 3. 可以使用 -Xverify:none 参数来关闭大部分的类验证措施，缩短虚拟机类加载的时间        。

     ​         

   - **准备**

     > 1. JVM会在该阶段对静态变量，分配内存并默认初始化(对应数据类型的默认初始化，如0，0L，null，false等)。这些变量所使用的内存都会在方法区分配。
     >
     >    ```java
     >    Class A {
     >       //属性--成员变量--字段；在大多数情况下都是一样的。
     >        public int n1 = 10;// n1是实例属性，不是静态变量，因此在准备阶段，是不会分配内存的。
     >        public static int n2 = 20;// n2是静态变量，分配内存 n2 是默认初始化0，而不是20。
     >        public static final n3 = 30;// n3是static final常量，他和静态变量不一样，因为一旦赋值就不变n3=30。
     >    }
     >    ```

   - **解析**

     > 1. 虚拟机将常量池的符号引用替换为直接引用
     >
     >    符号引用在把字节码文件放入内存前 类似于 A要引用B，只是名字上显示 A引用B；
     >
     >    直接引用就是把字节码文件放入内存后，有了地址，把名字上的引用，变为A的内存地址引用B的内存地址

3. **初始化阶段**(initialization)

   >1. 到初始化阶段，才真正执行类中定义的Java代码此阶段是执行<clinit>()方法的过程。
   >2. <clinit>()方法是由编译器按语句在源文件中出现的顺序，依此自动收集类中的所有==静态变量的赋值动作==和==静态代码块中的赋值语句==，并进行整和。
   >
   >3. 虚拟机会保证一个类的<clinit>()方法在多线程环境中被正确地加锁、同步；如果多个多线程同时去初始化一个类，那么只有一个线程去执行这个方法，其他线程都要阻塞等待，直到活动线程执行<clinit>()方法完毕。

**Java中类加载执行顺序是：**
主类中的静态代码块–>父类中的静态成员和静态代码块–>子类中的静态成员和静态代码块–>父类中的成员变量和构造代码块–>父类中的构造方法–>子类中的成员变量和构造代码块–>子类构造方法

# IO流

## 抽象基类

### 流的三种分类方式

**流向：**输入流、输出流

**数据单位：**字节流、字符流

**流的角色：**节点流(文件流)、处理流(比如 缓冲流)



**4个抽象基类**InputStream，OutputStream，Reader，Writer

OutputStream

`public void write(byte[] b)`：将 b.length个字节从指定的字节数组写入此输出流。

InputStream

`public void read(byte[] b)`：将输入流中的数据写入字节数组中



**字符流`专门用于处理文本`文件。如果处理纯文本的数据优先考虑字符流，其他情况就只能用字节流了**



### 文件字节输入流FileInputStream

### 文件字节输入出流FileOutputStream





### 文件字符输入流FileReader

### 文件字符输出流FileWriter



## 缓存流

### 缓冲字节输入流BufferedinputStream

- `public BufferedInputStream(InputStream in)` ：创建一个新的缓冲输入流，注意参数类型为**InputStream**。

### 缓冲字节输出流BufferedOutputStream

- `public BufferedOutputStream(OutputStream out)`： 创建一个新的缓冲输出流，注意参数类型为**OutputStream**。





### 缓冲字节输入流BufferedReader

### 缓冲字节输出流BufferedWriter



## 转换流

### 字节流转换为字符流

* 转换输入流  (转换必须要保持编码格式一致，不然会出现中文乱码)
* InputStreamReader



### 字节流转换为字符流

* 转换输出流  (转换必须要保持编码格式一致，不然会出现中文乱码)
* OutputStreamReader



## 对象流

Java 提供了一种对象序列化的机制。用一个字节序列可以表示一个对象，该字节序列包含该对象的数据、对象的类型和对象中存储的属性等信息。字节序列写出到文件之后，相当于文件中持久保存了一个对象的信息。

反之，该字节序列还可以从文件中读取回来，重构对象，对它进行反序列化。对象的数据、对象的类型和对象中存储的数据信息，都可以用来在内存中创建对象。看图理解序列化：

![image-20210916095957110](image/image-20210916095957110.png)





==InputStream和Reader是接收数据（读），OutputStream和Writer是发送数据（写）。==

# Thread多线程

## 继承Thread类的方式实现多线程

重写run方法

```java
public class TestThread extends Thread {
    @Override
    public void run() {
        System.out.println("多线程运行的代码");
        for(int i=0;i<5;i++){
            System.out.println("这是多线程的逻辑代码"+i);
        }
    }
}


```

```java
public class Test01 {
    public static void main(String[] args) {
        Thread t0= new TestThread();
        t0.start();//启动线程
        System.out.println("++++++++++");
        System.out.println("++++++++++");
        System.out.println("++++++++++");
    }
```

 main方法执行t0.start之后，就相当于在main方法之外开辟了一条支流
 在t0.start之后的main方法的其他代码就与run中的代码无关了，
 是并行运行的；各走各的了。

 如果拆开来看，run()中的代码和main之后的代码是保存各自的执行顺序的

 这就是多线程的异步性
 同步性是严格按照代码从上到下执行

## 实现Runnable接口实现多线程

实现run()方法

```java
public class TestRunable implements Runnable{
    @Override
    public void run() {

            System.out.println(Thread.currentThread().getName()+"  Runnable多线程运行的代码");
            for(int i=0;i<5;i++){
                System.out.println("Runnable 这是多线程的逻辑代码"+i);
        }
    }
}
```



```java
public class Test01 {
    public static void main(String[] args) {
        /*Thread t0= new TestThread();
        t0.start();//启动线程*/

        Thread t3 = new Thread(new TestRunable());

        Thread t4 = new Thread(new TestRunable(),"d4c");//参数1：实现Runable接口的类实例对象，参数2：线程名
        t3.start();
        t4.start();
        System.out.println("++++++++++");
        System.out.println("++++++++++");
        System.out.println("++++++++++");
    }
```



**一般使用实现接口(Runnable)的方式来实现多线程**

**好处**：1、避免了单继承的局限性

​            2、多个线程可以共享同一个接口实现类的对象，非常适合多个相同的线程来处理同一个资源

   **使用多线程的优点**

1.提高应用程序的响应。对图形化界面更有意义，可增强用户体验

2.提高CPU的利用率

3.改善程序结构。将既长又复杂的进程分为多个线程，独立运行，利于理解和修改 





```java
public class TestRunable implements Runnable{
    int count = 0;//设置变量count
    @Override
    public void run() {

            System.out.println(Thread.currentThread().getName()+"  Runnable多线程运行的代码");
            for(int i=0;i<5;i++){
                count++;
                System.out.println("Runnable 这是多线程的逻辑代码"+count);
        }
    }
}
```



```java
public class Test01 {
    public static void main(String[] args) {
        /*Thread t0= new TestThread();
        t0.start();//启动线程*/
        TestRunable testRunable  =new TestRunable();//实例化一个实现了Runable接口的对象

        Thread t3 = new Thread(testRunable);

        Thread t4 = new Thread(testRunable,"d4c");
        t3.start();
        t4.start();
    }
```

```java
Runnable 这是多线程的逻辑代码1
Runnable 这是多线程的逻辑代码2
Runnable 这是多线程的逻辑代码3
Runnable 这是多线程的逻辑代码5
Runnable 这是多线程的逻辑代码6
Runnable 这是多线程的逻辑代码4
Runnable 这是多线程的逻辑代码8
Runnable 这是多线程的逻辑代码9     //count++被两个线程执行了10次，count变量被两个线程共享了
Runnable 这是多线程的逻辑代码10
Runnable 这是多线程的逻辑代码7   
```



# 线程同步问题 

```
多线程调用这个方法，就有问题，线程资源共享时，一个线程在执行这个方法没有完毕时，另一个线程又开始执行这个方法
* 解决思路：先让一个线程整体执行完这个方法，再让另一个线程执行
* 通过synchronized同步锁来完成
* 可以直接在方法上加synchronized关键字
* 在普通方法上加同步锁，锁住的是整个对象，不是某一个方法,锁住之后，对象的所有方法都加是上了同一个同步锁
* 不同的对象就是不同是锁，普通方法加synchronized，线程使用不同的次方法的对象，还是有线程同步问题
*如果针对对象要加同步锁，那就加在方法上
* 如果针对某y一段代码需要加同步锁，就直接在代码块上加同步锁
*
**************************************************************************************
* 普通方法加同步锁synchronized，各种线程的操作只针对某同一个对象有用，如果再实例化一个对象的话
* 两个对象的同步锁就无法解决线程同步问题了。想要实例化的所有对象都同步锁，可以用静态方法加synchronized同步锁
```

# 线程通信

#### wait()和notify()，notifyAll()

**wait()**：令当前线程挂起并放弃CPU、同步资源，使别的线程可访问并修改共享资源，而当前线程排队等候再次对资源的访问。

**notify()**：唤醒正在排队等待同步资源的线程中优先级最高者，结束等待。

**notifyAll()**：唤醒正在排队等待同步资源的所有线程。

**Java.lang.Object**提供的这三个方法只有在**synchronized方法**或**synchronized代码块**中才能使用，否则会报异常

是属于Object的方法

### **生产者和消费者问题**

```java
/*
* 生产者和消费者
* */
public class Test {
    public static void main(String[] args) {
        Clerk c = new Clerk();
        new Thread(new Runnable() {
            @Override
            public void run() {
                synchronized (c) {
                    while (true) {//保持一直生产
                        if (c.productNum == 0) {//产品数为0，开始生产
                            System.out.println("产品数为0，开始生产");
                            while (c.productNum < 4) {
                                c.productNum++;
                                System.out.println("库存" + c.productNum);
                            }
                            System.out.println("产品数为" + c.productNum + "结束生产");
                            c.notify();//唤醒消费者线程
                        } else {
                            try {
                                c.wait();//生产者线程等待
                            } catch (Exception e) {
                                e.printStackTrace();
                            }
                        }

                    }
                }
            }
        }, "生产者").start();

        new Thread(new Runnable() {
            @Override
            public void run() {
                synchronized (c) {
                    while (true) {//保持一直消费
                        if (c.productNum == 4) {//产品数为4，开始消费
                            System.out.println("产品数为0，开始生产");
                            while (c.productNum > 0) {
                                c.productNum--;
                                System.out.println("库存" + c.productNum);
                            }
                            System.out.println("产品数为" + c.productNum + "结束消费");
                            c.notify();//唤醒生产者线程
                        } else {
                            try {
                                c.wait();//消费者线程等待
                            } catch (Exception e) {
                                e.printStackTrace();
                            }
                        }

                    }
                }
            }
        }, "消费者").start();
    }
}
 class Clerk{
    public static int productNum = 0;
}
```

