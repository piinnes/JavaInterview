## final, finally, finalize 的区别

### final

用于声明属性，方法和类， 分别表示属性不可变，方法不可覆盖， 类不可继承。

### finally

Java 中的 Finally 关键字一般与try一起使用，在程序进入try块之后，无论程序是因为异常而中止或其它方式返回终止的，finally块的内容一定会被执行 。通常finally里适合存放释放资源、后续处理的代码。

### finalize

是Object类的一个方法，在垃圾收集器执行的时候会调用被回收对象的此方法，可以覆盖此方法提供垃圾收集时的其他资源回收，例如关闭文件等。 JVM不保证此方法总被调用。并且finalize()只会在对象内存回收前被调用一次。