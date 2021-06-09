## hashCode()与 equals()

一些面试官可能会问你：“你重写过 `hashcode` 和 `equals`么，为什么重写 `equals` 时必须重写 `hashCode` 方法？”或者两个对象的 、`hashCdoe() `相同，它的 `equals()` 方法一定要返回 true，对吗？

### hashCode()介绍:

`hashCode()` 的作用是获取哈希码，也称为散列码；它实际上是返回一个 int 整数。这个哈希码的作用是确定该对象在哈希表中的索引位置。`hashCode()`定义在 JDK 的 `Object` 类中，这就意味着 Java 中的任何类都包含有 `hashCode()` 函数。另外需要注意的是： `Object` 的 hashcode 方法是本地方法，也就是用 c 语言或 c++ 实现的，该方法通常用来将对象的 内存地址 转换为整数之后返回。

```java
public native int hashCode();
```

散列表存储的是键值对(key-value)，它的特点是：能根据“键”快速的检索出对应的“值”。这其中就利用到了散列码！（可以快速找到所需要的对象）

### equals()介绍：

[==和 equals 的区别](https://github.com/piinnes/JavaInterview/blob/master/docs/basics/==和 equals 的区别.md)

### 为什么要有 hashCode？

**equals() 会有力不从心的时候**

Set 和 Map 不存放重复的元素（key），这些容器在存储元素的时必须对元素做出判断：**在当前的容器中有没有和新元素相同的元素？**

你可能会想：这容易呀，直接调用元素对象的 equals() 方法进行比较不就行了吗？

果容器中的存储的对象数量较少，这确实是个好主意，但是如果容器中存放的对象达到了一定的规模，要调用容器中所有对象的 equals() 方法和新元素进行比较，就不是一件容易的事情了。

就算 equals() 方法的比较逻辑简单无比，总的来说也是一个时间复杂度为 O(n) 的操作啊。

**hashCode() 小力出奇迹**

我们不妨假设**两个相同的对象，hashCode() 一定相同**，这么一来就体现出哈希函数的威力了。

由于相同的输入一定会产生相同的输出，于是如果新对象，和容器中已存在的对象相同，新对象计算出的哈希值就会和已存在的对象的哈希值产生冲突。

这时容器就能判断：这个新加入的元素已经存在，需要另作处理：覆盖掉原来的元素（key）或舍弃。

按照这个思路，如果这个元素计算出的哈希值所对应的内存单元没有产生冲突，也就是没有重复的元素，那么它就可以直接插入。

**所以当运用 hashCode() 时，判断是否有相同元素的代价，只是一次哈希计算，时间复杂度为O(1)**，这极大地提高了数据的存储性能。

### 为什么重写 `equals` 时必须重写 `hashCode` 方法？

我们以`HashSet`举例说明，我们认为`Person`对象的名字和年纪相同就代表是同一个人。现在我们重写`equals()`看看运行结果如何。

```java
public class Person {
    private String name;
    private Integer age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public Person(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return Objects.equals(name, person.name) &&
                Objects.equals(age, person.age);
    }

//    @Override
//    public int hashCode() {
//        return Objects.hash(name, age);
//    }
}
```

```java
HashSet<Person> personHashSet = new HashSet<Person>();
personHashSet.add(new Person("张三",22));
personHashSet.add(new Person("张三",22));
System.out.println(personHashSet);
```

运行结果：

```java
[Person{name='张三', age=22}, Person{name='张三', age=22}]
```

集合是没有当作相同对象处理的，因为`HashSet`首先是通过对象的hashcode来判断对象是否相同（因为hashcode相同可能对象还是不相同的），这时会调用 `equals` 方法来检查 hashcode 相等的对象是否真的相同。如果两者相同，`HashSet` 就不会让其加入操作成功。

接着我们把`hashcode`的注释打开在看看运行结果。

运行结果：

```java
[Person{name='张三', age=22}]
```

这下就只有一个人了。所以重写 `equals` 时必须重写 `hashCode` 方法。

### **为什么两个对象有相同的 hashcode 值，它们也不一定是相等的？**

因为 `hashCode()` 所使用的哈希算法也许刚好会让多个对象传回相同的哈希值。这就是所谓的hash碰撞。