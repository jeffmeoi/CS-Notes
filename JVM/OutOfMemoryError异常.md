<!-- TOC -->

- [OutOfMemoryError异常](#outofmemoryerror异常)
    - [堆溢出](#堆溢出)
        - [测试代码](#测试代码)
        - [VM参数](#vm参数)
        - [运行结果](#运行结果)
            - [intelliJ IDEA添加vm参数的操作流程](#intellij-idea添加vm参数的操作流程)
        - [检查流程](#检查流程)
    - [虚拟机栈和本地方法栈溢出](#虚拟机栈和本地方法栈溢出)
        - [测试代码](#测试代码-1)
        - [VM参数](#vm参数-1)
        - [运行结果](#运行结果-1)
    - [方法区和运行时常量池溢出](#方法区和运行时常量池溢出)
    - [本机直接内存溢出](#本机直接内存溢出)

<!-- /TOC -->

# OutOfMemoryError异常

## 堆溢出

### 测试代码

```java
import java.util.ArrayList;
import java.util.List;

public class HeapOOM {
    static class OOMObject { }

    public static void main(String[] args) {
        List<OOMObject> list = new ArrayList<>();
        while(true){
            list.add(new OOMObject());
        }
    }
}
```

### VM参数

-Xms20m -Xmx20m -XX:+HeapDumpOnOutOfMemoryError

- -Xms20m 设定程序启动时占用内存大小为20m；

- -Xmx20m 设定程序运行期间最大可占用的内存大小；

- 通过参数-XX:+HeapDumpOnOutOfMemoryError可以让虚拟机在出现内存溢出异常时Dump出当前的内存堆转储快照以便事后进行分析。

### 运行结果

![image-20200521204135579](OutOfMemoryError%E5%BC%82%E5%B8%B8.assets/image-20200521204135579.png)

#### intelliJ IDEA添加vm参数的操作流程

- ![image-20200521203859294](OutOfMemoryError%E5%BC%82%E5%B8%B8.assets/image-20200521203859294.png)
- ![image-20200521203914220](OutOfMemoryError%E5%BC%82%E5%B8%B8.assets/image-20200521203914220.png)

### 检查流程

1. 打开Visual VM，jdk一般自带Visual VM，在jdk的bin目录下

   ![image-20200521204042960](OutOfMemoryError%E5%BC%82%E5%B8%B8.assets/image-20200521204042960.png)

2. 确认堆转储文件的生成地址

   ![image-20200521204135579](OutOfMemoryError%E5%BC%82%E5%B8%B8.assets/image-20200521204135579.png)

3. 在Visual VM中打开堆转储文件

   1. ![image-20200521204332459](OutOfMemoryError%E5%BC%82%E5%B8%B8.assets/image-20200521204332459.png)

   2. 点击红框里的main

      ![image-20200521204410264](OutOfMemoryError%E5%BC%82%E5%B8%B8.assets/image-20200521204410264.png)

   3. 点击红框里的类

      ![image-20200521204508921](OutOfMemoryError%E5%BC%82%E5%B8%B8.assets/image-20200521204508921.png)

      ![image-20200521204556896](OutOfMemoryError%E5%BC%82%E5%B8%B8.assets/image-20200521204556896.png)

   4. 发现造成OOM的对象是OOMObject，双击该对象，查看实例

      ![image-20200521204737738](OutOfMemoryError%E5%BC%82%E5%B8%B8.assets/image-20200521204737738.png)

   5. 通过对象信息(引用情况等)判断是发生了内存泄漏还是内存溢出

## 虚拟机栈和本地方法栈溢出

HotSpot虚拟机并不区分虚拟机栈和本地方法栈。（-Xoss参数设置本地方法栈大小，但是是无效的）

- 两种溢出情况
  - 如果线程请求的栈深度大于虚拟机所允许的最大深度，将抛出StackOverflowError异常
  - 如果虚拟机在扩展栈时无法申请到足够的内存空间，则抛出OutOfMemoryError异常

### 测试代码

```java
public class JavaVMSOF {
    private int stackLength = 1;
    public void stackLeak() {
        stackLength++;
        stackLeak();
    }

    public static void main(String[] args) {
        JavaVMSOF javaVMSOF = new JavaVMSOF();
        try {
            javaVMSOF.stackLeak();
        } catch (StackOverflowError e) {
            System.out.println("stack length: " + javaVMSOF.stackLength);
        }
    }
}
```

### VM参数

-Xss128k 设定每个线程的堆栈大小；

### 运行结果

![image-20200521205945645](OutOfMemoryError%E5%BC%82%E5%B8%B8.assets/image-20200521205945645.png)

## 方法区和运行时常量池溢出

在Java1.6及之前的版本，字符串常量池在方法区中，因此当对方法区大小进行限制并且无限的使用String.intern()发生溢出时，提示的信息是OOM:PermGen space；

在Java1.7之后，字符串常量池移到了堆中，因此当对方法区大小进行限制并且无限的使用String.intern()时，不会发生方法区溢出。

可以使用CGLib无限的创建动态类，来实现溢出测试。

## 本机直接内存溢出

- 直接内存容量可以通过JVM参数-XX:MaxDirectMemorySize指定，如果不指定，则默认与Java堆最大值一样。