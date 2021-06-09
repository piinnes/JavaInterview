## 什么是Java泛型？

Java泛型（generics）是JDK 5中引入的一个新特性,泛型提供了编译时类型安全检测机制，该机制允许程序员在编译时检测到非法的类型。

泛型一般有三种使用方式:泛型类、泛型接口、泛型方法。

1. ### 泛型类

   ```java
   public class Box<T> {
    
     private T t;
    
     public void add(T t) {
       this.t = t;
     }
    
     public T get() {
       return t;
     }
    
     public static void main(String[] args) {
        Box<Integer> integerBox = new Box<Integer>();
        Box<String> stringBox = new Box<String>();
      
        integerBox.add(new Integer(10));
        stringBox.add(new String("Hello World"));
    
        System.out.printf("Integer Value :%d\n\n", integerBox.get());
        System.out.printf("String Value :%s\n", stringBox.get());
     }
   }
   ```

   编译以上代码，运行结果如下所示：

   ```java
   Integer Value :10
    
   String Value :Hello World
   ```

2. ### 泛型接口

   ```java
   public interface Generator<T> {
       public T method();
   }
   ```

   ```java
   class GeneratorImpl<T> implements Generator<T>{
       @Override
       public T method() {
           return null;
       }
   }
   ```

   ```java
   class GeneratorImpl implements Generator<String>{
       @Override
       public String method() {
           return "hello";
       }
   }
   ```

3. ### 泛型方法

```java
public class GenericMethodTest
{
   // 泛型方法 printArray                         
   public static < E > void printArray( E[] inputArray )
   {
      // 输出数组元素            
         for ( E element : inputArray ){        
            System.out.printf( "%s ", element );
         }
         System.out.println();
    }

    public static void main( String args[] )
    {
        // 创建不同类型数组： Integer, Double 和 Character
        Integer[] intArray = { 1, 2, 3, 4, 5 };
        Double[] doubleArray = { 1.1, 2.2, 3.3, 4.4 };
        Character[] charArray = { 'H', 'E', 'L', 'L', 'O' };

        System.out.println( "Array integerArray contains:" );
        printArray( intArray  ); // 传递一个整型数组

        System.out.println( "\nArray doubleArray contains:" );
        printArray( doubleArray ); // 传递一个双精度型数组

        System.out.println( "\nArray characterArray contains:" );
        printArray( charArray ); // 传递一个字符型型数组
    } 
}
```

编译以上代码，运行结果如下所示：

```java
Array integerArray contains:
1 2 3 4 5 6
 
Array doubleArray contains:
1.1 2.2 3.3 4.4
 
Array characterArray contains:
H E L L O
```

## 泛型在使用中的一些规则和限制

- 泛型的类型参数只能是类类型（包括自定义类），不能是简单类型。
- 同一种泛型可以对应多个版本（因为参数类型是不确定的），不同版本的泛型类实例是不兼容的。
- 泛型的类型参数可以有多个。
- 泛型的参数类型可以使用extends语句，例如<T extends superclass>。习惯上成为“有界类型”。
- 泛型的参数类型还可以是通配符类型。例如Class<?> classType = Class.forName(Java.lang.String);

## 什么是类型擦除？

JVM并不知道泛型的存在，因为泛型在编译阶段就已经被处理成普通的类和方法，处理机制是通过类型擦除。

在写代码时，无法把一个 String 类型的实例加到 ArrayList<Integer> 中，因为ArrayList<Integer> 和 ArrayList<String> 在编译的时候是完全不同的类型，但是运行结果却是true。这就Java泛型的类型擦除造成的。因为不管是 ArrayList<Integer> 还是 ArrayList<String>，在编译完成后都会被编译器擦除成了 ArrayList。

Java 泛型擦除是 Java 泛型中的一个重要特性，其目的是避免过多的创建类而造成的运行时的过度消耗。所以，想 ArrayList<Integer> 和 ArrayList<String> 这两个实例，其类实例是同一个。

```java
ArrayList<String> stringArrayList = new ArrayList<>();
ArrayList<Integer> integerArrayList = new ArrayList<>();
System.out.println(stringArrayList.getClass() == integerArrayList.getClass());
System.out.println(stringArrayList.getClass().getName() + "=========" + integerArrayList.getClass().getName());
```

运行结果

```java
true
java.util.ArrayList=========java.util.ArrayList
```

## **常用的通配符为： T，E，K，V，？**

- ？ 表示不确定的 java 类型
- T (type) 表示具体的一个 java 类型
- K V (key value) 分别代表 java 键值中的 Key Value
- E (element) 代表 Element