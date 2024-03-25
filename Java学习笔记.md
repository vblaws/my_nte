# Java学习

## P157-常用API学习-System

System是一个工具类，提供一些与系统相关的方法
|方法名|说明|
|:-:|:-:|
|pulic static void exit(int status)|终止当前Java虚拟机|
|public static long currentTimeMills|返回当前系统的毫秒值形式|
|public static void arraycopy(数据源数组,起始索引,目的地数组,起始索引,拷贝个数)|数组拷贝|

计算机时间原点:**1970年1月1日00:00:00**,我国在东八区,有八个小时时差,所以应该是**1970年1月1日00:08:00**

> public static void exit(int status),参数status，0为正常退出,非0是不正常推出

> public static long currentTimeMills()这个方法可以用它来计算函数的执行时间,下面是一个例子

```java
long start = System.currentTimeMillis();
        for (int i = 1; i < 1000; i++) {
            boolean result = isPrime1(i);
            if (result) {
                System.out.println(i);
            }
        }
        long end = System.currentTimeMillis();
        System.out.println("程序执行时间" + (end - start));

        long start1 = System.currentTimeMillis();
        for (int i = 1; i < 100; i++) {
            boolean result = isPrime2(i);
            if (result) {
                System.out.println(i);
            }
        }
        long end1 = System.currentTimeMillis();
        System.out.println("程序执行时间" + (end1 - start1));

    }

    public static boolean isPrime1(int num) {
        for (int i = 2; i < num; i++) {
            if (num % i == 0) {
                return false;
            }
        }
        return true;
    }

    public static boolean isPrime2(int num) {
        for (int i = 2; i < Math.sqrt(num); i++) {
            if (num % i == 0) {
                return false;
            }
        }
        return true;
    }
```

这段代码是用来检测这两个判断是否为质数的执行速度的

> public static void arraycopy(数据源数组,起始索引,目的地数组,起始索引,拷贝个数)

```java
        // 如何使用System.arraycopy
        //基本数据类型
        int[] arr1 = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
        int[] arr2 = new int[10];
        //参数一:要拷贝的源数组
        //参数二:拷贝的索引
        //参数三:目标数组
        //参数四:目标数组索引
        //参数五:需要复制的个数
        System.arraycopy(arr1, 0, arr2, 0, 10);

        for (int arr22 : arr2) {
            System.out.println(arr22);
        }
        
        //引用数据类型
        Student s1 = new Student("wx", 12);
        Student s2 = new Student("ww", 22);
        Student s3 = new Student("wa", 99);

        Student[] stuArr = { s1, s2, s3 };
        Person[] perArr = new Person[3];
        System.arraycopy(stuArr, 0, perArr, 0, 3);
        for (Person person : perArr) {
            System.out.println(person.getAge() + "," + person.getName());
```

> 如果数据源数组和目的地数组都是基本数据类型,那么就需要保持两者的类型相同,否则就会报错例如:int[]不能拷贝到double[]中
>如果数据源数组和目的地数组都是引用数据类型,那么子类对象可以赋值给父类类型,

## P158-常用API学习-Runtime

<font size=5>Runtime</font>
//这是一个修改，这是第二个
>Runtime表示当前虚拟机的运行环境

这个类对象不能自己new,需要使用方法获取

|方法名|说明|
|:-:|:-:|
|public static Runtime getRuntime()|获取系统的运行环境对象|
|public void exit(int status)|停止虚拟机,跟上面的System.exit(int staus)相同|
|public int availableProcessors()|获取CPU线程数|
|public long maxMemory()|JVM虚拟机从系统中获取的总内存大小(单位byte)|
|pulic long totalMemory()|JVM虚拟机已经获取的内存大小(单位byte)|
